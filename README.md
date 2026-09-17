# ServiceHub API 🛠️

![Java](https://img.shields.io/badge/Java-17-orange.svg)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-brightgreen.svg)
![MySQL](https://img.shields.io/badge/MySQL-8.0-blue.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

> API REST completa para gestão de **Ordens de Serviço (OS)**, controle de estoque de peças e faturamento atômico, focada em oficinas e empresas de manutenção técnica.

## 📌 O Problema
Empresas de manutenção precisam vincular serviços e peças aos veículos/ativos de seus clientes. O fechamento de uma OS envolve complexidades como a dedução do estoque e a imutabilidade do preço da peça no momento do serviço. O **ServiceHub API** resolve isso garantindo integridade transacional e histórico seguro.

## 🚀 Funcionalidades (Features)
- **Gestão de Clientes e Veículos:** Relacionamento 1:N com validações robustas.
- **Controle de Estoque:** Peças com controle rigoroso de saldo (prevenção de estoque negativo).
- **Ciclo de Vida da OS:** Abertura, inclusão de peças e mão de obra, atualização de status.
- **Transação Atômica (`@Transactional`):** O fechamento da OS garante a baixa de estoque na mesma transação. Se não houver saldo, a operação inteira sofre *rollback*.
- **Histórico de Preços:** O preço praticado na peça é salvo na OS (evitando que aumentos de preço futuros mudem ordens antigas).

## 🛠️ Tecnologias Utilizadas
- **Linguagem:** Java 17
- **Framework:** Spring Boot 3
- **Persistência:** Spring Data JPA / Hibernate
- **Banco de Dados:** MySQL
- **Boilerplate & Validações:** Lombok, Bean Validation
- **Padrões de Projeto:** DTOs (via Java `record`), Controller Advice (Tratamento Global de Exceções), Controller-Service-Repository.

## ⚙️ Como Executar o Projeto

**1. Clone o repositório**
```bash
git clone https://github.com/seu-usuario/service-hub-api.git
cd service-hub-api
```

**2. Configure o Banco de Dados**
Certifique-se de ter o MySQL rodando na porta `3306` e configure seu usuário/senha no arquivo `src/main/resources/application.properties`.

**3. Rode a aplicação**
```bash
./mvnw spring-boot:run
```
*(A aplicação estará disponível em `http://localhost:8080` e criará o banco automaticamente caso configurado)*

## 📚 Endpoints Principais (Exemplos)

| Método | Rota | Descrição |
|---|---|---|
| `POST` | `/clientes` | Cadastra um novo cliente |
| `POST` | `/veiculos` | Associa um veículo a um cliente |
| `POST` | `/pecas` | Cadastra peça e atualiza estoque |
| `POST` | `/ordens-servico` | Abre uma nova Ordem de Serviço |
| `PUT`  | `/ordens-servico/{id}/itens` | Adiciona peças à OS em andamento |
| `PATCH`| `/ordens-servico/{id}/fechar` | Conclui OS, calcula total e baixa estoque |

## 🏗️ Decisões Técnicas (ADR Resumido)
* **Uso do `BigDecimal`:** Garantia de precisão financeira para valores de peças e total da OS.
* **DTOs via `Records`:** Escolhidos por serem imutáveis e menos verbosos para transitar dados da API, evitando exposição direta das entidades de banco (`@Entity`).
* **Relacionamentos `LAZY`:** Evita *N+1 Queries*, garantindo que as consultas ao banco não tragam o banco de dados inteiro na memória desnecessariamente.

## 👨‍💻 Autor
Desenvolvido por **João Vitor Costa**. Conecte-se comigo no [LinkedIn](https://www.linkedin.com/in/jo%C3%A3o-vitor-costa-71b9a6254/)!
