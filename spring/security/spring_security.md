# Spring Security

## O que é

Spring Security é o módulo do Spring responsável por **autenticação** (verificar quem é o usuário) e **autorização** (verificar o que esse usuário pode acessar). É o padrão utilizado para proteger APIs REST e aplicações web construídas com Spring, oferecendo suporte a login por formulário, autenticação via token (JWT), OAuth2, controle de permissões por rota e proteção contra ataques comuns (CSRF, sessão fixa etc.).

## Principais conceitos

- **Authentication**: representa a identidade do usuário autenticado (quem ele é).
- **Authorization**: define o que o usuário autenticado tem permissão de fazer (roles/permissões).
- **SecurityFilterChain**: cadeia de filtros que intercepta todas as requisições HTTP antes de chegarem ao controller.
- **UserDetailsService**: interface usada para carregar os dados do usuário (normalmente do banco de dados) durante a autenticação.
- **PasswordEncoder**: responsável por criptografar senhas (o padrão recomendado é `BCryptPasswordEncoder`).
- **JWT (JSON Web Token)**: abordagem comum em APIs REST para autenticação sem estado (*stateless*), em que o cliente envia um token a cada requisição.

## Exemplo básico de configuração

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable())
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll()
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            );

        return http.build();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

```java
@Service
public class UsuarioDetailsService implements UserDetailsService {

    @Autowired
    private UsuarioRepository usuarioRepository;

    @Override
    public UserDetails loadUserByUsername(String email) {
        Usuario usuario = usuarioRepository.findByEmail(email)
                .orElseThrow(() -> new UsernameNotFoundException("Usuário não encontrado"));

        return new org.springframework.security.core.userdetails.User(
                usuario.getEmail(),
                usuario.getSenha(),
                List.of(new SimpleGrantedAuthority("ROLE_" + usuario.getRole()))
        );
    }
}
```

## Fluxo comum com JWT

1. O usuário envia login e senha para `/api/auth/login`.
2. O servidor valida as credenciais e gera um token JWT.
3. O cliente armazena o token e o envia no cabeçalho `Authorization: Bearer <token>` em cada requisição.
4. Um filtro do Spring Security valida o token antes de liberar o acesso ao endpoint.

## Quando usar

Sempre que a API precisar restringir o acesso a determinados endpoints, diferenciar usuários comuns de administradores, ou implementar login e controle de sessão/token.

## Referências

- Documentação oficial: https://spring.io/projects/spring-security
- Guia de autenticação segura: https://spring.io/guides/topicals/spring-security-architecture
