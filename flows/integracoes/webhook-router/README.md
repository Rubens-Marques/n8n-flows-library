# Roteador de Webhooks

## O que faz

Recebe um webhook genérico com um campo `tipo` no payload e roteia para o sistema correto: pedidos vão para o sistema de e-commerce, pagamentos para o financeiro, cancelamentos de volta ao sistema de pedidos.

## Pré-requisitos

- N8N 1.0+
- Credenciais necessárias:
  - **Sistema Pedidos API** — header de autenticação da API de pedidos
  - **Sistema Financeiro API** — header de autenticação da API financeira

## Como configurar

1. No N8N: Menu → Settings → Import from File → selecione `flow.json`
2. Configure os nós `Processar Pedido` e `Processar Cancelamento` com a credencial do sistema de pedidos ⚠️
3. Configure o nó `Processar Pagamento` com a credencial do sistema financeiro ⚠️
4. Altere as URLs dos três nós `httpRequest` para os endpoints reais de cada sistema
5. Para adicionar novos tipos: adicione uma nova condição no nó `Roteador por Tipo` (Switch) e crie o nó de destino correspondente
6. Configure seu sistema de origem para enviar eventos para a URL gerada pelo nó `Receber Webhook`
7. Ative o flow

## Nodes utilizados

- `n8n-nodes-base.webhook` — ponto de entrada único para todos os eventos
- `n8n-nodes-base.switch` — roteamento condicional por valor do campo `tipo`
- `n8n-nodes-base.httpRequest` — envio para cada sistema de destino (3 instâncias)
- `n8n-nodes-base.respondToWebhook` — confirmação de recebimento

## Exemplo de payload

**Evento de pedido:**
```json
{
  "tipo": "pedido",
  "id": "PED-001",
  "cliente_id": 42,
  "valor_total": 299.90,
  "itens": [{ "produto": "Camisa M", "qty": 2 }]
}
```

**Evento de pagamento:**
```json
{
  "tipo": "pagamento",
  "pedido_id": "PED-001",
  "metodo": "pix",
  "valor": 299.90,
  "status": "aprovado"
}
```

**Resposta do webhook:**
```json
{
  "received": true,
  "tipo": "pedido",
  "processado_em": "2026-03-31T14:30:00.000Z"
}
```
