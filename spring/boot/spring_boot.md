# Spring Boot

## O que é

Spring Boot é um módulo do ecossistema Spring que simplifica a criação de aplicações Java, eliminando grande parte da configuração manual exigida pelo Spring tradicional. Ele oferece configuração automática (*auto-configuration*), um servidor embutido (Tomcat, Jetty ou Undertow) e um sistema de dependências pré-definidas (*starters*), permitindo criar uma aplicação backend funcional em minutos.

Hoje é praticamente o padrão de mercado para iniciar qualquer projeto Spring.

## Principais características

- **Auto-configuração**: o Spring Boot detecta as dependências do projeto e configura automaticamente os componentes necessários.
- **Servidor embutido**: não é preciso instalar um servidor de aplicação separado; a aplicação já roda como um `.jar` executável.
- **Starters**: dependências agrupadas por finalidade (ex.: `spring-boot-starter-web`, `spring-boot-starter-data-jpa`).
- **Spring Boot Actuator**: expõe endpoints de monitoramento (saúde, métricas, informações da aplicação).
- **Application Properties/YAML**: configuração centralizada em `application.properties` ou `application.yml`.

## Exemplo básico

```java
@SpringBootApplication
public class BackendApplication {
    public static void main(String[] args) {
        SpringApplication.run(BackendApplication.class, args);
    }
}
```

```java
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Olá, mundo!";
    }
}
```

## Estrutura de projeto comum

```
src/main/java/com/exemplo/backend
├── controller
├── service
├── repository
├── model
└── BackendApplication.java
src/main/resources
└── application.properties
```

## Quando usar

Sempre que for iniciar um novo projeto backend em Java, já que o Spring Boot reduz drasticamente o tempo de configuração inicial e integra facilmente com os demais módulos do Spring (MVC, Data JPA, Security, Cloud).

## Referências

- Documentação oficial: https://spring.io/projects/spring-boot
- Guia de início rápido: https://spring.io/guides/gs/spring-boot/
