# Dapper Stored Procedures Example

Este projeto é um exemplo de uma API RESTful construída com .NET Core que demonstra o uso do Dapper para acessar um banco de dados SQL Server através de stored procedures.

## 📋 Visão Geral

O projeto implementa um gerenciador de tarefas simples com operações CRUD (Create, Read, Update, Delete) utilizando:
- .NET 7.0
- Dapper como micro ORM
- SQL Server como banco de dados
- Stored Procedures para todas as operações de banco de dados
- Swagger/OpenAPI para documentação da API

## 🏗️ Estrutura do Projeto

O projeto está organizado nas seguintes camadas:

- **WebApiDapper**: Camada de apresentação (API REST)
  - Controllers: Endpoints da API
  - Repository: Implementação do repositório usando Dapper
  - Data: Modelos de dados

- **Domain**: Camada de domínio (não implementada neste exemplo)
- **Infra**: Camada de infraestrutura (não implementada neste exemplo)
- **Shared**: Recursos compartilhados (não implementado neste exemplo)
- **TestDapper**: Projeto de testes (não implementado neste exemplo)

## 🚀 Como Executar

### Pré-requisitos

- .NET 7.0 SDK ou superior
- SQL Server (local ou container Docker)
- Um cliente HTTP como Postman, Insomnia ou curl

### Configuração

1. **Banco de Dados**:
   - Crie um banco de dados no SQL Server
   - Execute o script SQL abaixo para criar a tabela e as stored procedures necessárias

```sql
-- Criação da tabela Tarefa
CREATE TABLE Tarefa (
    Id INT IDENTITY(1,1) PRIMARY KEY,
    Nome NVARCHAR(100) NOT NULL,
    Concluida BIT NOT NULL DEFAULT 0
);

-- Stored Procedure para listar todas as tarefas
CREATE PROCEDURE GetAllTarefa
AS
BEGIN
    SELECT Id, Nome, Concluida FROM Tarefa;
END;

-- Stored Procedure para buscar uma tarefa por ID
CREATE PROCEDURE GetTarefaById
    @Id INT
AS
BEGIN
    SELECT Id, Nome, Concluida FROM Tarefa WHERE Id = @Id;
END;

-- Stored Procedure para adicionar uma nova tarefa
CREATE PROCEDURE AddTarefa
    @Nome NVARCHAR(100),
    @Concluida BIT
AS
BEGIN
    INSERT INTO Tarefa (Nome, Concluida) VALUES (@Nome, @Concluida);
END;

-- Stored Procedure para atualizar uma tarefa existente
CREATE PROCEDURE UpdateTarefa
    @Id INT,
    @Nome NVARCHAR(100),
    @Concluida BIT
AS
BEGIN
    UPDATE Tarefa 
    SET Nome = @Nome, 
        Concluida = @Concluida 
    WHERE Id = @Id;
END;

-- Stored Procedure para excluir uma tarefa
CREATE PROCEDURE DeleteTarefa
    @Id INT
AS
BEGIN
    DELETE FROM Tarefa WHERE Id = @Id;
END;
```

2. **Configuração da Aplicação**:
   - Atualize a string de conexão no arquivo `appsettings.json` com as credenciais do seu SQL Server

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=seu_servidor;Database=seu_banco;User Id=seu_usuario;Password=sua_senha;TrustServerCertificate=true;"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### Executando a Aplicação

1. Navegue até a pasta do projeto:
   ```bash
   cd Dapper/WebApiDapper
   ```

2. Execute a aplicação:
   ```bash
   dotnet run
   ```

3. Acesse a documentação da API em: `https://localhost:5001/swagger`

## 📚 Endpoints da API

- `GET /api/tarefa` - Lista todas as tarefas
- `GET /api/tarefa/{id}` - Obtém uma tarefa pelo ID
- `POST /api/tarefa` - Cria uma nova tarefa
- `PUT /api/tarefa/{id}` - Atualiza uma tarefa existente
- `DELETE /api/tarefa/{id}` - Remove uma tarefa

## 🛠️ Tecnologias Utilizadas

- [.NET 7.0](https://dotnet.microsoft.com/download/dotnet/7.0)
- [Dapper](https://github.com/StackExchange/Dapper)
- [SQL Server](https://www.microsoft.com/pt-br/sql-server/)
- [Swagger/OpenAPI](https://swagger.io/)

## 📄 Licença

Este projeto está licenciado sob a licença MIT - veja o arquivo [LICENSE](LICENSE) para detalhes.

## 👥 Contribuição

Contribuições são bem-vindas! Sinta-se à vontade para abrir uma issue ou enviar um pull request.
