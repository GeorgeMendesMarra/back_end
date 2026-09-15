# ☕ Back-End — Java e Tecnologias do Lado Servidor

![Java](https://img.shields.io/badge/Java-ED8B00?style=flat&logo=openjdk&logoColor=white) ![Back-End](https://img.shields.io/badge/Desenvolvimento-Back--End-blue) ![Spring](https://img.shields.io/badge/Spring-6DB33F?style=flat&logo=spring&logoColor=white) ![Hibernate](https://img.shields.io/badge/Hibernate-59666C?style=flat&logo=hibernate&logoColor=white) ![Java EE](https://img.shields.io/badge/Java%20Enterprise-5382A1?style=flat&logo=java&logoColor=white) ![Status](https://img.shields.io/badge/status-em%20desenvolvimento-success)

# 📚 Sobre o Repositório

Este repositório foi criado para centralizar **materiais didáticos, exemplos práticos, códigos-fonte, exercícios e experimentos relacionados ao desenvolvimento Back-End**, com ênfase no ecossistema **Java** e nas tecnologias utilizadas no desenvolvimento de aplicações corporativas.

O conteúdo é destinado principalmente a estudantes de graduação e pós-graduação, desenvolvedores iniciantes e profissionais que desejam aprofundar seus conhecimentos em **programação do lado servidor, persistência de dados, componentes corporativos, frameworks Spring e arquiteturas de aplicações Java**.

A proposta é estabelecer uma ponte entre os **fundamentos teóricos da programação Back-End** e sua aplicação em projetos reais.

---

# 🎯 Objetivos

Este repositório tem como principais objetivos:

- ☕ Explorar o desenvolvimento Back-End utilizando Java;
- 🏗️ Apresentar arquiteturas e padrões utilizados em aplicações servidor;
- 🧩 Demonstrar componentes e tecnologias do ecossistema Java Enterprise;
- 🗄️ Trabalhar persistência e acesso a dados;
- 🔄 Apresentar o mapeamento objeto-relacional;
- 🍃 Explorar o framework Spring e seus principais módulos;
- 📦 Demonstrar organização e estruturação de aplicações;
- 🧪 Fornecer exemplos práticos para aulas e estudos;
- 📖 Servir como material de apoio acadêmico;
- 🚀 Estabelecer fundamentos para o desenvolvimento de aplicações corporativas.

---

# 🏛️ Arquitetura do Repositório

O repositório está organizado em módulos independentes, permitindo que cada tecnologia seja estudada de maneira progressiva.

```
back_end/
│
├── enterprise_java_beans/
│   └── Exemplos relacionados a componentes corporativos Java
│
├── hibernate/
│   └── Exemplos relacionados à persistência e ORM
│
├── spring/
│   ├── spring-boot.md
│   ├── spring-mvc.md
│   ├── spring-data-jpa.md
│   ├── spring-security.md
│   └── spring-cloud.md
│
└── README.md
```

---

# ☕ Enterprise Java Beans — EJB

O diretório `enterprise_java_beans` concentra exemplos relacionados ao **Enterprise JavaBeans (EJB)** e ao desenvolvimento de componentes para aplicações corporativas.

Os estudos permitem compreender conceitos como:

- Componentes corporativos;
- Arquitetura Java Enterprise;
- Gerenciamento de componentes;
- Injeção de dependências;
- Serviços transacionais;
- Persistência;
- Comunicação entre componentes;
- Separação de responsabilidades;
- Aplicações distribuídas;
- Desenvolvimento de sistemas corporativos.

📁 Diretório:

`enterprise_java_beans/`

---

# 🗄️ Hibernate

O diretório `hibernate` é dedicado ao estudo do **Hibernate**, uma das principais tecnologias utilizadas para persistência de objetos Java em bancos de dados relacionais.

Entre os conceitos trabalhados estão:

- ORM — Object-Relational Mapping;
- Mapeamento objeto-relacional;
- Entidades;
- Relacionamentos;
- Persistência de objetos;
- Consultas;
- Transações;
- Sessões;
- Mapeamento de tabelas;
- Chaves primárias;
- Associações entre entidades;
- Integração entre Java e bancos de dados.

📁 Diretório:

`hibernate/`

---

# 🍃 Spring

O diretório `spring` reúne material didático sobre o **framework Spring**, hoje o padrão de mercado para desenvolvimento Back-End em Java, cobrindo desde a criação de aplicações até arquiteturas de microsserviços.

| Arquivo | Módulo | Finalidade |
| ------- | ------ | ---------- |
| [`spring-boot.md`](spring/spring-boot.md) | **Spring Boot** | Configuração automática, servidor embutido e criação simplificada de aplicações |
| [`spring-mvc.md`](spring/spring-mvc.md) | **Spring MVC** | Construção de APIs REST e aplicações web (padrão MVC) |
| [`spring-data-jpa.md`](spring/spring-data-jpa.md) | **Spring Data JPA** | Persistência e acesso a dados com JPA/Hibernate |
| [`spring-security.md`](spring/spring-security.md) | **Spring Security** | Autenticação, autorização e proteção de endpoints (JWT, roles) |
| [`spring-cloud.md`](spring/spring-cloud.md) | **Spring Cloud** | Arquiteturas de microsserviços (Config, Eureka, Gateway, Feign) |

Cada arquivo segue a mesma estrutura: conceito, principais componentes, exemplo de código e quando usar o módulo.

📁 Diretório:

`spring/`

---

# 🔗 Relação entre Java, Back-End e Banco de Dados

Um dos objetivos deste projeto é demonstrar como os diferentes componentes de uma aplicação Back-End podem trabalhar em conjunto.

Uma arquitetura simplificada pode ser representada da seguinte forma:

```
┌──────────────────────────────┐
│          Cliente             │
│ Browser / Aplicação / API    │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│          Back-End            │
│      Java / Spring Boot      │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│     Componentes / Regras     │
│  EJB / Spring MVC / Serviços │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│        Persistência          │
│   Hibernate / Spring Data    │
│             ORM              │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│       Banco de Dados         │
│ PostgreSQL / MySQL / Oracle  │
└──────────────────────────────┘
```

Essa representação permite compreender uma das ideias fundamentais do desenvolvimento Back-End:
> **A aplicação não é apenas código. Ela é composta por diferentes camadas e tecnologias que trabalham conjuntamente para processar regras de negócio, persistir informações e disponibilizar serviços.**

---

# 🧱 Conceitos Fundamentais

O estudo dos exemplos deste repositório está relacionado a conceitos fundamentais do desenvolvimento de software.

### Programação Orientada a Objetos

- Classes;
- Objetos;
- Encapsulamento;
- Herança;
- Polimorfismo;
- Abstração;
- Interfaces.

### Desenvolvimento Back-End

- Regras de negócio;
- Camadas da aplicação;
- Serviços;
- Persistência;
- Transações;
- APIs;
- Segurança;
- Integração com bancos de dados.

### Persistência

- CRUD;
- ORM;
- Entidades;
- Relacionamentos;
- Consultas;
- Transações;
- Mapeamento objeto-relacional.

### Arquitetura

- Separação de responsabilidades;
- Baixo acoplamento;
- Alta coesão;
- Componentização;
- Reutilização;
- Manutenibilidade;
- Escalabilidade.

---

# 🛠️ Tecnologias e Conceitos

| Tecnologia / Conceito    | Finalidade                                 |
| ------------------------ | ------------------------------------------ |
| ☕ Java                   | Linguagem principal                        |
| 🏢 Java Enterprise        | Desenvolvimento de aplicações corporativas |
| 🧩 EJB                    | Componentes e serviços corporativos        |
| 🗄️ Hibernate             | Persistência e ORM                         |
| 🔄 JPA                    | Persistência de objetos Java               |
| 🍃 Spring Boot            | Criação simplificada de aplicações Java    |
| 🌐 Spring MVC             | APIs REST e aplicações web                 |
| 🗃️ Spring Data JPA       | Persistência e acesso a dados              |
| 🔐 Spring Security        | Autenticação e autorização                 |
| ☁️ Spring Cloud           | Arquiteturas de microsserviços             |
| 🗃️ Banco de Dados        | Armazenamento persistente                  |
| 🏗️ Padrões Arquiteturais | Organização das aplicações                 |
| 🌐 APIs                   | Comunicação entre sistemas                 |

> Algumas tecnologias podem ser incorporadas ao repositório progressivamente conforme a evolução dos estudos e das disciplinas.

---

# 🎓 Aplicação Acadêmica

Este repositório pode ser utilizado como material de apoio em disciplinas relacionadas a:

- Programação Web;
- Desenvolvimento Back-End;
- Programação Orientada a Objetos;
- Engenharia de Software;
- Desenvolvimento de Sistemas;
- Banco de Dados;
- Arquitetura de Software;
- Desenvolvimento de Aplicações Corporativas;
- Java Enterprise.

---

# 👨‍🏫 Autor

## Professor George Mendes Marra

Professor e pesquisador na área de **Computação e Tecnologia da Informação**, com atuação acadêmica em desenvolvimento de software, programação, sistemas, segurança, inteligência artificial e tecnologias relacionadas à Computação.

Este repositório faz parte de um conjunto de materiais utilizados para **ensino, experimentação, pesquisa e compartilhamento de conhecimento**.

---

# 👥 Público-Alvo

Este projeto é destinado a:

- 🎓 Estudantes de graduação;
- 🎓 Estudantes de pós-graduação;
- 👨‍💻 Desenvolvedores iniciantes;
- 👨‍💻 Desenvolvedores Java;
- 🧑‍🏫 Professores;
- 🔬 Pesquisadores;
- 💼 Profissionais de Tecnologia da Informação;
- ☕ Entusiastas do ecossistema Java.

---

# 📈 Evolução do Projeto

O repositório está em **desenvolvimento contínuo**.

Novos exemplos e tecnologias poderão ser adicionados conforme a evolução das disciplinas, projetos acadêmicos e estudos relacionados ao desenvolvimento Back-End.

Possíveis evoluções incluem:

```
Java
 │
 ├── Programação Orientada a Objetos
 │
 ├── Java Web
 │
 ├── Servlets / JSP
 │
 ├── EJB
 │
 ├── JPA / Hibernate
 │
 ├── Spring ✅
 │   ├── Spring Boot ✅
 │   ├── Spring MVC ✅
 │   ├── Spring Data JPA ✅
 │   ├── Spring Security ✅
 │   └── Spring Cloud ✅
 │
 ├── APIs REST
 │
 ├── Microsserviços
 │
 └── Arquitetura de Sistemas
```

---

# 📖 Abordagem de Ensino

Os exemplos procuram seguir uma abordagem progressiva:

```
CONCEITO
   ↓
FUNDAMENTAÇÃO TEÓRICA
   ↓
EXEMPLO DE CÓDIGO
   ↓
EXECUÇÃO
   ↓
EXPERIMENTAÇÃO
   ↓
ANÁLISE
   ↓
APLICAÇÃO EM PROJETO
```

A intenção é que o estudante não apenas compreenda **como escrever o código**, mas também **por que a tecnologia existe, qual problema ela resolve e como ela se relaciona com os demais componentes de uma aplicação Back-End**.

---

# 🤝 Contribuições

Sugestões, correções e contribuições relacionadas ao conteúdo técnico e acadêmico são bem-vindas.

Para contribuir:

1. Faça um **Fork** do projeto;
2. Crie uma nova branch;
3. Realize suas alterações;
4. Documente o código quando necessário;
5. Faça o commit;
6. Envie um **Pull Request**.

As contribuições devem preservar o caráter **educacional, técnico e acadêmico** do projeto.

---

# ⭐ Apoie o Projeto

Se este repositório for útil para seus estudos, aulas ou projetos, considere deixar uma ⭐ no GitHub.

O compartilhamento de materiais acadêmicos e exemplos práticos contribui para a formação de novos profissionais na área de Computação.

---

## ☕ Java • 🏗️ Back-End • 🍃 Spring • 🗄️ Hibernate • 🧩 Java Enterprise

**Professor George Mendes Marra**

**Computação • Desenvolvimento de Software • Educação**
