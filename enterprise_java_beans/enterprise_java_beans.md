# Tutorial Técnico de Enterprise JavaBeans (EJB) para Iniciantes

Enterprise JavaBeans (EJB) é uma especificação do lado do servidor para desenvolvimento de componentes de negócio transacionais, seguros e distribuídos em Java. Este tutorial apresenta os conceitos fundamentais para quem nunca teve contato com a tecnologia.

---

## 1. O que é Enterprise JavaBeans?

Enterprise JavaBeans (EJB) define um modelo para construção de aplicações baseadas em componentes em Java. EJBs são componentes de software reutilizáveis que executam dentro de um **EJB Container** (como GlassFish, WebLogic ou WildFly) e encapsulam a lógica de negócio da aplicação.

### Características Fundamentais

- **Execução no Servidor:** EJBs sempre executam no lado servidor, dentro de um container que fornece serviços de infraestrutura como transações, persistência e pooling.
- **Encapsulamento da Lógica de Negócio:** O EJB contém o código que cumpre o propósito da aplicação — métodos como `checkInventoryLevel` e `orderProduct` em um sistema de controle de estoque.
- **Serviços Automáticos:** O container gerencia automaticamente transações, segurança, concorrência e pooling de recursos, liberando o desenvolvedor para focar na lógica de negócio.

### Analogia para Entender

Pense no EJB como a **cozinha de um restaurante**:
- **Front-end (JSF/Servlets):** O garçom (interage com o cliente)
- **EJB:** A cozinha (prepara a comida, segue receitas, gerencia estoque)
- **Container EJB:** A estrutura da cozinha (fogão, geladeira, sistema de exaustão)

O garçom nunca cozinha, e a cozinha nunca atende clientes diretamente.

---

## 2. A Arquitetura do EJB

O EJB define papéis e responsabilidades separados:

| Papel | Responsabilidade |
|-------|------------------|
| **EJB Developer** | Projeta e cria os EJBs (escreve as classes e interfaces) |
| **Application Developer** | Monta aplicações usando EJBs |
| **EJB Container** | Hospeda os EJBs e fornece serviços de runtime (WebLogic, GlassFish) |
| **System Administrator** | Implanta (deploy) os EJBs no servidor |

### Os Contratos do EJB

A especificação EJB estabelece "contratos" que definem os serviços fornecidos por cada papel. Esses contratos garantem que um EJB desenvolvido para um container funcione em qualquer outro container compatível.

### Tecnologias Subjacentes

EJBs dependem de outras tecnologias Java EE:
- **JNDI (Java Naming and Directory Interface):** Para localizar instâncias de EJBs
- **JDBC:** Para persistência em bancos de dados relacionais
- **JTA (Java Transaction API):** Para participar de transações
- **JMS (Java Message Service):** Para mensageria assíncrona
- **RMI/IIOP:** Para operar em ambientes distribuídos

---

## 3. Tipos de EJBs

### 3.1 Session Beans (Beans de Sessão)

Session beans são EJBs **transientes** que servem um único cliente. Não são persistentes — quando o cliente se desconecta, o session bean é removido. Session beans tendem a implementar lógica procedural, incorporando ações em vez de dados.

#### Stateless Session Bean (Bean de Sessão Sem Estado)

**Características:**
- Não mantém estado específico do cliente entre chamadas
- Pode ser usado por qualquer cliente
- Ideal para serviços que não dependem do contexto da sessão (ex: conversão de moedas, envio de e-mails)

**Exemplo Prático (ConverterBean):**

```java
@Stateless
public class ConverterBean {
    private BigDecimal yenRate = new BigDecimal("83.0602");
    private BigDecimal euroRate = new BigDecimal("0.0093016");

    public BigDecimal dollarToYen(BigDecimal dollars) {
        BigDecimal result = dollars.multiply(yenRate);
        return result.setScale(2, BigDecimal.ROUND_UP);
    }

    public BigDecimal yenToEuro(BigDecimal yen) {
        BigDecimal result = yen.multiply(euroRate);
        return result.setScale(2, BigDecimal.ROUND_UP);
    }
}
```

**Observação:** A anotação `@Stateless` informa ao container que este é um bean de sessão sem estado.

**Uso no Cliente:**

```java
@WebServlet(urlPatterns="/")
public class ConverterServlet extends HttpServlet {
    @EJB
    ConverterBean converter;  // Injeção de dependência
    
    protected void doGet(HttpServletRequest request, 
                         HttpServletResponse response) {
        BigDecimal dollars = new BigDecimal(request.getParameter("amount"));
        BigDecimal yenAmount = converter.dollarToYen(dollars);
        // Exibe resultado...
    }
}
```

A anotação `@EJB` injeta automaticamente uma referência ao EJB no servlet.

#### Stateful Session Bean (Bean de Sessão com Estado)

**Características:**
- Mantém estado específico do cliente entre múltiplas interações
- Útil para gerenciar processos (ex: carrinho de compras, workflow)
- O container pode passivar (salvar em armazenamento secundário) e ativar o bean conforme necessário

