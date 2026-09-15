# Spring MVC

## O que é

Spring MVC é o módulo do Spring responsável por construir aplicações web e APIs REST, seguindo o padrão arquitetural **MVC (Model-View-Controller)**. Ele gerencia o ciclo de requisição/resposta HTTP, o roteamento de URLs para métodos Java (*controllers*) e a conversão de dados (JSON, XML, formulários).

Em projetos modernos, o Spring MVC costuma ser usado para expor **APIs REST** consumidas por um frontend separado (React, Angular, Vue etc.), em vez de gerar páginas HTML no servidor.

## Principais componentes

- **DispatcherServlet**: ponto central que recebe todas as requisições HTTP e as encaminha para o controller correto.
- **@Controller / @RestController**: classes que tratam as requisições. `@RestController` já retorna os dados diretamente como JSON, sem precisar de `@ResponseBody` em cada método.
- **@RequestMapping / @GetMapping / @PostMapping / @PutMapping / @DeleteMapping**: mapeiam rotas HTTP para métodos Java.
- **@PathVariable / @RequestParam / @RequestBody**: capturam dados da URL, da query string ou do corpo da requisição.
- **ResponseEntity**: permite controlar o status HTTP e o corpo da resposta com mais precisão.

## Exemplo básico de API REST

```java
@RestController
@RequestMapping("/api/produtos")
public class ProdutoController {

    @Autowired
    private ProdutoService produtoService;

    @GetMapping
    public List<Produto> listar() {
        return produtoService.listarTodos();
    }

    @GetMapping("/{id}")
    public ResponseEntity<Produto> buscarPorId(@PathVariable Long id) {
        return produtoService.buscarPorId(id)
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }

    @PostMapping
    public ResponseEntity<Produto> criar(@RequestBody Produto produto) {
        Produto salvo = produtoService.salvar(produto);
        return ResponseEntity.status(HttpStatus.CREATED).body(salvo);
    }
}
```

## Fluxo de uma requisição

1. O cliente (frontend, Postman etc.) envia uma requisição HTTP.
2. O `DispatcherServlet` recebe a requisição e identifica o controller responsável.
3. O controller processa a lógica (normalmente delegando para uma camada de *service*).
4. A resposta é convertida para JSON (ou outro formato) e devolvida ao cliente.

## Quando usar

Sempre que o backend precisar expor endpoints HTTP — seja para uma API REST consumida por um frontend, seja para uma aplicação web tradicional renderizada no servidor (com Thymeleaf, por exemplo).

## Referências

- Documentação oficial: https://docs.spring.io/spring-framework/reference/web/webmvc.html
- Guia de API REST: https://spring.io/guides/gs/rest-service/
