# Monitor de Erros com Alertas

## O que faz

Verifica a cada 5 minutos se um endpoint está respondendo com status 200. Quando detecta falha, dispara simultaneamente um alerta por email e uma mensagem no WhatsApp para a equipe de DevOps.

## Pré-requisitos

- N8N 1.0+
- Credenciais necessárias:
  - **SMTP Email** — servidor de email para alertas
  - **WhatsApp API Auth** — header de autenticação do provedor WhatsApp (Evolution API, ChatPro, etc.)

## Como configurar

1. No N8N: Menu → Settings → Import from File → selecione `flow.json`
2. No nó `Health Check Endpoint`, altere a URL `https://suaaplicacao.com/health` para o endpoint a monitorar ⚠️
3. Configure o nó `Alerta por Email` com suas credenciais SMTP ⚠️
4. Altere os emails `fromEmail` e `toEmail` no nó de email
5. Configure o nó `Alerta WhatsApp` com sua credencial de API ⚠️
6. Altere a URL e o número `5511999999999` no payload do WhatsApp
7. Para monitorar múltiplos endpoints, duplique o fluxo e ajuste a URL
8. Ative o flow

**Para reduzir a frequência:** altere o cron `*/5 * * * *` para `*/15 * * * *` (15 min) ou `0 * * * *` (1 hora).

## Nodes utilizados

- `n8n-nodes-base.scheduleTrigger` — polling a cada 5 minutos via cron
- `n8n-nodes-base.httpRequest` — requisição GET para o endpoint monitorado
- `n8n-nodes-base.if` — condição: status diferente de 200 dispara os alertas
- `n8n-nodes-base.emailSend` — alerta email com detalhes da falha
- `n8n-nodes-base.httpRequest` — alerta WhatsApp via API do provedor

## Exemplo de payload

**Alerta WhatsApp enviado:**
```
ALERTA: Serviço fora do ar!
Status: 503
Horário: 31/03/2026, 14:35:00
```
