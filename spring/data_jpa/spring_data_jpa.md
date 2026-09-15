# Spring Data JPA

## O que é

Spring Data JPA é o módulo do Spring que simplifica o acesso a bancos de dados relacionais, usando a especificação **JPA (Jakarta Persistence API)** com implementação padrão do **Hibernate**. Ele elimina a necessidade de escrever SQL manual e código repetitivo de DAO, gerando automaticamente as operações de CRUD a partir de interfaces Java.

## Principais componentes

- **@Entity**: marca uma classe Java como uma tabela do banco de dados.
- **@Id / @GeneratedValue**: define a chave primária e sua estratégia de geração.
- **JpaRepository**: interface que já fornece métodos prontos (`save`, `findById`, `findAll`, `deleteById` etc.), sem necessidade de implementação.
- **Query Methods**: métodos cujo nome é interpretado automaticamente e convertido em consulta SQL (ex.: `findByNome`, `findByPrecoGreaterThan`).
- **@Query**: permite escrever consultas JPQL ou SQL nativo quando o nome do método não é suficiente.
- **Relacionamentos**: `@OneToMany`, `@ManyToOne`, `@ManyToMany`, `@OneToOne` para mapear relações entre entidades.

## Exemplo básico

```java
@Entity
public class Produto {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nome;
    private BigDecimal preco;

    // getters e setters
}
```

```java
public interface ProdutoRepository extends JpaRepository<Produto, Long> {

    List<Produto> findByNomeContaining(String nome);

    List<Produto> findByPrecoGreaterThan(BigDecimal preco);
}
```

```java
@Service
public class ProdutoService {

    @Autowired
    private ProdutoRepository produtoRepository;

    public List<Produto> listarTodos() {
        return produtoRepository.findAll();
    }

    public Produto salvar(Produto produto) {
        return produtoRepository.save(produto);
    }
}
```

## Configuração (application.properties)

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/meubanco
spring.datasource.username=root
spring.datasource.password=senha
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

## Quando usar

Sempre que o backend precisar persistir dados em um banco relacional (MySQL, PostgreSQL, H2 etc.), evitando a escrita manual de SQL e reduzindo código repetitivo de acesso a dados.

## Referências

- Documentação oficial: https://spring.io/projects/spring-data-jpa
- Guia de acesso a dados: https://spring.io/guides/gs/accessing-data-jpa/