**Exemplo (Carrinho de Compras):**

```java
@Stateful
public class CartBean implements Cart {
    private String customerId;
    private String customerName;
    private List<String> contents;
    
    public void initialize(String person, String id) {
        this.customerName = person;
        this.customerId = id;
        this.contents = new ArrayList<>();
    }
    
    public void addBook(String title) {
        contents.add(title);
    }
    
    public void removeBook(String title) throws BookException {
        boolean result = contents.remove(title);
        if (!result) {
            throw new BookException("\"" + title + "\" not in cart.");
        }
    }
    
    @Remove
    public void remove() {
        contents = null;  // Container remove o bean após esta chamada
    }
}
```

**Observações Técnicas:**
- A anotação `@Stateful` indica que este é um bean de sessão com estado
- O método anotado com `@Remove` instrui o container a remover o bean após a execução
- Métodos de negócio NÃO podem começar com `ejb` (ex: `ejbCreate`) para evitar conflitos com métodos de callback

#### Singleton Session Bean (Bean de Sessão Singleton)

**Características:**
- Uma única instância para toda a aplicação
- Thread-safe por padrão (acesso sincronizado)
- Útil para caches, contadores globais, configurações

**Exemplo:**

```java
@Singleton
public class CounterBean {
    private int count = 0;
    
    public int increment() {
        return ++count;
    }
    
    public int getCount() {
        return count;
    }
}
```

### 3.2 Entity Beans (Beans de Entidade) - Obsoleto

Entity beans representam objetos de negócio persistentes em um banco de dados (ex: clientes, pedidos, produtos). Cada instância de um entity bean corresponde a uma linha em uma tabela do banco de dados.

**Estado Atual:** Entity beans foram **substituídos pela JPA (Java Persistence API)** nas versões modernas do Java EE/Jakarta EE. Para novas aplicações, recomenda-se usar JPA com anotações `@Entity` em POJOs.

### 3.3 Message-Driven Beans (MDB)

**Características:**
- Escutam mensagens do Java Message Service (JMS)
- Processam mensagens de forma assíncrona
- Não possuem interfaces remote/home — o container chama diretamente o método `onMessage()`

**Exemplo:**

```java
@MessageDriven(mappedName = "jms/Queue")
public class OrderProcessorBean implements MessageListener {
    
    @Resource
    private MessageDrivenContext context;
    
    public void onMessage(Message message) {
        try {
            TextMessage textMsg = (TextMessage) message;
            String orderData = textMsg.getText();
            // Processa o pedido...
        } catch (JMSException e) {
            context.setRollbackOnly();  // Marca transação para rollback
        }
    }
}
```

---

## 4. Interfaces do EJB (EJB 2.x e Anterior)

Para EJBs que permitem acesso remoto, são necessárias interfaces específicas:

### 4.1 Remote Interface (Interface Remota)

Define a visão do cliente sobre o EJB. É declarada usando sintaxe RMI.

```java
import java.rmi.RemoteException;
import javax.ejb.EJBObject;

public interface Demo extends EJBObject {
    public String demoSelect() throws RemoteException;
}
```

### 4.2 Home Interface

Fornece o mecanismo pelo qual o container cria novos EJBs em nome do cliente.

```java
import javax.ejb.EJBHome;
import java.rmi.RemoteException;

public interface DemoHome extends EJBHome {
    public Demo create() throws CreateException, RemoteException;
}
```

### 4.3 Bean Class (Classe do Bean)

Contém a implementação da lógica de negócio e métodos do ciclo de vida.

```java
import javax.ejb.SessionBean;
import javax.ejb.SessionContext;

public class DemoBean implements SessionBean {
    private SessionContext ctx;
    
    public void setSessionContext(SessionContext ctx) {
        this.ctx = ctx;
    }
    
    public void ejbCreate() {
        // Inicialização
    }
    
    public void ejbRemove() {
        // Limpeza
    }
    
    // Método de negócio
    public String demoSelect() {
        return "hello world";  // A lógica de negócio REAL
    }
}
```

**IMPORTANTE:** A implementação do método de negócio (como `demoSelect()`) é a única parte onde o desenvolvedor realmente inventa código — todo o resto são declarações de interfaces ou implementações de métodos do ciclo de vida.

### 4.4 Deployment Descriptor (Descritor de Implantação)

Arquivo XML (`ejb-jar.xml`) que descreve como os EJBs devem ser gerenciados pelo container. Define:
- Quais beans estão no pacote
- Configurações de transação
- EJB QL para métodos de busca
- Configurações de segurança e pooling

**Vantagem:** Configurações podem ser alteradas sem modificar o código-fonte dos beans.

---

## 5. Ciclo de Vida dos Session Beans

### Stateless Session Bean

```
[Pool de Instâncias] → [Instância Criada] → [Pronta para Uso] → [Método de Negócio] → [Devolvida ao Pool]
```

**Eventos:**
- O container mantém um pool de instâncias stateless
- Quando um cliente chama um método, uma instância é retirada do pool
- Após a execução, a instância retorna ao pool
- **Não há estado entre chamadas**

