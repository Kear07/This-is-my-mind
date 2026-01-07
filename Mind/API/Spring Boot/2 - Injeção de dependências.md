-----------------
### Os Problemas: Acoplamento e Controle

Vamos imaginar um código Java "normal" (sem Spring) para um sistema de pedidos:

```java
// Sem Spring
class PedidoService {
    
    // Problema 1: Acoplamento Forte
    // A classe PedidoService está DIRETAMENTE acoplada à classe PedidoRepository.
    // Ela é responsável por construir (dar 'new') sua própria dependência.
    private PedidoRepository repositorio = new PedidoRepository();

    public void salvarPedido(Pedido pedido) {
        // ...lógica de negócio...
        repositorio.salvar(pedido);
    }
}
```

Este código tem dois problemas graves:

1. **Acoplamento Forte:** `PedidoService` _conhece_ a implementação exata de `PedidoRepository`. E se quiséssemos trocar por um `PedidoRepositoryMock` (para testes) ou um `PedidoRepositoryPostgres`? Teríamos que _mudar o código_ da classe `PedidoService`.
    
2. **Gerenciamento Manual:** A `PedidoService` controla o ciclo de vida do `PedidoRepository`. E se o `PedidoRepository` precisasse de uma conexão com o banco (ex: `new PedidoRepository(conexao)`)? A `PedidoService` teria que gerenciar isso também.

-------
### A Solução: IoC e DI

#### a) Inversão de Controle (Inversion of Control - IoC)

**IoC** é o princípio de design. É a filosofia.

- **Tradução:** "Não me ligue, eu ligo para você." 
- **Conceito:** Em vez da sua classe (`PedidoService`) controlar e construir suas dependências (`new PedidoRepository()`), você **inverte o controle**. Um "contêiner" (o Spring) fica responsável por construir e gerenciar todos os objetos.
- **No Spring:** Esse contêiner é chamado de **ApplicationContext** ou **IoC Container**.

#### b) Injeção de Dependência (Dependency Injection - DI)

**DI** é a _implementação_ prática do princípio **IoC**. É _como_ o Spring aplica a Inversão de Controle.

- **Conceito:** Se a `PedidoService` precisa de um `PedidoRepository`, ela não vai mais dar `new`. Ela vai simplesmente _pedir_ por um no seu construtor ou em um atributo. O Spring (o IoC Container) verá esse "pedido" e **injetará** (fornecerá) uma instância pronta para uso.

-------
### "Beans": Os Tijolos do Spring

Quando você entrega o controle para o Spring, como ele sabe quais objetos ele deve gerenciar?

- **Bean:** Um **Bean** é simplesmente um objeto que é instanciado, montado e gerenciado pelo Spring IoC Container.

Para dizer ao Spring "Ei, gerencie este objeto para mim", você usa anotações.

--------
### Anotações de Componente (Estereótipos)

São anotações que marcam uma classe para que o `@ComponentScan` (que vimos no arquivo 01) a encontre e a transforme em um Bean gerenciado pelo Spring.

- **`@Component`**
    - A anotação mais genérica. Diz ao Spring: "Esta classe é um componente. Gerencie-a."
- **`@Service`**
    - Uma especialização do `@Component`.
    - Usada para marcar classes que contêm **lógica de negócio** (ex: `PedidoService`, `UsuarioService`). Semanticamente, é mais claro.
- **`@Repository`**
    - Uma especialização do `@Component`.
    - Usada para marcar classes que acessam dados (ex: `PedidoRepository`, `UsuarioRepository`).
    - **Bônus:** O Spring automaticamente traduz exceções específicas do banco de dados (como `SQLException`) para exceções genéricas do Spring (`DataAccessException`), tornando seu Service mais limpo.
- **`@Configuration`**
    - Uma especialização do `@Component`.
    - Usada para marcar classes que contêm definições de Beans feitas manualmente (usando o `@Bean`, que veremos depois).

----------
### Como Injetar Dependências: `@Autowired`

Uma vez que suas classes são Beans gerenciados, como elas "pedem" por outras?

- **`@Autowired`**: Esta anotação diz ao Spring: "Neste ponto, por favor, injete a dependência necessária."

Existem três formas de usar:

#### a) Injeção por Construtor (RECOMENDADA)

Esta é a **melhor prática absoluta** por várias razões:

1. **Imutabilidade:** As dependências são `final`, garantindo que não possam ser trocadas depois.
2. **Garantia:** O objeto _não pode_ ser construído sem suas dependências.
3. **Testabilidade:** Fica explícito o que a classe precisa. É muito fácil de testar, pois você pode dar `new` nela manualmente e passar "Mocks" (dependências falsas).

```java
// PedidoRepository.java
@Repository // 1. O Spring gerencia esta classe como um Bean
public class PedidoRepository {
    public void salvar(Pedido pedido) {
        // ...lógica de banco de dados...
    }
}

// PedidoService.java
@Service // 2. O Spring também gerencia esta classe
public class PedidoService {
    
    // 3. A dependência é declarada como 'final'
    private final PedidoRepository repositorio;

    // 4. A anotação @Autowired é colocada no construtor.
    // O Spring verá que este construtor precisa de um 'PedidoRepository'.
    // Ele vai procurar por um Bean desse tipo no Contêiner e "injetá-lo".
    @Autowired
    public PedidoService(PedidoRepository repositorio) {
        this.repositorio = repositorio;
    }

    public void salvarPedido(Pedido pedido) {
        // ...lógica de negócio...
        repositorio.salvar(pedido); // 5. A dependência está pronta para uso!
    }
}

/* * Bônus: Se sua classe tem APENAS UM construtor, o Spring é inteligente
 * o suficiente para saber que deve usá-lo para injeção. 
 * Nesses casos, a anotação @Autowired no construtor é OPCIONAL.
 */
```

#### b) Injeção por Setter (Menos comum)

Usada quando a dependência é opcional ou pode precisar ser trocada em tempo de execução.

```java
@Service
public class PedidoService {
    
    private PedidoRepository repositorio;

    @Autowired
    public void setRepositorio(PedidoRepository repositorio) {
        this.repositorio = repositorio;
    }
}
```

#### c) Injeção por Campo (NÃO RECOMENDADA)

Parece a mais fácil, mas é a **pior prática**.

- **Por quê?** Dificulta testes (você precisa usar "Reflection" para injetar mocks), esconde as dependências da classe e pode levar a problemas de `NullPointerException`.

```java
@Service
public class PedidoService {
    
    // Parece simples, mas é ruim para testes e clareza. EVITE.
    @Autowired
    private PedidoRepository repositorio;
    
    // ...
}
```

