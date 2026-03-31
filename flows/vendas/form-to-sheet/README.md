# Formulário para Google Sheets

## O que faz

Recebe submissões de formulários web via webhook, salva os dados em uma planilha Google Sheets e envia uma notificação por email para a equipe de vendas.

## Pré-requisitos

- N8N 1.0+
- Credenciais necessárias:
  - **Google Sheets OAuth2** — autorização OAuth2 com acesso a Sheets
  - **SMTP Email** — servidor de email para envio de notificações

## Como configurar

1. No N8N: Menu → Settings → Import from File → selecione `flow.json`
2. Configure o nó `Salvar no Google Sheets` com sua credencial Google Sheets OAuth2 ⚠️
3. Substitua `SEU_SPREADSHEET_ID` pelo ID da sua planilha (encontrado na URL do Google Sheets)
4. Certifique-se de que a aba `Leads` existe na planilha com os cabeçalhos: Nome, Email, Telefone, Mensagem, Data Envio
5. Configure o nó `Enviar Email Notificação` com suas credenciais SMTP ⚠️
6. Altere os emails `fromEmail` e `toEmail` conforme necessário
7. No seu formulário web, configure o `action` para a URL gerada pelo nó `Webhook Formulário`
8. Ative o flow

## Nodes utilizados

- `n8n-nodes-base.webhook` — recebe a submissão do formulário
- `n8n-nodes-base.set` — formata e normaliza os campos
- `n8n-nodes-base.googleSheets` — append de nova linha na planilha
- `n8n-nodes-base.emailSend` — notificação por email para equipe

## Exemplo de payload

**Input (body do formulário):**
```json
{
  "nome": "Maria Oliveira",
  "email": "maria@empresa.com",
  "telefone": "11987654321",
  "mensagem": "Gostaria de agendar uma demonstração"
}
```

**Linha criada no Google Sheets:**
| Nome | Email | Telefone | Mensagem | Data Envio |
|------|-------|----------|----------|------------|
| Maria Oliveira | maria@empresa.com | 11987654321 | Gostaria de agendar... | 31/03/2026, 14:30 |
