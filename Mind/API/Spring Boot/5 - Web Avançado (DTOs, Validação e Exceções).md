-----------
### O Problema: Por que NÃO usar `@Entity` na API?

Nos arquivos anteriores, por simplicidade, usamos nossa classe `@Entity` (ex: `Usuario`) diretamente no `@RestController`. Isso é uma **péssima prática** em produção por três motivos:

1.  **Risco de Segurança (Over-posting):** Se o seu JSON no `POST` tiver um campo `admin: true` e sua entidade `Usuario` tiver esse campo, o Jackson (conversor JSON) pode tentar "mapear" isso, causando uma falha de segurança.
2.  **Exposição de Dados Sensíveis (Under-posting):** Ao fazer um `GET`, você retorna a entidade inteira, o que pode incluir campos sensíveis que o front-end não precisa (ex: `senha`, `dataDeCriacao`, `salario`).
3.  **Acoplamento:** Seu front-end fica *acoplado* à sua estrutura de banco de dados. Se você renomear uma coluna no banco (ex: `email` para `email_pessoal`), a API quebra e o front-end quebra junto.

------
### A Solução: Padrão DTO (Data Transfer Object)

**DTO** é um objeto simples (um "POJO") criado *exclusivamente* para **transferir dados** entre camadas (ex: entre o Controller e o Cliente).

* Você cria DTOs separados para **Entrada** (Request) e **Saída** (Response).
* A camada de **Service** se torna a "tradutora" oficial: ela recebe um DTO, converte-o para Entidade (para salvar no banco) ou busca uma Entidade e a converte para um DTO (para enviar como resposta).

#### Exemplo de DTOs

```java
// DTO de ENTRADA (só o que o cliente pode enviar)
// (Não tem ID, não tem senha)
public class UsuarioRequestDTO {
    private String nome;
    private String email;
    private String senhaPlana; // Recebemos a senha, mas não é a 'senha' do banco
    // Getters e Setters
}

// DTO de SAÍDA (só o que o cliente pode ver)
// (Não tem senha!)
public class UsuarioResponseDTO {
    private Long id;
    private String nome;
    private String email;
    // Getters e Setters
    
    // Construtor que "traduz" a Entidade (MUITO ÚTIL)
    public UsuarioResponseDTO(Usuario entidade) {
        this.id = entidade.getId();
        this.nome = entidade.getNome();
        this.email = entidade.getEmail();
    }
}
```

#### Usando DTOs no Controller e Service

```java
// Controller (AGORA SÓ CONHECE DTOs)
@RestController
@RequestMapping("/usuarios")
public class UsuarioController {
    
    @Autowired
    private UsuarioService usuarioService;

    // RECEBE um RequestDTO
    @PostMapping
    public UsuarioResponseDTO criarUsuario(@RequestBody UsuarioRequestDTO dto) {
        return usuarioService.criar(dto);
    }
    
    // RETORNA um ResponseDTO
    @GetMapping("/{id}")
    public UsuarioResponseDTO buscarPorId(@PathVariable Long id) {
        return usuarioService.buscar(id);
    }
}

// Service (FAZ A TRADUÇÃO)
@Service
public class UsuarioService {
    
    @Autowired
    private UsuarioRepository usuarioRepository;

    public UsuarioResponseDTO criar(UsuarioRequestDTO dto) {
        // 1. Traduz DTO -> Entidade
        Usuario entidade = new Usuario();
        entidade.setNome(dto.getNome());
        entidade.setEmail(dto.getEmail());
        entidade.setSenha(dto.getSenhaPlana()); // (Aqui você faria o HASH da senha)
        
        // 2. Salva a Entidade
        Usuario entidadeSalva = usuarioRepository.save(entidade);
        
        // 3. Traduz Entidade -> DTO e retorna
        return new UsuarioResponseDTO(entidadeSalva);
    }
    
    public UsuarioResponseDTO buscar(Long id) {
        Usuario entidade = usuarioRepository.findById(id).orElseThrow();
        return new UsuarioResponseDTO(entidade);
    }
}
```

-----
### Validações (`spring-boot-starter-validation`)

Agora que temos DTOs de entrada, podemos validar os dados _antes_ que eles cheguem no Service.

**1. Adicione a dependência:** `spring-boot-starter-validation`
**2. Anote o DTO de Entrada:**

```java
// Importe de 'jakarta.validation.constraints.*'
import jakarta.validation.constraints.*;

public class UsuarioRequestDTO {
    
    @NotBlank(message = "Nome não pode estar em branco")
    @Size(min = 3, message = "Nome deve ter no mínimo 3 caracteres")
    private String nome;
    
    @NotBlank(message = "Email não pode estar em branco")
    @Email(message = "Email inválido")
    private String email;
    
    @NotBlank
    @Size(min = 6, max = 20)
    private String senhaPlana;
    
    // Getters e Setters
}
```

- `@NotBlank`: Não pode ser nulo E não pode ser `""`.
- `@NotEmpty`: Não pode ser nulo E não pode ser `[]` (para listas).
- `@Email`: Valida o formato de email.
- `@Size(min, max)`: Valida o tamanho.
- `@Min`, `@Max`: Para números.

**3. Ative a Validação no Controller
Use a anotação `@Valid` no `@RequestBody`.

```java
@PostMapping
// 1. O Spring agora valida o DTO antes de chamar o método.
// 2. Se falhar, ele lança uma 'MethodArgumentNotValidException'
public UsuarioResponseDTO criarUsuario(@Valid @RequestBody UsuarioRequestDTO dto) {
    return usuarioService.criar(dto);
}
```

--------
### Tratamento Global de Exceções

O que acontece se o `@Valid` falhar? Ou se o `usuarioRepository.findById().orElseThrow()` for executado? Por padrão, o Spring retorna um JSON feio e um erro 500 (Internal Server Error) ou 400 (Bad Request).

Vamos capturar _todas_ as exceções da nossa API em um só lugar para retornar um JSON de erro limpo.

**1. Crie uma classe de Erro Padrão:**

```java
// DTO para nossos erros
public class ErroResponseDTO {
    private int status;
    private String mensagem;
    private long timestamp;
    // Construtor, Getters, Setters...
}
```

**2. Crie o Handler Global (`@ControllerAdvice`)2. Crie o Handler Global (`@ControllerAdvice`)

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import java.util.NoSuchElementException; // Exceção comum do orElseThrow()

// 1. @ControllerAdvice: Diz ao Spring "Esta classe vai monitorar
//    TODOS os Controllers em busca de exceções".
@ControllerAdvice
public class GlobalExceptionHandler {

    // 2. @ExceptionHandler: Mapeia uma exceção específica para este método.
    //    Vamos capturar o erro de "não encontrado".
    @ExceptionHandler(NoSuchElementException.class)
    public ResponseEntity<ErroResponseDTO> handleNaoEncontrado(NoSuchElementException ex) {
        
        ErroResponseDTO erro = new ErroResponseDTO(
            HttpStatus.NOT_FOUND.value(), // 404
            "Recurso não encontrado.",
            System.currentTimeMillis()
        );
        
        // 3. ResponseEntity: Objeto do Spring que permite controlar
        //    o status HTTP (404) e o corpo (nosso DTO de erro)
        return new ResponseEntity<>(erro, HttpStatus.NOT_FOUND);
    }
    
    // (Você pode adicionar outros handlers aqui, como para a
    // 'MethodArgumentNotValidException' da validação, retornando um 400)
}
```

Agora, toda vez que um `orElseThrow()` falhar em _qualquer service_, o cliente receberá um JSON limpo e um status `404 Not Found`.

----------