### Stateful Session Bean

```
[Instância Criada] → [Pronta para Uso] ↔ [Passivação] ↔ [Ativação] → [Remoção]
```

**Eventos:**
- **@PostConstruct:** Após criação e injeção de dependências
- **@PrePassivate:** Antes de ser movida para armazenamento secundário
- **@PostActivate:** Após ser restaurada do armazenamento secundário
- **@PreDestroy:** Antes da remoção (após método @Remove)
- **@Remove:** Remove a instância do container após a execução

**Observação:** Métodos de callback devem retornar `void` e não ter parâmetros.

---

## 6. Por que EJB ainda é relevante?

Apesar de ser considerada uma tecnologia mais antiga, o EJB ainda oferece vantagens significativas:

### Vantagens Técnicas

1. **Transações Automáticas:** Com apenas `@Stateless` ou `@Singleton`, você obtém transações gerenciadas automaticamente
2. **Concorrência Simplificada:** Singleton EJBs são thread-safe; Stateless não mantêm estado, evitando problemas de concorrência
3. **Timer Service:** Agendamento simplificado com `@Schedule` e `@Timeout`
4. **Segurança:** `@RolesAllowed` funciona nativamente em EJBs
5. **Inicialização na Startup:** `@Startup` em Singleton EJBs executa código na inicialização da aplicação

### Comparação com CDI (Contexts and Dependency Injection)

Alguns desenvolvedores argumentam que EJBs podem ser substituídos por CDI, mas há casos onde o EJB ainda é mais simples:

| Funcionalidade | EJB | CDI Puro |
|----------------|-----|----------|
| Transações | `@Transactional` automático | Requer código adicional |
| Thread-safety | Singleton EJB é seguro | `@ApplicationScoped` não é thread-safe |
| Timer Service | `@Schedule` integrado | Precisa de implementação manual |
| Startup | `@Startup` em Singleton | `@Observes` com evento de startup |

---

## 7. Exemplo Completo: Aplicação de Conversão

Este exemplo demonstra um stateless session bean que converte dólares para ienes e euros, com um servlet como cliente web.

### Passo 1: Criar o EJB

```java
package ee.jakarta.tutorial.converter.ejb;

import java.math.BigDecimal;
import jakarta.ejb.Stateless;

@Stateless
public class ConverterBean {
    private BigDecimal yenRate = new BigDecimal("83.0602");
    private BigDecimal euroRate = new BigDecimal("0.0093016");

    public BigDecimal dollarToYen(BigDecimal dollars) {
        return dollars.multiply(yenRate)
                      .setScale(2, BigDecimal.ROUND_UP);
    }

    public BigDecimal yenToEuro(BigDecimal yen) {
        return yen.multiply(euroRate)
                  .setScale(2, BigDecimal.ROUND_UP);
    }
}
```

### Passo 2: Criar o Cliente (Servlet)

```java
package converter.web;

import ee.jakarta.tutorial.converter.ejb.ConverterBean;
import jakarta.ejb.EJB;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

@WebServlet(urlPatterns="/")
public class ConverterServlet extends HttpServlet {
    
    @EJB
    private ConverterBean converter;
    
    protected void doGet(HttpServletRequest request, 
                         HttpServletResponse response) {
        String amount = request.getParameter("amount");
        if (amount != null && !amount.isEmpty()) {
            BigDecimal dollars = new BigDecimal(amount);
            BigDecimal yenAmount = converter.dollarToYen(dollars);
            BigDecimal euroAmount = converter.yenToEuro(yenAmount);
            // Exibe resultados...
        }
    }
}
```

### Passo 3: Empacotar e Deploy

1. **Empacotamento:** Os EJBs vão em um arquivo `ejb-jar` ou `war` com o descritor de implantação
2. **Deploy:** Implante no servidor de aplicação (GlassFish, WildFly, WebLogic)
3. **Acesso:** Acesse `http://localhost:8080/converter`

**Observação:** O container EJB gerencia automaticamente transações, pooling e ciclo de vida do bean.

---

## 8. Considerações Finais

### Quando usar EJB?

**Use EJB quando:**
- Precisar de transações declarativas automáticas
- Requerer segurança integrada (`@RolesAllowed`)
- Desenvolver aplicações Jakarta EE completas
- Precisar de Timer Service para agendamentos

**Considere alternativas quando:**
- Aplicação for simples e não precisar de transações distribuídas
- Estiver usando Spring Framework (que tem sua própria abordagem)
- Preferir arquiteturas mais leves e modernas

### Evolução do EJB

O EJB moderno (versões 3.x e superiores) é significativamente mais simples que as versões anteriores. A especificação atual enfatiza o uso de anotações em vez de XML, e os EJBs são essencialmente POJOs com anotações, tornando-os mais fáceis de desenvolver e testar.

---

## Referências

- Jakarta EE Tutorial - Enterprise Beans
- Oracle WebLogic Documentation - EJB Overview
- EJB 1.1 Specification
- NYU Course Materials - Building Stateless Session Beans
