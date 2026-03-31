# API para Banco de Dados

## O que faz

Executa um polling agendado em uma API externa às 6h todo dia, transforma os registros retornados e os insere em uma tabela MySQL — ideal para ingestão diária de dados de terceiros.

## Pré-requisitos

- N8N 1.0+
- Credenciais necessárias:
  - **API Externa Auth** — header de autenticação da API de origem (ex: `Bearer token`)
  - **MySQL Database** — conexão com o banco de dados de destino

## Como configurar

1. No N8N: Menu → Settings → Import from File → selecione `flow.json`
2. Configure o nó `Buscar Dados da API` com sua credencial de acesso à API ⚠️
3. Altere a URL `https://api.exemplo.com/v1/registros` para o endpoint real
4. Configure o nó `Inserir no MySQL` com suas credenciais de banco ⚠️
5. Crie a tabela `registros_importados` no MySQL com o script abaixo
6. Ajuste o cron no nó `Agendamento Diário` se precisar de horário diferente
7. Ative o flow

**Script de criação da tabela:**
```sql
CREATE TABLE IF NOT EXISTS registros_importados (
  id BIGINT AUTO_INCREMENT PRIMARY KEY,
  id_externo VARCHAR(255) UNIQUE,
  nome VARCHAR(255),
  valor DECIMAL(15,2),
  status VARCHAR(50),
  categoria VARCHAR(100),
  data_registro DATETIME,
  importado_em DATETIME DEFAULT NOW()
);
```

## Nodes utilizados

- `n8n-nodes-base.scheduleTrigger` — disparo agendado via cron (`0 6 * * *`)
- `n8n-nodes-base.httpRequest` — consumo da API externa com paginação
- `n8n-nodes-base.code` — transformação e normalização dos campos
- `n8n-nodes-base.mySql` — inserção em lote no banco de dados

## Exemplo de payload

**Resposta esperada da API:**
```json
{
  "data": [
    { "id": "ext-001", "name": "Produto A", "value": "150.00", "status": "Active", "category": "Eletronicos" },
    { "id": "ext-002", "name": "Produto B", "value": "89.90",  "status": "Inactive", "category": "Roupas" }
  ]
}
```
