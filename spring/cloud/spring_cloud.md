# Spring Cloud

## O que é

Spring Cloud é um conjunto de ferramentas do ecossistema Spring voltado para a construção de **arquiteturas de microsserviços**. Ele resolve problemas comuns quando um sistema é dividido em vários serviços independentes, como descoberta de serviços, roteamento, configuração centralizada, tolerância a falhas e comunicação entre serviços.

Diferente do Spring Boot (que cria uma aplicação) e do Spring MVC (que expõe endpoints dentro dela), o Spring Cloud atua na **coordenação entre múltiplas aplicações Spring Boot** que juntas formam o sistema.

## Principais componentes

- **Spring Cloud Config**: centraliza as configurações de todos os microsserviços em um único repositório (ex.: Git), evitando duplicação de `application.properties`.
- **Eureka (Service Discovery)**: permite que os microsserviços se registrem e se localizem automaticamente, sem precisar de endereços fixos.
- **Spring Cloud Gateway**: atua como *API Gateway*, roteando as requisições externas para o microsserviço correto e centralizando preocupações como autenticação e limitação de taxa.
- **OpenFeign**: cliente HTTP declarativo que simplifica a comunicação entre microsserviços.
- **Resilience4j / Circuit Breaker**: evita que a falha de um microsserviço derrube o sistema inteiro, aplicando padrões como *circuit breaker* e *retry*.

## Exemplo básico de comunicação entre serviços (OpenFeign)

```java
@FeignClient(name = "servico-estoque", url = "${servico.estoque.url}")
public interface EstoqueClient {

    @GetMapping("/api/estoque/{produtoId}")
    EstoqueDTO consultarEstoque(@PathVariable Long produtoId);
}
```

```java
@Service
public class PedidoService {

    @Autowired
    private EstoqueClient estoqueClient;

    public boolean produtoDisponivel(Long produtoId) {
        EstoqueDTO estoque = estoqueClient.consultarEstoque(produtoId);
        return estoque.getQuantidade() > 0;
    }
}
```

## Exemplo de uso do Eureka

```java
@SpringBootApplication
@EnableEurekaClient
public class PedidoServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(PedidoServiceApplication.class, args);
    }
}
```

## Quando usar

Apenas quando o sistema realmente é dividido em múltiplos microsserviços independentes. Para uma aplicação monolítica simples (um único backend), Spring Boot + Spring MVC + Spring Data JPA já são suficientes, e o Spring Cloud tende a adicionar complexidade desnecessária.

## Referências

- Documentação oficial: https://spring.io/projects/spring-cloud
- Guia de microsserviços com Spring: https://spring.io/microservices
