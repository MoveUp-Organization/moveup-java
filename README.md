
<div align="center">

# MoveUP — API REST em Java

### Sustentabilidade em movimento

**Challenge SoulUp • FIAP • Sprint 4 — Domain Driven Design Using Java**

![Java](https://img.shields.io/badge/Java-21-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-4.1.1-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Oracle](https://img.shields.io/badge/Oracle-Database-F80000?style=flat-square&logo=oracle&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-Build-C71A36?style=flat-square&logo=apachemaven&logoColor=white)

</div>

---

## Sobre o projeto

O **MoveUP** é uma solução acadêmica que incentiva a mobilidade sustentável por meio de missões e **MovePoints**, que podem ser convertidos em benefícios relacionados ao transporte público.

Este repositório contém o **back-end Java** do projeto: uma API REST desenvolvida com **Spring Boot**, organizada em camadas para receber requisições HTTP, aplicar regras de negócio e acessar o banco de dados **Oracle** por meio de JDBC.

A API disponibiliza operações de cadastro, consulta, atualização e exclusão para **usuários, vouchers e históricos**, utilizadas na integração com o front-end do MoveUP.

## Tecnologias utilizadas

| Tecnologia | Utilização |
| --- | --- |
| **Java 21** | Linguagem principal |
| **Spring Boot 4.1.1** | Aplicação web e endpoints REST |
| **Spring Web MVC** | Controllers e respostas HTTP |
| **Jakarta Bean Validation** | Anotações para validação de dados |
| **Oracle Database / JDBC** | Persistência de informações no banco relacional |
| **Maven / Maven Wrapper** | Gerenciamento de dependências e compilação |
| **Docker** | Empacotamento e execução da aplicação em contêiner |
| **Render** | Endereço de hospedagem configurado para a API |

## Arquitetura e estrutura de pastas

O código separa objetos de transferência de dados, regras de negócio, persistência e recursos HTTP.

```text
moveup-java/
├── .mvn/wrapper/                 # Configurações do Maven Wrapper
├── src/
│   ├── main/
│   │   ├── java/br/com/fiap/
│   │   │   ├── MoveupApplication.java
│   │   │   ├── to/
│   │   │   │   ├── UsuarioTO.java
│   │   │   │   ├── VoucherTO.java
│   │   │   │   └── HistoricoTO.java
│   │   │   ├── bo/
│   │   │   │   ├── UsuarioBO.java
│   │   │   │   ├── VoucherBO.java
│   │   │   │   └── HistoricoBO.java
│   │   │   ├── dao/
│   │   │   │   ├── ConnectionFactory.java
│   │   │   │   ├── UsuarioDAO.java
│   │   │   │   ├── VoucherDAO.java
│   │   │   │   └── HistoricoDAO.java
│   │   │   └── resource/
│   │   │       ├── CorsFilter.java
│   │   │       ├── UsuarioResource.java
│   │   │       ├── VoucherResource.java
│   │   │       └── HistoricoResource.java
│   │   └── resources/
│   │       └── application.properties
│   └── test/java/br/com/fiap/
│       └── MoveupApplicationTests.java
├── Dockerfile
├── pom.xml
├── mvnw
├── mvnw.cmd
└── README.md
```

**Responsabilidades das camadas:**

- **TO (Transfer Object):** define os dados das entidades e suas restrições de validação.
- **BO (Business Object):** concentra validações e regras de negócio.
- **DAO (Data Access Object):** executa operações SQL com `PreparedStatement`.
- **Resource:** expõe as rotas REST e organiza as respostas HTTP.
- **ConnectionFactory:** lê as configurações de conexão com o banco de dados.
- **CorsFilter:** configura os cabeçalhos de acesso entre origens.

## Funcionalidades implementadas no código

- **Usuários:** cadastro, listagem, consulta por ID, atualização e exclusão.
- **Vouchers/cartões:** cadastro, listagem, consulta por código, atualização de dados e saldo, e exclusão.
- **Históricos:** registro, listagem, consulta por ID, atualização e exclusão de operações relacionadas ao transporte.
- **Validações:** campos obrigatórios, formato de e-mail, regras de senha, status, saldos e quantidades não negativas.

## Endpoints da API

**URL-base utilizada pelo front-end:** `https://moveup-89cd.onrender.com/moveup`

| Método | Endpoint | Finalidade | Status esperado* |
| --- | --- | --- | --- |
| `GET` | `/usuario` | Listar usuários | `200` |
| `GET` | `/usuario/{idUsuario}` | Consultar usuário por ID | `200`, `404` |
| `POST` | `/usuario` | Cadastrar usuário | `201`, `400` |
| `PUT` | `/usuario/{idUsuario}` | Atualizar usuário | `200`, `400`, `404` |
| `DELETE` | `/usuario/{idUsuario}` | Excluir usuário | `204`, `404` |
| `GET` | `/voucher` | Listar vouchers | `200` |
| `GET` | `/voucher/{cdCartao}` | Consultar voucher por código | `200`, `404` |
| `POST` | `/voucher` | Cadastrar voucher | `201`, `400` |
| `PUT` | `/voucher/{cdCartao}` | Atualizar voucher | `200`, `400`, `404` |
| `DELETE` | `/voucher/{cdCartao}` | Excluir voucher | `204`, `404` |
| `GET` | `/historico` | Listar históricos | `200` |
| `GET` | `/historico/{idHistorico}` | Consultar histórico por ID | `200`, `404` |
| `POST` | `/historico` | Cadastrar histórico | `201`, `400` |
| `PUT` | `/historico/{idHistorico}` | Atualizar histórico | `200`, `400`, `404` |
| `DELETE` | `/historico/{idHistorico}` | Excluir histórico | `204`, `404` |

\* **Atenção:** os códigos acima representam o comportamento REST desejado. Na versão examinada, os métodos `PUT` ainda retornam `201 Created`, e alguns tratamentos de erro devem ser revisados. A documentação final em PDF deve registrar os códigos efetivamente validados após essas correções. Falhas internas podem resultar em `500`.

### Exemplos de requisições

Listar usuários:

```bash
curl https://moveup-89cd.onrender.com/moveup/usuario
```

Consultar um voucher pelo código cadastrado:

```bash
curl https://moveup-89cd.onrender.com/moveup/voucher/1
```

Cadastrar um usuário de demonstração (exige Oracle disponível):

```bash
curl -X POST https://moveup-89cd.onrender.com/moveup/usuario \
  -H "Content-Type: application/json" \
  -d '{"nomeUsuario":"Usuario Exemplo","email":"exemplo@moveup.test","senha":"Exemplo123!"}'
```

> Não execute inserções de teste no banco de produção sem autorização da equipe. A aplicação Java atual não implementa autenticação de login; o login do front-end é demonstrativo.

## Como executar localmente

### Pré-requisitos

- **JDK 21** instalado.
- Conexão com um **Oracle Database** contendo as tabelas esperadas pelos DAOs (`ddd_usuario`, `ddd_voucher` e `ddd_historico`).
- Acesso ao terminal e, opcionalmente, **VS Code, IntelliJ IDEA ou Eclipse**.

O repositório inclui o **Maven Wrapper** (`mvnw` e `mvnw.cmd`), dispensando a instalação manual do Maven em instalações compatíveis.

### 1. Clonar o repositório

```bash
git clone https://github.com/MoveUp-Organization/moveup-java.git
cd moveup-java
```

### 2. Configurar a conexão com o Oracle

A classe `ConnectionFactory` lê estas variáveis de ambiente:

| Variável | Descrição |
| --- | --- |
| `DB_URL` | URL JDBC do Oracle |
| `DB_USER` | Usuário do banco |
| `DB_PASSWORD` | Senha do banco |

Exemplo de configuração **no PowerShell** (substitua pelos dados reais de seu ambiente):

```powershell
$env:DB_URL = "jdbc:oracle:thin:@//SEU_HOST:1521/SEU_SERVICO"
$env:DB_USER = "SEU_USUARIO"
$env:DB_PASSWORD = "SUA_SENHA"
```

Não publique credenciais, arquivos locais de configuração ou senhas no GitHub.

### 3. Executar a aplicação

No **Windows (PowerShell ou CMD)**:

```powershell
.\mvnw.cmd spring-boot:run
```

No **Linux/macOS**:

```bash
./mvnw spring-boot:run
```

Por padrão, o Spring Boot inicia na porta `8080`, quando disponível. Com o servidor local ativo, um exemplo de consulta é:

```text
http://localhost:8080/moveup/usuario
```

### 4. Gerar o pacote da aplicação

Windows:

```powershell
.\mvnw.cmd clean package
```

Linux/macOS:

```bash
./mvnw clean package
```

O `.jar` compilado é gerado na pasta `target/`, desde que a compilação seja concluída com sucesso.

## Deploy e integração com o front-end

- **API Java (endereço configurado no Render):** https://moveup-89cd.onrender.com/moveup
- **Front-end publicado na Vercel:** https://moveupbr.vercel.app
- **Código do front-end:** https://github.com/MoveUp-Organization/MoveUP-Sprint4-front

No front-end Vite, a URL-base é configurada pela variável:

```dotenv
VITE_API_URL=https://moveup-89cd.onrender.com/moveup
```

O front-end consome a API para cadastrar usuários, criar e consultar vouchers, atualizar o saldo de cartões e registrar históricos de conversão. Algumas funcionalidades, como o login demonstrativo e os MovePoints, também utilizam armazenamento local no navegador.

**Importante:** os links acima são os endereços utilizados/configurados pelo projeto. Antes da entrega final, a equipe deve comprovar o funcionamento do deploy, das operações CRUD e da integração entre front-end, API e banco de dados.

## Documentação e entrega da Sprint 4

Conforme as orientações da disciplina **Domain Driven Design Using Java**, o repositório deve disponibilizar:

- **Código-fonte Java** atualizado (presente neste repositório).
- **Pasta do projeto compactada em `.zip`** (arquivo de entrega a adicionar).
- **Documentação final em PDF** (arquivo de entrega a adicionar), contendo capa, equipe e integrantes, objetivo, escopo, funcionalidades, tabela de endpoints e status, prints reais, MER, diagrama de classes atualizado e instruções de execução.
- **Link do deploy na nuvem**, indicado na seção anterior.

> O README complementa a documentação, mas **não substitui o PDF e o ZIP** exigidos pelo professor. Os diagramas devem corresponder às classes e ao modelo do banco entregues.

## Integrantes — Equipe MoveUP

**FIAP — Turma 1TDSPH • Challenge SoulUp**

| Integrante | RM | GitHub |
| --- | --- | --- |
| Isabelle Ferreira Neri Feitoza | 573507 | [isabelleferreiraa](https://github.com/isabelleferreiraa) |
| Milena Silva Conegin | 568923 | [MilenaConegin](https://github.com/MilenaConegin) |
| Eduardo Damasio Guelere | 569960 | [Eduardoguelere](https://github.com/Eduardoguelere) |
| Marina Fernandes Gomes Mesquita | 571265 | [marifernandesgm](https://github.com/marifernandesgm) |

---

<div align="center">

**MoveUP — Sustentabilidade em movimento**  
FIAP • Sprint 4 • 2026

</div>
