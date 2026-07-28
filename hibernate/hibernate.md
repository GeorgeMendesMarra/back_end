# Tutorial Técnico de Hibernate para Iniciantes

Hibernate é uma das soluções mais populares para **Mapeamento Objeto-Relacional (ORM)** no ecossistema Java, automatizando a tarefa de persistir objetos Java em bancos de dados relacionais . Este tutorial apresenta os conceitos fundamentais para quem nunca teve contato com a tecnologia.

---

## 1. O que é Hibernate?

Hibernate é uma biblioteca para mapeamento objeto-relacional que reduz significativamente o tempo de desenvolvimento, eliminando a necessidade de manipulação manual de dados com SQL e JDBC . Seu objetivo é aliviar o desenvolvedor de aproximadamente 95% das tarefas comuns de programação relacionadas à persistência de dados .

### Hibernate vs JPA

É importante entender a relação entre Hibernate e JPA:

| Aspecto | JPA (Jakarta Persistence API) | Hibernate |
|---------|-------------------------------|-----------|
| **Natureza** | Especificação (conjunto de regras) | Implementação concreta da especificação |
| **Funcionalidade** | Define interfaces e anotações padrão | Implementa JPA e oferece funcionalidades extras |
| **Exemplo** | `@Entity`, `EntityManager` | Caching, HQL, mapeamentos avançados |

Pense na JPA como as "regras" do jogo, enquanto Hibernate é o jogador que as executa, muitas vezes indo além das regras .

### Analogia para Entender

Hibernate atua como uma **ponte** entre o mundo orientado a objetos do Java e o mundo relacional do banco de dados:

- **Objetos Java:** Como arquivos organizados em pastas com estrutura hierárquica
- **Tabelas SQL:** Como planilhas com linhas e colunas
- **Hibernate:** O tradutor que converte entre esses dois mundos

---

## 2. A Arquitetura do Hibernate

Hibernate é flexível e suporta várias abordagens de arquitetura. As duas extremidades são a arquitetura "mínima" e a "completa" .

### Componentes Principais

| Componente | Descrição | Características Técnicas |
|------------|-----------|--------------------------|
| **SessionFactory** | Fábrica de Sessions e cliente de ConnectionProvider | Thread-safe, cache imutável de metadados compilados, contém cache opcional de segundo nível  |
| **Session** | Representa uma conversa entre aplicação e banco de dados | Single-threaded, curta duração, envolve uma conexão JDBC, possui cache obrigatório de primeiro nível  |
| **Transaction** | Define unidades atômicas de trabalho | Single-threaded, abstrai JDBC/JTA/CORBA, uma Session pode ter várias transações  |
| **ConnectionProvider** | Fábrica e pool de conexões JDBC | Opcional, abstrai DataSource ou DriverManager  |

### Estados das Instâncias

Uma instância de uma classe persistente pode estar em três estados diferentes, definidos em relação a um contexto de persistência (a `Session`) :

1. **Transiente (Transient):** A instância não está associada a nenhum contexto de persistência. Não possui identidade persistente ou chave primária .

2. **Persistente (Persistent):** A instância está associada a um contexto de persistência. Possui identidade persistente e pode ter uma linha correspondente no banco de dados .

3. **Detached (Desanexado):** A instância esteve associada a um contexto de persistência, mas este foi fechado. Possui identidade persistente e pode ter uma linha correspondente no banco de dados .

---

## 3. Operações Fundamentais com Session

Uma `Session` (ou `EntityManager` da JPA) representa um **contexto de persistência com estado**. Ela mantém um conjunto de entidades associadas, detecta automaticamente modificações e as propaga para o banco de dados .

### Operações Básicas

#### #1 `find()` - Buscar Entidade
O método `find()` busca a entidade com a chave primária fornecida no banco de dados:
```java
var book = session.find(Book.class, isbn);
```
A entidade retornada é associada ao contexto de persistência, portanto qualquer modificação em seu estado será automaticamente propagada ao banco de dados quando a sessão fizer o `flush` .

`find()` também aceita opções como `LockMode` e `EntityGraph` para personalizar o comportamento .

