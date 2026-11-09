# Rastreador de Saúde e Sintomas

Identificar sensibilidades alimentares exige um registro consistente ao longo do tempo, o que costuma ser difícil de manter. Você precisa de lembretes para registrar e de análises para identificar padrões.

Este fluxo rastreia alimentação e sintomas automaticamente:

• Envie mensagens com o que comeu e seus sintomas em um tópico dedicado no Telegram e o OpenClaw registra tudo com carimbos de data/hora (timestamps)  
• 3 lembretes diários (manhã, almoço e noite) incentivam você a registrar as refeições  
• Com o tempo, analisa padrões para identificar possíveis gatilhos  

## Skills Necessárias

- Tarefas agendadas (Cron jobs) para os lembretes
- Tópico no Telegram para registro
- Armazenamento de arquivos (arquivo de log em Markdown)

## Como Configurar

1. Crie um tópico no Telegram chamado "health-tracker" (ou similar).
2. Crie um arquivo de registro: `~/clawd/memory/health-log.md`
3. Dê o prompt ao OpenClaw:
```text
Quando eu enviar uma mensagem no tópico "health-tracker":
1. Analise a mensagem procurando por itens alimentares e sintomas
2. Registre em ~/clawd/memory/health-log.md com data e hora
3. Confirme o que foi registrado

Configure 3 lembretes diários:
- 08:00: "🍳 Registre o seu café da manhã"
- 13:00: "🥗 Registre o seu almoço"
- 19:00: "🍽️ Registre o seu jantar e quaisquer sintomas"

Todo domingo, analise o registro da última semana e identifique padrões:
- Quais alimentos se correlacionam com os sintomas?
- Existem padrões em horários específicos do dia?
- Há algum gatilho claro?

Publique a análise no tópico health-tracker.
```

4. Opcional: Adicione um arquivo de memória para o OpenClaw acompanhar gatilhos já conhecidos e atualizá-lo à medida que novos padrões surgirem.
