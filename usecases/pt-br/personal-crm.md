# CRM Pessoal com Descoberta Automática de Contatos

Acompanhar com quem você se reuniu, quando isso aconteceu e o que foi discutido é impraticável de fazer manualmente. Retornos importantes acabam caindo no esquecimento e perde-se o contexto antes de reuniões relevantes.

Este fluxo de trabalho cria e mantém um CRM pessoal de maneira totalmente automática:

• Rotina diária via cron varre e-mails e agenda em busca de novos contatos e interações  
• Salva os contatos em um banco de dados estruturado com o contexto do relacionamento  
• Consultas em linguagem natural: "O que eu sei sobre [pessoa]?", "Quem está precisando de acompanhamento?", "Quando foi a última vez que conversei com [pessoa]?"  
• Briefing matinal de preparação para reuniões: antes dos compromissos do dia, pesquisa participantes externos no CRM e no histórico de e-mails, entregando um resumo contextualizado  

## Skills Necessárias

- `gog` CLI (para Gmail e Google Agenda)
- Banco de dados de CRM personalizado (SQLite ou similar) ou a skill [crm-query](https://clawhub.ai), caso disponível
- Tópico no Telegram configurado para consultas de CRM

## Como Configurar

1. Crie uma tabela para o banco de dados do CRM:
```sql
CREATE TABLE contacts (
  id INTEGER PRIMARY KEY,
  name TEXT,
  email TEXT,
  first_seen TEXT,
  last_contact TEXT,
  interaction_count INTEGER,
  notes TEXT
);
```

2. Configure um tópico no Telegram com o nome `personal-crm` para realizar consultas.

3. Envie este prompt ao OpenClaw:
```text
Execute uma rotina cron diária às 06:00 para:
1. Varrer meu Gmail e Google Agenda nas últimas 24 horas
2. Extrair novos contatos e atualizar os existentes
3. Registrar interações (reuniões, e-mails) com data, hora e contexto

Além disso, todas as manhãs às 07:00:
1. Checar minha agenda para as reuniões do dia
2. Para cada participante externo, consultar o histórico no CRM e nos e-mails
3. Enviar um briefing no Telegram com: quem são, última vez que conversamos, assuntos tratados e pendências de acompanhamento

Quando eu perguntar sobre um contato no tópico personal-crm, faça uma busca no banco de dados e traga todas as informações disponíveis.
```
