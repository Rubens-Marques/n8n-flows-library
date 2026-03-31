# Processador de CSV

## O que faz

Recebe upload de arquivo CSV via webhook, valida os dados (campos obrigatórios, formato de email), transforma os registros e os envia para um endpoint de destino — útil para importações em massa.

## Pré-requisitos

- N8N 1.0+
- Credenciais necessárias:
  - **API Destino Auth** — header de autenticação da API que receberá os dados

## Como configurar

1. No N8N: Menu → Settings → Import from File → selecione `flow.json`
2. Configure o nó `Enviar para Destino` com sua credencial de API ⚠️
3. Altere a URL `https://api.destino.com/v1/registros/bulk` para o endpoint real
4. Ajuste os campos esperados no nó `Validar e Transformar` conforme seu CSV
5. Ative o flow e use a URL gerada pelo nó `Webhook Upload CSV` para enviar arquivos

**Exemplo de chamada para enviar o arquivo:**
```bash
curl -X POST https://seu-n8n.com/webhook/upload-csv \
  -F "data=@seus-dados.csv"
```

## Nodes utilizados

- `n8n-nodes-base.webhook` — recebe o upload multipart do arquivo CSV
- `n8n-nodes-base.spreadsheetFile` — lê e parseia o CSV (suporte a separador `;` ou `,`)
- `n8n-nodes-base.code` — validação de campos obrigatórios, formato email, normalização
- `n8n-nodes-base.set` — preparação final do payload
- `n8n-nodes-base.httpRequest` — envio dos registros válidos para o destino

## Exemplo de payload

**Formato esperado do CSV:**
```csv
nome;email;telefone;valor
João Silva;joao@email.com;11999999999;150.00
Maria Souza;maria@email.com;21988887777;89.90
```

**Registro rejeitado (retornado no console):**
```
Erros: 1 — campo email inválido
```
