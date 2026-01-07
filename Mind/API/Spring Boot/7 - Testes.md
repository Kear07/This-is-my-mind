---------------

Testar em Spring Boot é uma parte fundamental do desenvolvimento. O ecossistema facilita a criação de testes de unidade (rápidos e isolados) e testes de integração (completos e realistas).
### O Ponto de Partida: `spring-boot-starter-test`

Quando você cria um projeto no Spring Initializr, este "Starter" já vem incluído. Ele agrupa as 3 bibliotecas de teste mais importantes do mundo Java:

1.  **JUnit 5:** O framework padrão para *executar* os testes (ex: `@Test`).
2.  **Mockito:** A biblioteca para criar "Mocks" (objetos falsos). Essencial para *isolar* sua classe, fingindo suas dependências.
3.  **AssertJ:** A biblioteca para *verificar* os resultados (ex: `assertThat(resultado).isEqualTo("Kear")`). É mais legível que as asserções padrão do JUnit.

---------
### A Pirâmide de Testes no Spring

* **Testes de Unidade (Base Larga):** Testa UMA classe por vez, em *total isolamento*. (Ex: Testar o `UsuarioService` *sem* o `UsuarioRepository`). Rápido, barato, 90% dos seus testes.
* **Testes de Integração (Meio):** Testa como as camadas do Spring funcionam *juntas*. (Ex: Testar o `UsuarioController` *com* o Spring MVC, ou o `UsuarioRepository` *com* o banco H2). Lento, caro, 10% dos seus testes.
* **Testes E2E (Ponta):** Testa a aplicação inteira, do front-end ao banco. (Não coberto pelo Spring Boot).

---
### Testes de Unidade (com Mockito)

**Objetivo:** Testar a lógica de negócio no `UsuarioService` sem tocar no banco de dados.

**Ferramentas:** `JUnit 5` + `Mockito`.

```java
package com.kear.meuprojeto.service;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;
import static org.assertj.core.api.Assertions.*;

// 1. Diz ao JUnit 5 para ativar a extensão do Mockito
@ExtendWith(MockitoExtension.class)
class UsuarioServiceTest {

    // 2. @Mock: Cria um "dublê" (fake) do repositório.
    //    Ele não acessará o banco; apenas fingirá.
    @Mock
    private UsuarioRepository usuarioRepository;

    // 3. @InjectMocks: Cria uma instância REAL do UsuarioService,
    //    mas "injeta" os @Mocks (acima) dentro dele.
    @InjectMocks
    private UsuarioService usuarioService;

    @Test
    void deveSalvarUsuarioComSucesso() {
        // --- ARRANGE (Arrumação) ---
        // 1. Cria os dados de entrada
        UsuarioRequestDTO dto = new UsuarioRequestDTO("Kear", "kear@email.com");
        Usuario usuarioParaSalvar = new Usuario(dto); // (supõe um construtor)
        
        // 2. Define o comportamento do Mock:
        //    "QUANDO (when) o 'repository.save()' for chamado COM (any())
        //     ENTÃO (then) retorne (Return) o 'usuarioParaSalvar'"
        when(usuarioRepository.save(any(Usuario.class))).thenReturn(usuarioParaSalvar);

        // --- ACT (Ação) ---
        // 3. Executa o método que queremos testar
        UsuarioResponseDTO resultado = usuarioService.criar(dto);

        // --- ASSERT (Verificação) ---
        // 4. Verifica se o resultado é o esperado
        assertThat(resultado).isNotNull();
        assertThat(resultado.getNome()).isEqualTo("Kear");

        // 5. Verifica se o mock foi chamado corretamente
        //    "Verifique se o 'repository.save()' foi chamado 1 vez"
        verify(usuarioRepository, times(1)).save(any(Usuario.class));
    }
}
```

------------
### Testes de Integração (com Spring Boot)

Aqui, não usamos Mockito (ou usamos pouco). Queremos que o **Spring** carregue o contexto.

#### a) Fatia de Teste: `@DataJpaTest` (Testando o Repository)

**Objetivo:** Testar se o `UsuarioRepository` (JPA) está salvando e buscando no banco corretamente.

- **O que ele faz:** Carrega _apenas_ a camada de persistência (JPA, Hibernate, `@Repository`). 
- **Ambiente:** Por padrão, configura e usa um **banco de dados em memória (H2)**.
- **Importante:** Cada teste é `@Transactional` e dá _rollback_ (desfaz) no final, para que um teste não suje o próximo.

```java
package com.kear.meuprojeto.repository;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;
import org.springframework.boot.test.autoconfigure.orm.jpa.TestEntityManager;
import static org.assertj.core.api.Assertions.*;

// 1. Carrega apenas a "fatia" de teste do JPA
@DataJpaTest
class UsuarioRepositoryTest {

    // 2. O Spring injeta o repositório real
    @Autowired
    private UsuarioRepository usuarioRepository;
    
    // 3. (Opcional) TestEntityManager é um helper para inserir dados
    //    ANTES do repositório ser testado.
    @Autowired
    private TestEntityManager entityManager;

    @Test
    void deveEncontrarUsuarioPorEmail() {
        // --- ARRANGE ---
        Usuario usuario = new Usuario("Kear", "kear@email.com");
        entityManager.persistAndFlush(usuario); // Salva o usuário no banco H2

        // --- ACT ---
        Optional<Usuario> resultado = usuarioRepository.findByEmail("kear@email.com");

        // --- ASSERT ---
        assertThat(resultado).isPresent();
        assertThat(resultado.get().getNome()).isEqualTo("Kear");
    }
}
```

#### b) Fatia de Teste: `@WebMvcTest` (Testando o Controller)

**Objetivo:** Testar se o `UsuarioController` está recebendo requisições HTTP e retornando o JSON correto (sem tocar no Service real).

- **O que ele faz:** Carrega _apenas_ a camada Web (`@RestController`, `@ControllerAdvice`, Jackson, etc.).
- **O que ele NÃO faz:** **Não** carrega `@Service` ou `@Repository`.
- **A Ferramenta:** `MockMvc` (um "Postman" falso para disparar requisições HTTP).

```java
package com.kear.meuprojeto.controller;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;
import static org.mockito.Mockito.*;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

// 1. Carrega apenas a "fatia" Web, focada no UsuarioController
@WebMvcTest(UsuarioController.class)
class UsuarioControllerTest {

    // 2. Injeta o 'Postman' falso
    @Autowired
    private MockMvc mockMvc;

    // 3. @MockBean: Como o @WebMvcTest NÃO carrega o Service,
    //    precisamos "mockar" (fingir) a camada de serviço.
    @MockBean
    private UsuarioService usuarioService;

    @Test
    void deveBuscarUsuarioPorIdComSucesso() throws Exception {
        // --- ARRANGE ---
        UsuarioResponseDTO dto = new UsuarioResponseDTO(1L, "Kear", "kear@email.com");
        
        // Define o comportamento do Mock (Service)
        when(usuarioService.buscar(1L)).thenReturn(dto);

        // --- ACT & ASSERT ---
        // Executa um GET HTTP falso para /api/v1/usuarios/1
        mockMvc.perform(get("/api/v1/usuarios/1")
                .contentType(MediaType.APPLICATION_JSON))
                
                // Verifica se o status HTTP foi 200 OK
                .andExpect(status().isOk())
                
                // Verifica se o JSON de resposta bate
                .andExpect(jsonPath("$.id").value(1))
                .andExpect(jsonPath("$.nome").value("Kear"));
    }
}
```

-