#### #2 `persist()` - Inserir Entidade
O método `persist()` adiciona uma entidade ao contexto de persistência e a agenda para inserção no banco de dados:
```java
session.persist(book);
```
A inserção real não ocorre até que a sessão execute o `flush` .

#### #3 `refresh()` - Atualizar Estado
O método `refresh()` sobrescreve o estado da entidade com o estado persistente atual do banco de dados :
```java
session.refresh(book);
```

#### #4 `merge()` - Mesclar Entidade
O método `merge()` mescla o estado de uma entidade que não está associada ao contexto de persistência atual :
```java
var managedBook = session.merge(detachedBook);
```
Retorna uma entidade associada ao contexto de persistência, com o mesmo estado da entidade passada como argumento .

#### #5 `remove()` - Remover Entidade
O método `remove()` agenda a exclusão de uma entidade do banco de dados :
```java
session.remove(book);
```
A exclusão real ocorre durante o `flush` da sessão .

### Queries

- **Selection Queries:** Recuperam dados do banco de dados. As entidades retornadas são associadas ao contexto de persistência .
- **Mutation Queries:** Atualizam ou deletam dados .
- **Criteria Queries:** Construídas programaticamente .
- **Native Queries:** SQL nativo do banco. Desde a versão 6 do Hibernate, são raramente necessárias devido ao poder do HQL .

---

## 4. Mapeamentos Comuns

### #1 Classes de Entidade

Uma classe de entidade é uma classe Java que mapeia uma tabela do banco de dados, anotada com `@Entity` :
```java
@Entity
class Book { ... }
```

A tabela mapeada pode ser especificada com `@Table`:
```java
@Entity
@Table(name = "books")
class Book { ... }
```

### #2 Herança de Entidades

Uma entidade pode herdar de outra . Por padrão, as duas entidades mapeiam a mesma tabela. Para um esquema normalizado, usa-se `@Inheritance(strategy = JOINED)` .

### #3 Atributos Básicos

Um atributo básico mapeia uma coluna da tabela. Geralmente é um tipo primitivo ou tipo integrado do Java como `String` ou `LocalDateTime` :
```java
String name;
```

A coluna pode ser especificada com `@Column`:
```java
@Column(name = "author_name")
String name;
```

### #4 Identificadores

Toda entidade deve ter um identificador mapeando a chave primária da tabela :
```java
@Id
String isbn;
```

Para chaves geradas automaticamente:
```java
@Id
@GeneratedValue
UUID id;
```

### #5 Versões e Timestamps

A anotação `@Version` declara um campo usado para bloqueio otimista automático :
```java
@Version
LocalDateTime lastUpdated;
```

### #6 Associações

A associação mais comum é o relacionamento bidirecional one-to-many :
```java
@Entity
class Book {
    @Id
    String isbn;

    @ManyToOne(fetch = LAZY)
    Publisher publisher;
}

@Entity
class Publisher {
    @Id
    @GeneratedValue
    Long id;

    @OneToMany(mappedBy = "publisher")
    List<Book> books;
}
```

### #7 Classes Embeddable

Classes Java que não mapeiam suas próprias tabelas, mas cujos campos mapeiam colunas da tabela da entidade proprietária :
```java
@Embeddable
class Name {
    String first;
    String last;
    Character initial;
}

@Entity
class Author {
    @Id
    String ssn;

    Name name;
}
```

### #8 Tipos Enumerados

Um `enum` Java pode mapear uma coluna `tinyint` com seu valor ordinal ou `varchar` com seu nome :
```java
@Enumerated(STRING)
DayOfWeek dayOfWeek;
```

---

## 5. Evolução e Versões do Hibernate

Hibernate possui um longo histórico de versões. A tabela abaixo resume as principais séries, suas características e compatibilidade :

### Séries Atuais

