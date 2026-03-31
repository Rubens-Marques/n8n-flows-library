# n8n-flows-library

> Biblioteca de flows N8N prontos para automações reais de negócio. Importe, adapte e use.

![N8N](https://img.shields.io/badge/N8N-EA4B71?style=flat&logo=n8n&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=flat)
![Flows](https://img.shields.io/badge/Flows-8-blue?style=flat)

## Como usar

1. Acesse seu N8N → Menu → Settings → Import from File
2. Selecione o `flow.json` desejado
3. Configure as credenciais nos nós marcados com ⚠️
4. Ative o flow

## Flows disponíveis

### Vendas
| Flow | O que faz |
|------|-----------|
| [whatsapp-lead-crm](flows/vendas/whatsapp-lead-crm/) | Captura mensagem WhatsApp → cria lead no CRM automaticamente |
| [form-to-sheet](flows/vendas/form-to-sheet/) | Formulário web → Google Sheets → notificação email |

### Dados
| Flow | O que faz |
|------|-----------|
| [api-to-database](flows/dados/api-to-database/) | Polling de API externa → salva no banco de dados agendado |
| [csv-processor](flows/dados/csv-processor/) | Upload CSV → validação → transformação → destino configurável |

### Notificações
| Flow | O que faz |
|------|-----------|
| [error-alerter](flows/notificacoes/error-alerter/) | Monitora endpoint → alerta WhatsApp/email em caso de erro |
| [daily-report](flows/notificacoes/daily-report/) | Gera relatório diário automático e envia por email |

### Integrações
| Flow | O que faz |
|------|-----------|
| [webhook-router](flows/integracoes/webhook-router/) | Recebe webhook genérico → roteamento condicional por tipo |
| [sync-databases](flows/integracoes/sync-databases/) | Sincronização periódica entre duas fontes de dados |

## Estrutura do repositório

```
flows/
├── vendas/
│   ├── whatsapp-lead-crm/   # Webhook + Set + HTTP Request + Respond
│   └── form-to-sheet/       # Webhook + Set + Google Sheets + Email
├── dados/
│   ├── api-to-database/     # Schedule + HTTP Request + Code + MySQL
│   └── csv-processor/       # Webhook + Spreadsheet File + Code + HTTP Request
├── notificacoes/
│   ├── error-alerter/       # Schedule + HTTP Request + IF + Email + WhatsApp
│   └── daily-report/        # Schedule + MySQL + Code + Email
└── integracoes/
    ├── webhook-router/      # Webhook + Switch + HTTP Request (x3)
    └── sync-databases/      # Schedule + MySQL + Code + HTTP Request + MySQL
```

## Contribuindo

Tem um flow útil? Abra um PR com `flow.json` + `README.md` seguindo a estrutura existente.

## Licença

MIT © Rubens Marques
