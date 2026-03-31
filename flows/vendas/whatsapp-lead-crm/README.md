# WhatsApp Lead to CRM

## O que faz

Recebe mensagens via webhook do WhatsApp e cria automaticamente um lead no CRM, mapeando nome, telefone e primeira mensagem do contato.

## Pré-requisitos

- N8N 1.0+
- Credenciais necessárias:
  - **CRM API Key** — chave de acesso à API do seu CRM (header `Authorization`)
  - Endpoint do webhook configurado no seu provedor WhatsApp (Evolution API, ChatPro, etc.)

## Como configurar

1. No N8N: Menu → Settings → Import from File → selecione `flow.json`
2. Configure o nó `Criar Lead no CRM` com sua credencial `CRM API Key` ⚠️
3. Altere a URL `https://api.seucrm.com/v1/leads` para o endpoint real do seu CRM
4. No seu provedor WhatsApp, aponte o webhook para a URL gerada pelo nó `Webhook WhatsApp`
5. Ative o flow

## Nodes utilizados

- `n8n-nodes-base.webhook` — recebe a notificação do WhatsApp
- `n8n-nodes-base.set` — mapeia e normaliza os campos do lead
- `n8n-nodes-base.httpRequest` — cria o lead via API REST do CRM
- `n8n-nodes-base.respondToWebhook` — retorna confirmação ao provider

## Exemplo de payload

**Input (body do webhook WhatsApp):**
```json
{
  "from": "5511999999999@s.whatsapp.net",
  "pushName": "João Silva",
  "message": {
    "conversation": "Olá, tenho interesse no produto!"
  }
}
```

**Output (resposta ao webhook):**
```json
{
  "success": true,
  "lead_id": "abc123",
  "message": "Lead criado com sucesso"
}
```