| Série | Status | Java | Jakarta Persistence | Destaques |
|-------|--------|------|---------------------|-----------|
| **8.0** | Em desenvolvimento | 17, 21, 25 | 4.0 | Jakarta EE 12, Jakarta Data 1.1  |
| **7.4** | Última estável | 17, 21, 25, 26 | 3.2 | `@Temporal`, `@Audited`, CacheMode.REFRESH_SESSION  |
| **7.3** | Suporte limitado | 17, 21, 25, 26 | 3.2 | `@NaturalIdClass`, entidades sem construtor padrão  |
| **6.6** | Suporte limitado | 11, 17, 21, 25, 26 | 3.1 | Jakarta Data 1.0, `@ConcreteProxy`  |

### Séries Antigas

| Série | Status | Java | Jakarta Persistence | Destaques |
|-------|--------|------|---------------------|-----------|
| **7.2** | Suporte limitado | - | - | Bloqueio pessimista, `@EmbeddedTable`, suporte a Vector  |
| **7.1** | Suporte limitado | - | - | Resource Scanning, melhorias em Locking  |
| **7.0** | Fim de vida | 17 | 3.2 | Apache License, Java 17, QuerySpecification  |
| **6.5** | Fim de vida | 11, 17, 21 | 3.1 | Java Time Handling, records como @IdClass, Jakarta Data (tech preview)  |
| **6.4** | Fim de vida | - | - | soft-delete, funções de array  |
| **6.3** | Fim de vida | - | 3.1 | Jakarta Persistence 3.1, query methods, finder methods  |
| **6.2** | Suporte limitado | 11, 17, 20, 21 | 3.1 | records, structs, partitioning, SQL MERGE  |
| **6.1** | Fim de vida | - | - | Subquery in FROM, JDBC Array support  |
| **6.0** | Fim de vida | - | - | Performance, HQL, Criteria  |
| **5.6** | Fim de vida | 8, 11, 17, 18 | 2.2 (JPA) | Fim do suporte a Javassist  |
| **5.5** | Fim de vida | - | 3.0 (Jakarta) | Introdução ao Jakarta JPA  |
| **5.4** | Fim de vida | 11 | - | EntityGraph improvements, JDK 11 support  |
| **5.3** | Fim de vida | - | 2.2 (JPA) | JPA 2.2, inheritance caching  |

### Compatibilidade entre Versões

As versões mais recentes são **retrocompatíveis** com versões anteriores da JPA — por exemplo, Hibernate ORM 4.3 funciona com JPA 1.0 . No entanto, versões mais novas do ORM podem ser **incompatíveis** com containers JPA mais antigos .

---

## 6. Sessões Stateless

Para cenários que exigem maior controle ou onde o contexto de persistência não é necessário, Hibernate oferece a `StatelessSession` .

### Características da StatelessSession

- **Sem contexto de persistência:** Mudanças só afetam o banco quando explicitamente solicitadas 
- **Operações diretas:** `insert()`, `update()`, `delete()`, `upsert()` executam imediatamente 
- **Sem lazy loading:** `fetch()` deve ser chamado explicitamente para inicializar associações 
- **Útil quando:** Você precisa gerenciar manualmente `flush()`, `detach()` e `clear()` de uma session com estado 

---

## 7. Considerações Finais

### Quando usar Hibernate?

**Use Hibernate quando:**
- Deseja mapear objetos Java para tabelas relacionais de forma produtiva
- Precisa de cache de primeiro e segundo nível
- Requer linguagem de consulta poderosa (HQL/JPA)
- Trabalha com modelos de domínio orientados a objetos no middle-tier Java 

**Evite Hibernate quando:**
- Aplicação é estritamente centrada em dados com stored procedures
- Não há benefício em usar ORM (ex: operações simples em lote)
- O time não tem familiaridade com conceitos de ORM

### Hibernate no Ecossistema Moderno

- **Spring Boot + Spring Data JPA:** A forma mais comum de usar Hibernate hoje, abstraindo grande parte da configuração 
- **Quarkus:** Hibernate ORM com suporte nativo e reativo
- **WildFly/Red Hat EAP:** Containers Jakarta EE com Hibernate integrado 

---

## Referências

- Hibernate ORM Documentation 
- Hibernate ORM Releases 
- Hibernate ORM Architecture Documentation 
- Hibernate Getting Started Guide 
- Hibernate ORM Quick Guide 
