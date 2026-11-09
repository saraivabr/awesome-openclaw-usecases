# Rastreador de Hábitos e Treinador de Consistência

Você já deve ter testado todos os aplicativos de rastreamento de hábitos existentes. Todos funcionam durante uma semana e depois você para de abrir. O problema não é o aplicativo — é que registrar hábitos é algo passivo. E se o seu agente entrasse em contato ativamente com você, perguntasse como foi o seu dia e adaptasse a abordagem com base em estar mantendo a sequência ou perdendo o ritmo?

Este caso de uso transforma o OpenClaw em um parceiro proativo de consistência (accountability) que faz check-ins diários com você via Telegram ou SMS.

## Dores e Desafios

Aplicativos de hábitos dependem de você se lembrar de abri-los. Notificações push são fáceis de ignorar. O que realmente funciona para mudança comportamental é **responsabilização ativa (active accountability)** — alguém (ou algo) que pergunta diretamente a você, celebra suas vitórias e dá aquele empurrãozinho quando você vacila. Este agente faz exatamente isso, sem o constrangimento de incomodar um amigo.

## O Que Ele Faz

- **Check-ins diários** via Telegram ou SMS nos horários que você escolher (por exemplo, 07:30 para a rotina matinal, 21:00 para a revisão do fim do dia)
- **Rastreia os hábitos** que você definir — exercícios, leitura, meditação, consumo de água, programação, o que for importante para você
- **Controle de sequências (streaks)** — sabe sua sequência atual para cada hábito e menciona isso nas mensagens
- **Incentivos adaptativos** — ajusta o tom com base no seu desempenho (encorajador quando você está consistente, gentilmente persistente quando pula dias)
- **Relatórios semanais** — resume sua semana com taxas de conclusão, maiores sequências e padrões (por exemplo: "Você costuma pular os treinos nas quartas-feiras")

## Skills Necessárias

- Integração com Telegram ou SMS (Twilio para SMS, ou Telegram Bot API)
- Agendamento / cron para check-ins programados
- Acesso ao sistema de arquivos ou banco de dados para armazenar dados de hábitos
- Opcional: Integração com Google Sheets para um painel visual de hábitos

## Como Configurar

1. Defina seus hábitos e o cronograma de check-ins:
```text
Quero que você seja meu treinador de consistência (accountability coach). Acompanhe estes hábitos diários para mim:

1. Treino matinal (check-in às 07:30)
2. Ler por 30 minutos (check-in às 20:00)
3. Sem redes sociais antes do meio-dia (check-in às 12:30)
4. Beber 8 copos de água (check-in às 18:00)

Envie-me uma mensagem no Telegram no horário de cada check-in perguntando se concluí o hábito. Mantenha o controle das minhas sequências em um arquivo local.
```

2. Configure o rastreamento e o tom:
```text
Quando eu confirmar um hábito, responda com uma mensagem curta e encorajadora, mencionando minha sequência atual. Exemplo: "Dia 12 de treinos matinais. Excelente."

Quando eu falhar em um hábito, não tente me fazer sentir culpado. Apenas registre e me lembre por que comecei. Se eu perder 3 dias seguidos, envie uma mensagem motivacional mais detalhada e pergunte se quero ajustar a meta.

Se eu não responder a um check-in em até 2 horas, envie um único follow-up de lembrete. Não envie mais mensagens depois disso.
```

3. Adicione relatórios semanais:
```text
Todo domingo às 10:00, me envie um resumo semanal:
- Taxa de conclusão para cada hábito
- Sequências atuais (streaks)
- Melhor dia e pior dia
- Um padrão que você notou (ex.: "Você sempre pula a leitura nas sextas-feiras")
- Uma sugestão para a próxima semana

Armazene todos os dados em ~/habits/log.json para que eu possa revisar o histórico a qualquer momento.
```

4. Opcional — painel visual via Google Sheets:
```text
No final de cada dia, atualize uma planilha do Google Sheets com os dados de hábitos de hoje.
Colunas: Data, Treino, Leitura, Sem Redes Sociais, Água, Observações.
Marque os hábitos concluídos com ✓ e os não realizados com ✗.
```

## Principais Insights

- O **tom adaptativo** é o que torna isso muito diferente de um simples cron job. Um lembrete estático é ignorado. Uma mensagem dizendo "Dia 15, não perca o ritmo agora" realmente motiva.
- Mantenha o número de hábitos rastreados reduzido (3 a 5). Monitorar muitos hábitos leva ao esgotamento de check-ins e você começará a ignorar as mensagens.
- A análise semanal de padrões é surpreendentemente útil — você descobrirá insights como "Nunca faço exercícios em dias com reuniões muito cedo" e poderá se planejar em função disso.
- Combina muito bem com o [Rastreador de Saúde e Sintomas](health-symptom-tracker.md) se você quiser correlacionar hábitos com a forma como se sente.

## Links Relacionados

- [Telegram Bot API](https://core.telegram.org/bots/api)
- [Twilio SMS API](https://www.twilio.com/docs/sms)
- [Google Sheets API](https://developers.google.com/sheets/api)
