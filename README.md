# SalesHubPro – Padrão Saga Orquestrado

Repositório contendo o projeto **SalesHubPro** desenvolvido a partir do curso [Arquitetura de Microsserviços: Padrão Saga Orquestrado](https://www.udemy.com/course/arquitetura-de-microsservicos-padrao-saga-orquestrado) na **Udemy**.

Este projeto explora o padrão Saga Orquestrado para garantir consistência de dados em operações distribuídas em microsserviços.

---

## Tecnologias Utilizadas

- **[Java](https://www.java.com/pt-BR/):** Linguagem de programação orientada a objetos, robusta e amplamente utilizada para desenvolvimento de aplicações empresariais.
- **[Spring Boot](https://spring.io/projects/spring-boot):** Framework que facilita o desenvolvimento de aplicações Java com configuração automatizada, injeção de dependência, servidor embutido e suporte para microserviços.
- **[Apache Kafka](https://kafka.apache.org/):** Plataforma de streaming distribuída para processamento em tempo real, permitindo a comunicação assíncrona entre microsserviços por meio de tópicos.
- **[MongoDB](https://www.mongodb.com/):** Banco de dados NoSQL orientado a documentos, ideal para armazenar e consultar dados não estruturados.
- **[PostgreSQL](https://www.postgresql.org/):** Banco de dados relacional open source, conhecido por sua extensibilidade e conformidade com o padrão SQL.
- **[Docker](https://www.docker.com/):** Plataforma para containerização que permite empacotar e distribuir aplicações com suas dependências, facilitando a portabilidade e escalabilidade.
- **[Redpanda Console](https://www.redpanda.com/):** Ferramenta para monitorar e gerenciar clusters Kafka de maneira simplificada, oferecendo uma interface amigável para visualizar tópicos, mensagens e status de consumidores.

---

## Ferramentas Utilizadas

- **IntelliJ IDEA Community Edition**
- **Docker**
- **Gradle**

---

## Arquitetura Proposta

A arquitetura é baseada em cinco microsserviços independentes que se comunicam por meio de eventos Kafka, seguindo o padrão Saga Orquestrado:

### **Serviços:**
- **Order-Service** – Responsável por gerar o pedido inicial e receber notificações. Usa MongoDB.
- **Orchestrator-Service** – Orquestra o fluxo da saga, coordenando a execução dos serviços.
- **Product-Validation-Service** – Valida os produtos informados no pedido. Usa PostgreSQL.
- **Payment-Service** – Processa o pagamento com base nos valores informados. Usa PostgreSQL.
- **Inventory-Service** – Realiza a baixa do estoque dos produtos de um pedido. Usa PostgreSQL.

---

## Instalação da aplicação

1. Primeiramente, faça o clone do repositório:
   ```bash
   git clone https://github.com/carolinaoliveirasilveira/SalesHub.git
   ```
2. Acesse o diretório do projeto:
   ```bash
   cd SalesHub
   ```
3. Compile o código e baixe as dependências do projeto:
   ```bash
   mvn clean package
   ```
4. Inicie a aplicação:
   ```bash
   mvn spring-boot:run
   ```

   ---

## Acessando a Aplicação
| **Serviço**                   | **URL**                               |
|--------------------------------|---------------------------------------|
| **Order-Service**              | http://localhost:3000/swagger-ui.html |
| **Orchestrator-Service**       | http://localhost:8080                |
| **Product-Validation-Service** | http://localhost:8090                |
| **Payment-Service**            | http://localhost:8091                |
| **Inventory-Service**          | http://localhost:8092                |
| **Redpanda Console**           | http://localhost:8081                |  


 **Dados da API**  
 **Produtos Registrados e Estoque**  
| **Produto**     | **Estoque** |
|-----------------|------------|
| COMIC_BOOKS     | 4          |
| BOOKS           | 2          |
| MOVIES          | 5          |
| MUSIC           | 9          |

---

 **Endpoint para iniciar a saga**  
**POST** `http://localhost:3000/api/order`  

**Payload:**  
```json
{
  "products": [
    {
      "product": {
        "code": "COMIC_BOOKS",
        "unitValue": 15.50
      },
      "quantity": 3
    },
    {
      "product": {
        "code": "BOOKS",
        "unitValue": 9.90
      },
      "quantity": 1
    }
  ]
}
  ```

**Endpoint para visualizar a saga**  
Recuperar os dados da saga por `orderId` ou `transactionId`:  
```bash
GET http://localhost:3000/api/event?orderId=64429e987a8b646915b3735f
GET http://localhost:3000/api/event?transactionId=1682087576536_99d2ca6c-f074-41a6-92e0-21700148b519
```
 **Status da Saga**  
| **Status**  | **Descrição**                                       |
|------------|-----------------------------------------------------|
| **PENDING** | Pedido iniciado, aguardando processamento           |
| **SUCCESS** | Pedido processado com sucesso                       |
| **FAILED**  | Falha em algum serviço                               |
| **ROLLBACK**| Pedido revertido devido a falha                      |

**Autora** Carolina Oliveira Silveira
