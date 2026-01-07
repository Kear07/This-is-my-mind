-----------
### O Problema: Java vs. SQL

* **Java (OO):** Pensa em Objetos (ex: `class Usuario`, `class Pedido`).
* **SQL (Relacional):** Pensa em Tabelas (ex: `TABLE usuarios`, `TABLE pedidos`).

Esses dois mundos não se falam naturalmente. O processo de "tradução" manual (pegar um objeto `Usuario` e escrever `INSERT INTO usuarios (nome, email)...`) é chato, repetitivo e fonte de muitos erros.

-------
### A Solução: ORM (Object-Relational Mapping)

**ORM** é uma técnica que "mapeia" automaticamente seus Objetos Java para Tabelas do banco de dados.

* **JPA (Java Persistence API):** É a **especificação** (a "interface" ou o "contrato"). É um conjunto de regras (em forma de anotações como `@Entity`, `@Id`) que define *como* deve ser feito um ORM em Java.
* **Hibernate:** É a **implementação** (a "classe"). É a biblioteca *real* que faz o trabalho sujo de transformar seu objeto Java em comandos SQL, seguindo as regras da JPA.
* **Spring Data JPA:** É a **camada de abstração** do Spring. Ele fica *em cima* do Hibernate e torna seu uso ainda mais fácil. Ele te dá "super-poderes", como criar consultas complexas apenas pelo nome de um método.

Quando você usa `spring-boot-starter-data-jpa`, você está, na verdade, importando:
**Spring Data JPA** -> que usa **JPA** -> que usa **Hibernate** -> que fala **SQL** com o banco.

---
### Configuração (application.yml)

Primeiro, diga ao Spring onde está seu banco.

```yaml
# src/main/resources/application.yml

spring:
  datasource:
    # O "dialeto" JDBC para o PostgreSQL
    url: jdbc:postgresql://localhost:5432/meu_banco_de_dados
    username: kear
    password: minha_senha_secreta
    # O driver (o "tradutor" Java <-> Postgres)
    driver-class-name: org.postgresql.Driver
  
  jpa:
    # Isso mostra o SQL que o Hibernate está gerando no console
    show-sql: true
    properties:
      hibernate:
        # Formata o SQL no console para ficar legível
        format_sql: true
    
    # MUITO IMPORTANTE: DDL-Auto
    # Define o que o Hibernate deve fazer com suas tabelas ao ligar a API
    # --------------------------------------------------------------------
    # 'create': Apaga tudo e cria do zero. (Bom para testes)
    # 'create-drop': Apaga, cria e destrói ao desligar. (Bom para testes)
    # 'update': Tenta alterar as tabelas existentes para bater com suas Entidades. (Bom para DEV)
    # 'validate': Verifica se as tabelas batem com as Entidades, se não, dá erro. (Bom para PROD)
    # 'none': Não faz nada. (Recomendado para PROD)
    hibernate:
      ddl-auto: update
```

------
### A Entidade (`@Entity`)

Esta é a classe Java que será mapeada para uma tabela.

```java
package com.kear.meuprojeto.model;

import jakarta.persistence.*; // Importa as anotações JPA

// 1. @Entity: Diz ao JPA/Hibernate: "Esta classe é uma tabela"
@Entity
// 2. @Table: (Opcional) Permite customizar o nome da tabela no banco
@Table(name = "tb_usuarios") 
public class Usuario {

    // 3. @Id: Marca qual atributo é a Chave Primária (PK)
    @Id
    // 4. @GeneratedValue: Diz como a PK será gerada.
    //    'IDENTITY' usa o 'AUTO_INCREMENT' (serial) do próprio banco.
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // 5. @Column: (Opcional) Customiza a coluna
    @Column(name = "nome_completo", length = 100, nullable = false)
    private String nome;

    @Column(unique = true, nullable = false)
    private String email;

    @Column(nullable = false)
    private String senha;
    
    // Construtores, Getters e Setters são necessários para o JPA
    // (Omitidos por brevidade)
}
```

------
### O Repositório (`JpaRepository`)

Esta é a **mágica** do Spring Data JPA. Em vez de escrever o código de `INSERT`, `SELECT`, `UPDATE` (o DAO), você apenas cria uma **interface**.

```
package com.kear.meuprojeto.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

// 1. @Repository: Marca como um Bean de acesso a dados
@Repository
// 2. Você "estende" JpaRepository
//    <Usuario, Long> significa:
//    - Esta interface gerencia a entidade "Usuario"
//    - O tipo da Chave Primária (PK) do "Usuario" é "Long"
public interface UsuarioRepository extends JpaRepository<Usuario, Long> {
    
    // (Veja a próxima seção)

}
```

**O que você ganha DE GRAÇA ao estender `JpaRepository`?** Você _automaticamente_ ganha métodos prontos (o Spring escreve o SQL para você):

- `save(Usuario usuario)`: (Cria ou Atualiza)
- `findById(Long id)`: (Busca um por ID)
- `findAll()`: (Busca todos)
- `deleteById(Long id)`: (Deleta por ID)
- `count()`: (Conta quantos existem)
- ...e muitos outros!

--------
### Query Methods (A "Outra Mágica")

E se você quiser buscar um usuário pelo email? Você **não precisa escrever SQL**. Você apenas _declara o método_ na sua interface e o Spring entende.

```java
// Dentro da interface UsuarioRepository...

public interface UsuarioRepository extends JpaRepository<Usuario, Long> {

    // O Spring Data JPA lê o nome do método e entende:
    // "Quero um 'find' (SELECT) 'By' 'Email'"
    // Ele automaticamente gera: "SELECT u FROM Usuario u WHERE u.email = ?"
    Optional<Usuario> findByEmail(String email);

    // Exemplos mais complexos:
    
    // "SELECT u FROM Usuario u WHERE u.nome = ? AND u.status = ?"
    List<Usuario> findByNomeAndStatus(String nome, String status);

    // "SELECT u FROM Usuario u WHERE u.idade > ? ORDER BY u.nome ASC"
    List<Usuario> findByIdadeGreaterThanOrderByNomeAsc(int idade);
}
```

A convenção de nomes (`FindBy...`, `And...`, `GreaterThan...`) é o que escreve o SQL para você.

------
### Exemplo: Usando o Repositório no Service

Agora, vamos conectar o `UsuarioService` (do arquivo 02) com o `UsuarioRepository`.

```java
@Service
public class UsuarioService {

    // Injetamos o REPOSITÓRIO (a interface mágica)
    private final UsuarioRepository usuarioRepository;

    @Autowired
    public UsuarioService(UsuarioRepository usuarioRepository) {
        this.usuarioRepository = usuarioRepository;
    }

    // O Controller (arquivo 03) chama este método
    public Usuario criar(Usuario usuario) {
        // ...regras de negócio...
        
        // Usamos o método 'save' que ganhamos de graça!
        return usuarioRepository.save(usuario);
    }

    public Usuario buscarPorId(Long id) {
        // Usamos o 'findById' (que retorna um Optional<T>)
        return usuarioRepository.findById(id)
            .orElseThrow(() -> new RuntimeException("Usuário não encontrado!"));
    }

    public Usuario buscarEmail(String email) {
        // Usamos o Query Method que criamos!
        return usuarioRepository.findByEmail(email)
            .orElseThrow(() -> new RuntimeException("Email não encontrado!"));
    }
}
```

----------

