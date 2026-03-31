# Sincronização entre Bancos de Dados

## O que faz

A cada hora, busca no MySQL de origem os registros criados ou atualizados na última hora (ainda não sincronizados), transforma o schema para o formato do destino, envia via API bulk-upsert e marca os registros como sincronizados no banco de origem.

## Pré-requisitos

- N8N 1.0+
- Credenciais necessárias:
  - **MySQL Origem** — conexão com o banco de dados fonte
  - **API Destino** — header de autenticação da API de destino (CRM, CDP, data warehouse, etc.)

## Como configurar

1. No N8N: Menu → Settings → Import from File → selecione `flow.json`
2. Configure ambos os nós MySQL (`Buscar Registros Novos` e `Marcar como Sincronizado`) com a credencial do banco de origem ⚠️
3. Adapte a query SQL no nó `Buscar Registros Novos` para sua tabela real
4. Adicione a coluna de controle no banco de origem:
   ```sql
   ALTER TABLE clientes ADD COLUMN sincronizado TINYINT(1) DEFAULT 0;
   ALTER TABLE clientes ADD COLUMN sincronizado_em DATETIME NULL;
   ```
5. Configure o nó `Enviar para API Destino` com sua credencial de API ⚠️
6. Altere a URL `https://api.destino.com/v2/contacts/bulk-upsert` para o endpoint real
7. Ajuste a transformação no nó `Transformar para Destino` para o schema da API de destino
8. Ative o flow

## Nodes utilizados

- `n8n-nodes-base.scheduleTrigger` — disparo horário via cron (`0 * * * *`)
- `n8n-nodes-base.mySql` — query com filtro de registros não sincronizados
- `n8n-nodes-base.code` — transformação de schema + early return se não há dados
- `n8n-nodes-base.httpRequest` — envio em lote para API de destino
- `n8n-nodes-base.mySql` — UPDATE para marcar registros como sincronizados

## Exemplo de payload

**Registro MySQL de origem:**
```json
{
  "id": 101,
  "nome": "Carlos Mendes",
  "email": "carlos@empresa.com",
  "status": "ativo",
  "categoria": "premium",
  "valor": 1500.00
}
```

**Payload enviado para API de destino:**
```json
{
  "contacts": [
    {
      "external_id": "mysql_101",
      "full_name": "Carlos Mendes",
      "email": "carlos@empresa.com",
      "status": "active",
      "segment": "premium",
      "lifetime_value": 1500.00,
      "source": "mysql_sync"
    }
  ]
}
```
