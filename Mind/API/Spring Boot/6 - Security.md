--------

O Spring Security é um framework de autenticação e autorização. É o "segurança" da sua API.
### Os Dois Pilares da Segurança

* **Autenticação (Quem é você?):** É o processo de verificar a identidade. O usuário prova quem é, geralmente com um login (`username`) e senha.
* **Autorização (O que você pode fazer?):** É o processo de verificar permissões. Uma vez que sabemos que você é o "Kear" (autenticado), verificamos se o "Kear" tem permissão (`ROLE_ADMIN`) para acessar o endpoint `/admin/deletar-tudo`.

------
### A "Mágica" do Spring Security: A Cadeia de Filtros

Quando você adiciona o `spring-boot-starter-security`, o Spring Boot **auto-configura** uma "parede" na frente de toda a sua API.

Essa "parede" é chamada de **Cadeia de Filtros de Segurança (Security Filter Chain)**.

Toda e qualquer requisição HTTP que chega à sua API (seja para `/usuarios` ou `/produtos`) é *primeiro* interceptada por essa cadeia de filtros.

* Um filtro verifica se a requisição tem um token.
* Outro filtro tenta autenticar o usuário com esse token.
* Outro filtro verifica se o usuário autenticado pode acessar a URL solicitada.

**O Efeito Colateral Imediato:** Ao adicionar o starter, o Spring Security, por padrão:
1.  Bloqueia **TUDO**. Todos os endpoints da sua API ficam protegidos.
2.  Gera um formulário de login HTML básico.
3.  Gera uma senha aleatória no console.

Isso é o que chamamos de "Seguro por Padrão" (Secure by Default).

-------
### O Problema: "Stateful" vs. "Stateless"

O comportamento padrão do Spring Security (com formulário de login) é **Stateful (com estado)**. Ele usa **Sessões HTTP** e **Cookies** para lembrar que você está logado.

* **Isso é ÓTIMO para:** Aplicações web tradicionais (Monólitos com Thymeleaf/JSP).
* **Isso é PÉSSIMO para:** APIs REST.

APIs REST (que servem front-ends em React/Vue, aplicativos mobile, etc.) devem ser **Stateless (sem estado)**. O servidor **não deve** armazenar o estado de login (Sessão). Cada requisição deve conter em si mesma toda a informação necessária para ser autenticada.

**A Solução Stateless:** **JWT (JSON Web Token)**.

---------
### O Fluxo de Autenticação com JWT (Stateless)

Este é o padrão de ouro para APIs REST:

1.  **Requisição de Login:** O cliente (front-end) envia um `POST` para um endpoint público `/api/login` contendo o JSON `{"username": "kear", "password": "123"}`.
2.  **Validação:** O Spring Security (que *não* protege esse endpoint `/api/login`) recebe as credenciais.
3.  **Geração do Token:** O servidor valida o usuário e a senha no banco de dados. Se estiverem corretos, ele gera um **Token JWT**.
    * Um JWT é uma string longa e criptografada (ex: `eyJhbGciOi...`) que contém os dados do usuário (ex: `{"sub": "kear", "role": "ADMIN"}`) e uma data de expiração.
4.  **Resposta:** O servidor retorna esse Token JWT para o cliente.
5.  **Requisições Futuras:** O cliente **armazena** esse token (ex: no Local Storage). Para *todas* as requisições futuras (ex: `GET /api/usuarios`), o cliente deve adicionar o token no cabeçalho (Header):
    `Authorization: Bearer eyJhbGciOi...`
6.  **Validação do Token:** A Cadeia de Filtros do Spring Security intercepta essa requisição. Um filtro customizado (que vamos criar) vê o token `Bearer`, o decodifica, valida sua assinatura e expiração, e diz ao Spring: "Ok, este é o 'Kear' e ele é 'ADMIN'. Pode deixar a requisição passar."

------
### Componentes Principais (Configuração)

Para fazer isso funcionar, precisamos configurar os "tijolos" do Spring Security.

#### a) `PasswordEncoder` (Obrigatório)

O Spring Security se recusa a trabalhar com senhas em texto plano. Você **DEVE** armazenar as senhas no banco de dados usando um *hash* (um algoritmo de mão única). O padrão é o **BCrypt**.

Precisamos expor um Bean para que o Spring saiba como "hashear" e comparar as senhas.

```java
@Configuration
public class SecurityConfig {
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        // Usa o BCrypt, o algoritmo de hash mais recomendado
        return new BCryptPasswordEncoder();
    }
}
```

#### b) `UserDetailsService` (Como carregar o usuário)

O Spring precisa saber como "buscar um usuário pelo nome de usuário" no seu banco. Você implementa essa interface.

```java
@Service
public class MeuUserDetailsService implements UserDetailsService {

    @Autowired
    private UsuarioRepository usuarioRepository; // Nosso repositório JPA

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        Usuario usuario = usuarioRepository.findByEmail(username) // Supondo que o email é o login
            .orElseThrow(() -> new UsernameNotFoundException("Usuário não encontrado!"));
        
        // Converte nosso 'Usuario' (JPA) para o 'User' (Spring Security)
        // O Spring usará isso para comparar a senha
        return new org.springframework.security.core.userdetails.User(
            usuario.getEmail(),
            usuario.getSenha(),
            // Aqui você passaria as permissões/roles (ex: "ROLE_ADMIN")
            new ArrayList<>() 
        );
    }
}
```

#### c) `SecurityFilterChain` (A "Parede" de Filtros)

Este é o Bean mais importante. É aqui que você desliga o "stateful" (sessões) e define as regras de "autorização".

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    // (O PasswordEncoder Bean está aqui...)

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            // 1. Desliga o CSRF (vulnerabilidade de sites stateful, não se aplica a API JWT)
            .csrf(csrf -> csrf.disable())
            
            // 2. Define a política de Sessão como STATELESS (NÃO criar sessões)
            .sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
            
            // 3. Define as regras de AUTORIZAÇÃO
            .authorizeHttpRequests(authz -> authz
                // Permite acesso público ao endpoint de login
                .requestMatchers("/api/login").permitAll() 
                // Permite acesso público à documentação Swagger/OpenAPI
                .requestMatchers("/v3/api-docs/**", "/swagger-ui/**").permitAll()
                
                // Exige ROLE_ADMIN para deletar usuários
                .requestMatchers(HttpMethod.DELETE, "/api/usuarios/**").hasRole("ADMIN")
                
                // Exige autenticação (qualquer token válido) para todo o resto
                .anyRequest().authenticated()
            );
            
        // 4. (Aqui você adicionaria seus filtros customizados, como o filtro JWT)
        // http.addFilterBefore(meuFiltroJwt, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }
}
```

------
