# SalesHub  

Projeto desenvolvido como parte do curso **Arquitetura de Microsserviços: Padrão Saga Orquestrado** na Udemy. O projeto explora o padrão **Saga Orquestrado** para garantir consistência em transações distribuídas, utilizando uma arquitetura baseada em microsserviços com comunicação assíncrona via **Kafka**.  

---

## 🚀 Visão Geral  
O **SalesHub** é uma aplicação de gerenciamento de pedidos que implementa o padrão **Saga Orquestrado** para garantir consistência em sistemas distribuídos. A aplicação utiliza microsserviços para gerenciar diferentes domínios do processo de venda, como validação de produtos, pagamento e atualização de estoque.  

---

## 🛠️ Tecnologias e Ferramentas  
- **[Java](https://www.java.com/pt-BR/):** Linguagem principal para implementação dos microsserviços  
- **[Spring Boot](https://spring.io/projects/spring-boot):** Framework para criação dos serviços REST e configuração simplificada  
- **[Apache Kafka](https://kafka.apache.org/):** Comunicação assíncrona entre microsserviços  
- **[PostgreSQL](https://www.postgresql.org/):** Banco de dados relacional para persistência de dados  
- **[MongoDB](https://www.mongodb.com/):** Banco de dados NoSQL para armazenar pedidos e eventos  
- **[Docker](https://www.docker.com/):** Contêinerização dos microsserviços e orquestração dos contêineres  
- **[Redpanda Console](https://www.redpanda.com/):** Interface para monitoramento de eventos Kafka  

---

## 🏛️ Arquitetura Proposta  
A arquitetura é baseada em **5 microsserviços** que se comunicam através de eventos Kafka:  

| **Microsserviço**             | **Função**                               | **Banco de Dados** | **Porta** |
|--------------------------------|-----------------------------------------|--------------------|-----------|
| **Order-Service**              | Criação e consulta de pedidos           | MongoDB            | 3000       |
| **Orchestrator-Service**       | Orquestração do fluxo da saga           | -                  | 8080       |
| **Product-Validation-Service** | Validação de produtos e quantidades     | PostgreSQL         | 8090       |
| **Payment-Service**            | Processamento de pagamento              | PostgreSQL         | 8091       |
| **Inventory-Service**          | Atualização de estoque                  | PostgreSQL         | 8092       |  

---

## 🔄 Fluxo da Saga  
1. **Order-Service** cria um pedido.  
2. **Orchestrator-Service** recebe o evento e aciona os demais serviços:  
   - Validação de produtos  
   - Processamento de pagamento  
   - Atualização de estoque  
3. Cada serviço emite um evento para o próximo estágio da saga.  
4. O **Orchestrator** gerencia o sucesso ou falha do fluxo.  

---

## 🚀 Setup do Projeto  

### ✅ **Pré-requisitos**  
Certifique-se de que os seguintes softwares estejam instalados:  
- **Java 17** – Para executar os microsserviços.  
- **Docker 17.06.0** – Para subir os serviços via container.  
- **Maven 3.3.3** – Para compilar e construir o projeto.  
- **Python 3** *(opcional)* – Para automação de tarefas.  

---

## 📥 **Instalação**  

### 1. Clone o repositório:  
```bash
git clone https://github.com/carolinaoliveirasilveira/saleshub.git
```
### 2. Acesse o diretório do projeto:  
```bash
cd saleshub
```
### 3. Compile o código e baixe as dependências do projeto:  
```bash
mvn clean package
```
### 4. Suba os serviços de banco de dados e Kafka utilizando Docker Compose:  
```bash
docker-compose up -d
```
A aplicação estará disponível em [http://localhost:8081](http://localhost:8081).  

---

## 🌐 Execução  
Após iniciar os serviços, eles estarão disponíveis nas seguintes portas:  

| **Microsserviço**                | **URL**                             |
|-----------------------------------|-------------------------------------|
| **Order-Service**                | [http://localhost:3000](http://localhost:3000) |
| **Orchestrator-Service**         | [http://localhost:8080](http://localhost:8080) |
| **Product-Validation-Service**   | [http://localhost:8090](http://localhost:8090) |
| **Payment-Service**              | [http://localhost:8091](http://localhost:8091) |
| **Inventory-Service**            | [http://localhost:8092](http://localhost:8092) |

---

## 🔎 **Verificando Logs**  
Ao iniciar os microsserviços, você verá logs semelhantes a este:  
```shell
Tomcat started on port(s): 3000 (http)
Started OrderServiceApplication in xxxx seconds (JVM running for xxxx)
```

---

## 📡 **APIs Disponíveis**  

### **Order-Service**  
- `POST /orders/create` – Cria um novo pedido.  
- `GET /orders/{id}` – Retorna os detalhes de um pedido pelo ID.  

### **Product-Validation-Service**  
- `POST /products/validate` – Valida os produtos e quantidades de um pedido.  

### **Payment-Service**  
- `POST /payments/process` – Processa o pagamento de um pedido.  

### **Inventory-Service**  
- `POST /inventory/update` – Atualiza o estoque após confirmação do pedido.  

### **Orchestrator-Service**  
- `POST /orchestrator/start` – Inicia o fluxo da saga para um novo pedido.  
- `POST /orchestrator/rollback` – Realiza o rollback em caso de falha no fluxo.  

---

## 🧪 **Testando os Microsserviços**  

Use **Postman** ou **cURL** para testar as APIs após iniciar os serviços.  

### **Exemplo: Criar Pedido**  
```bash
curl -X POST http://localhost:3000/orders/create \
-H "Content-Type: application/json" \
-d '{
  "productId": "12345",
  "quantity": 2,
  "price": 150.00,
  "customerId": "carolina"
}'
```

### **Exemplo: Validar Produto**  
```bash
curl -X POST http://localhost:8090/products/validate \
-H "Content-Type: application/json" \
-d '{
  "productId": "12345",
  "quantity": 2
}'
```

---

## 👨‍💻 **Desenvolvido por**  
```text
Carolina Oliveira Silveira
📧 carolinaoliveirasilveira@outlook.com
🌐 LinkedIn: https://www.linkedin.com/in/carolinaoliveirasilveira/
🌐 GitHub: https://github.com/carolinaoliveirasilveira
```
