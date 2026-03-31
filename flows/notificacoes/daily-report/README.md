# Relatório Diário Automático

## O que faz

Toda manhã (dias úteis, 8h), consulta o banco de dados com as métricas do dia anterior — pedidos, receita, taxa de conclusão — formata um relatório HTML e envia por email para a gestão.

## Pré-requisitos

- N8N 1.0+
- Credenciais necessárias:
  - **MySQL Database** — conexão com o banco de dados de negócio
  - **SMTP Email** — servidor de email para envio do relatório

## Como configurar

1. No N8N: Menu → Settings → Import from File → selecione `flow.json`
2. Configure o nó `Buscar Métricas do Dia` com suas credenciais MySQL ⚠️
3. Adapte a query SQL no mesmo nó para refletir o schema real da sua tabela `pedidos`
4. Configure o nó `Enviar Relatório por Email` com suas credenciais SMTP ⚠️
5. Altere `fromEmail` e `toEmail` conforme necessário (suporta múltiplos destinatários separados por vírgula)
6. Ajuste o cron `0 8 * * 1-5` se precisar de horário ou dias diferentes
7. Ative o flow

**Para adicionar métricas:** edite o nó `Formatar Relatório HTML` (código JavaScript) e adicione novas linhas à tabela HTML e à query SQL.

## Nodes utilizados

- `n8n-nodes-base.scheduleTrigger` — disparo agendado: dias úteis às 8h
- `n8n-nodes-base.mySql` — query agregada com métricas do dia anterior
- `n8n-nodes-base.code` — formatação do HTML e cálculo de indicadores derivados
- `n8n-nodes-base.emailSend` — envio do relatório formatado

## Exemplo de payload

**Email gerado:**
```
Assunto: Relatório Diário — 30/03/2026

Relatório Diário — 30/03/2026
┌─────────────────────┬──────────────┐
│ Métrica             │ Valor        │
├─────────────────────┼──────────────┤
│ Total de Pedidos    │ 143          │
│ Concluídos          │ 128          │
│ Cancelados          │ 7            │
│ Pendentes           │ 8            │
│ Taxa de Conclusão   │ 89.5%        │
│ Receita Total       │ R$ 32.450,00 │
│ Clientes Ativos     │ 91           │
└─────────────────────┴──────────────┘
```
