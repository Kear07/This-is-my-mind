------------------------
### Mágica do Starter: Tomcat Embutido

Quando você adiciona o `spring-boot-starter-web` ao seu projeto, duas coisas mágicas acontecem (graças à auto-configuração):

1.  **Spring MVC é Configurado:** O Spring configura um "DispatcherServlet" e tudo o que é necessário para lidar com requisições web.
2.  **Servidor Tomcat é Embutido:** O Spring Boot baixa um servidor Tomcat e o configura *dentro* da sua aplicação. Quando você executa seu arquivo `.jar`, o próprio Java "liga" o Tomcat na porta definida (ex: 8080) e sua API fica no ar. Você não precisa mais "implantar" (deploy) um arquivo `.war` em um servidor externo.

------
### O Controlador: `@RestController`

O **Controlador** (Controller) é a classe Java que atua como o "porteiro" da sua API. É a camada que recebe as requisições HTTP e decide o que fazer com elas.

* `@Controller`: (Antigo) Anotação clássica do Spring MVC. Retornava nomes de páginas (ex: "home.jsp").
* `@ResponseBody`: Dizia ao `@Controller` para, em vez de retornar uma página, converter o retorno (ex: um objeto `Usuario`) em **JSON** e enviá-lo no corpo da resposta.
* **`@RestController`**: (Moderno) É simplesmente um atalho para **`@Controller` + `@ResponseBody`**. É a anotação que você **sempre** usará para criar APIs REST.

------------------
### Mapeando Requisições (Endpoints)

Usamos anotações para "mapear" um método Java a um "endpoint" (uma URL + um verbo HTTP).

### `@RequestMapping`
Define o prefixo (caminho base) para todos os métodos dentro da classe.

```java
@RestController
@RequestMapping("/usuarios") // Todas as requisições para /usuarios caem nesta classe
public class UsuarioController {
    // ... métodos GET, POST, etc. virão aqui ...
}
```

#### Verbos HTTP (CRUD)

- **`@GetMapping`**: Mapeia para requisições `GET` (Leitura). 
- **`@PostMapping`**: Mapeia para requisições `POST` (Criação).
- **`@PutMapping`**: Mapeia para requisições `PUT` (Atualização completa).
- **`@DeleteMapping`**: Mapeia para requisições `DELETE` (Remoção).

-------
### As 3 Formas de Receber Dados

Quando um cliente (ex: Postman ou um front-end) faz uma requisição, ele pode enviar dados de três formas. O Spring tem uma anotação para cada uma.

#### a) `@PathVariable` (Na URL)

Usado para dados que são parte da URL (identificadores). **URL:** `GET /usuarios/123`

```java
// O {id} na URL...
@GetMapping("/{id}") 
// ...é mapeado para a variável 'id' no método.
public Usuario buscarPorId(@PathVariable Long id) {
    // ...lógica para buscar o usuário com id 123...
    return usuarioService.buscar(id);
}
```

#### b) `@RequestParam` (Parâmetros de Query)

Usado para filtros, paginação ou dados opcionais. **URL:** `GET /usuarios?status=ativo&pagina=2`

```java
// Mapeia os parâmetros da URL para as variáveis do método
@GetMapping
public List<Usuario> buscarPorStatus(
    @RequestParam("status") String status,
    @RequestParam(value = "pagina", required = false, defaultValue = "0") int pagina
) {
    // ...lógica para buscar usuários com status "ativo" na página 0...
    return usuarioService.buscarPorStatus(status, pagina);
}
```

- `required = false`: Torna o parâmetro opcional.
- `defaultValue = "0"`: Define um valor padrão se não for fornecido.

#### c) `@RequestBody` (Corpo da Requisição)

Usado para dados complexos (JSON) enviados no corpo da requisição. Essencial para `POST` e `PUT`. **Requisição:** `POST /usuarios` **Body (JSON):**

```java
{
    "nome": "Kear",
    "email": "kear@email.com"
}
```

```java
@PostMapping
// O Spring automaticamente converte o JSON do corpo
// em um objeto 'Usuario' (DTO)
public Usuario criarUsuario(@RequestBody Usuario novoUsuario) {
    // ...lógica para salvar o novoUsuario...
    return usuarioService.salvar(novoUsuario);
}
```

------
### Exemplo Completo de um `UsuarioController`

Juntando todos os conceitos (incluindo Injeção de Dependência):

```java
package com.kear.meuprojeto.controller;

import org.springframework.web.bind.annotation.*;
import org.springframework.beans.factory.annotation.Autowired;
import java.util.List;

@RestController
@RequestMapping("/api/v1/usuarios") // Define o caminho base da API
public class UsuarioController {

    // 1. Injetando o Service (do arquivo 02)
    private final UsuarioService usuarioService;

    @Autowired
    public UsuarioController(UsuarioService usuarioService) {
        this.usuarioService = usuarioService;
    }

    // 2. CREATE (POST) com @RequestBody
    @PostMapping
    public Usuario criarUsuario(@RequestBody Usuario usuario) {
        return usuarioService.criar(usuario);
    }

    // 3. READ (GET) para todos, com @RequestParam
    @GetMapping
    public List<Usuario> listarUsuarios(
        @RequestParam(value = "status", required = false) String status
    ) {
        if (status != null) {
            return usuarioService.buscarPorStatus(status);
        }
        return usuarioService.listarTodos();
    }

    // 4. READ (GET) por ID, com @PathVariable
    @GetMapping("/{id}")
    public Usuario buscarUsuarioPorId(@PathVariable Long id) {
        return usuarioService.buscarPorId(id);
    }

    // 5. UPDATE (PUT) com @PathVariable e @RequestBody
    @PutMapping("/{id}")
    public Usuario atualizarUsuario(@PathVariable Long id, @RequestBody Usuario usuarioDetalhes) {
        return usuarioService.atualizar(id, usuarioDetalhes);
    }

    // 6. DELETE (DELETE) com @PathVariable
    @DeleteMapping("/{id}")
    public void deletarUsuario(@PathVariable Long id) {
        usuarioService.deletar(id);
    }
}
```

-----
