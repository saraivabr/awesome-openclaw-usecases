# Resumo Matinal Personalizado

Você acorda e passa os primeiros 30 minutos do dia tentando se situar — navegando por notícias, conferindo sua agenda, revisando sua lista de tarefas e tentando descobrir o que realmente importa hoje. E se tudo isso já estivesse pronto e à sua espera em uma mensagem de texto?

Este fluxo de trabalho faz com que o OpenClaw envie um resumo matinal totalmente personalizado todos os dias em um horário agendado, cobrindo notícias, tarefas, ideias e recomendações proativas.

## O Que Ele Faz

- Envia um relatório matinal estruturado para o Telegram, Discord ou iMessage no mesmo horário todos os dias (ex.: 8h)
- Pesquisa notícias da madrugada relevantes para seus interesses navegando na web
- Revisa sua lista de tarefas e destaca o que precisa ser feito no dia
- Gera resultados criativos (roteiros completos, rascunhos de e-mails, propostas de negócios — não apenas ideias superficiais) enquanto você dorme
- Recomenda tarefas que a IA pode concluir de forma autônoma para ajudar você naquele dia

## O Problema

Você gasta suas horas matinais mais produtivas apenas tentando se organizar. Enquanto isso, seu agente de IA fica ocioso a noite inteira. O resumo matinal transforma essas horas ociosas da madrugada em tempo produtivo de preparação — você já acorda com o trabalho adiantado.

## Skills Necessárias

- Integração com Telegram, Discord ou iMessage
- Integração com Todoist / Apple Reminders / Asana (ou o aplicativo que você utilizar para tarefas)
- [x-research-v2](https://clawhub.ai) para pesquisa de tendências em redes sociais (opcional)

## Como Configurar

1. Conecte o OpenClaw à sua plataforma de mensagens e ao seu gerenciador de tarefas.

2. Envie o prompt para o OpenClaw:
```text
Quero configurar um resumo matinal diário. Todas as manhãs às 8h,
envie-me um relatório pelo Telegram.

Quero que este relatório inclua:
1. Notícias relevantes para meus interesses (IA, startups, tecnologia)
2. Ideias de conteúdo que posso criar hoje
3. Tarefas que preciso concluir hoje (busque da minha lista de tarefas)
4. Recomendações de tarefas que você pode realizar por mim hoje

Para as ideias de conteúdo, escreva rascunhos completos de roteiros/estruturas — não apenas títulos.
```

3. O OpenClaw agendará isso automaticamente. Verifique se está funcionando checando suas mensagens na manhã seguinte.

4. Personalize ao longo do tempo — basta mandar uma mensagem para o seu bot:
```text
Adicione a previsão do tempo ao meu resumo matinal.
Pare de incluir notícias gerais, foque apenas em IA.
Inclua uma frase motivacional a cada manhã.
```

5. Se você não souber exatamente o que incluir, não tem problema — basta dizer:
```text
Quero que este relatório inclua coisas relevantes para mim.
Pense no que seria mais útil incluir neste relatório.
```

## Principais Insights

- A seção de tarefas recomendadas pela IA é a parte mais poderosa — ela faz o agente pensar proativamente em formas de ajudar você, em vez de apenas esperar instruções.
- Você pode personalizar o resumo apenas mandando uma mensagem de texto. Diga "Adicione as cotações de ações ao meu resumo matinal" e ele atualiza.
- Rascunhos completos (e não apenas ideias) são o segredo para economizar tempo. Acorde com roteiros prontos, não com sugestões rasas.
- Não importa a sua área de atuação — um resumo matinal com tarefas, notícias e sugestões proativas é universalmente útil.

## Baseado Em

Inspirado no [vídeo de Alex Finn sobre casos de uso transformadores do OpenClaw](https://www.youtube.com/watch?v=41_TNGDDnfQ).
