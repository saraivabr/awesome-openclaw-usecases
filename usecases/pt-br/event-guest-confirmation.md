# Confirmação de Convidados para Eventos

Você está organizando um evento — um jantar, um casamento, um offsite da empresa — e precisa confirmar a presença de uma lista de convidados. Ligar manualmente para mais de 20 pessoas é cansativo: você cai na caixa postal, esquece quem disse o quê e perde o controle de restrições alimentares ou acompanhantes. Enviar mensagens às vezes funciona, mas as pessoas ignoram. Uma ligação telefônica real tem uma taxa de resposta muito mais alta.

Este caso de uso faz com que o OpenClaw ligue para cada convidado da sua lista usando o plugin [SuperCall](https://clawhub.ai/xonder/supercall), confirme se eles comparecerão, colete observações e compile tudo em um resumo para você.

## O Que Ele Faz

- Percorre uma lista de convidados (nomes + números de telefone) e liga para cada um
- A IA se apresenta como seu coordenador de eventos com uma persona amigável
- Confirma a data, hora e local do evento com o convidado
- Pergunta se eles comparecerão e coleta quaisquer observações (necessidades dietéticas, acompanhantes, horário de chegada, etc.)
- Após a conclusão de todas as chamadas, compila um resumo: quem confirmou, quem recusou, quem não atendeu e quaisquer observações

## Por Que o SuperCall

Este caso de uso funciona especificamente com o plugin [SuperCall](https://clawhub.ai/xonder/supercall) — e não com o plugin nativo `voice_call`. A principal diferença: o SuperCall é um agente de voz totalmente independente. A persona de IA na chamada **só tem acesso ao contexto que você fornecer** (o nome da persona, o objetivo e a frase de abertura). Ela não pode acessar seu agente de gateway, seus arquivos, suas outras ferramentas ou qualquer outra coisa.

Isso é importante para a confirmação de convidados porque:

- **Segurança**: A pessoa do outro lado da chamada não pode manipular ou acessar seu agente por meio da conversa. Não há risco de prompt injection ou vazamento de dados.
- **Melhores conversas**: Como a IA é limitada a uma única tarefa focada (confirmar presença), ela permanece no assunto e conduz a chamada com muito mais naturalidade do que um agente de voz de uso geral faria.
- **Adequado para lotes (Batch)**: Você está fazendo várias chamadas para pessoas diferentes. Uma persona isolada em sandbox que se reinicia a cada chamada é exatamente o que você quer — sem sobreposição ou vazamento de contexto entre as conversas.

## Skills Necessárias

- [SuperCall](https://clawhub.ai/xonder/supercall) — instale via `openclaw plugins install @xonder/supercall`
- Uma conta Twilio com um número de telefone (para fazer chamadas de saída)
- Uma chave de API da OpenAI (para a IA de voz GPT-4o Realtime)
- ngrok (para tunelamento de webhooks — o plano gratuito funciona)

Consulte o [README do SuperCall](https://github.com/xonder/supercall) para obter instruções completas de configuração.

## Como Configurar

1. Instale e configure o SuperCall seguindo o [guia de configuração](https://github.com/xonder/supercall#configuration). Certifique-se de que os hooks estejam habilitados na sua configuração do OpenClaw.

2. Prepare sua lista de convidados. Você pode colá-la diretamente no chat ou mantê-la em um arquivo:

```text
Lista de Convidados — Churrasco de Verão, Sábado, 14 de junho, 16h, Rua das Acácias, 23

- Sarah Johnson: +15551234567
- Mike Chen: +15559876543
- Rachel Torres: +15555551234
- David Kim: +15558887777
```

3. Dê o prompt ao OpenClaw:

```text
Preciso que você confirme a presença para o meu evento. Aqui estão os detalhes:

Evento: Churrasco de Verão
Data: Sábado, 14 de junho às 16h
Local: Rua das Acácias, 23

Aqui está minha lista de convidados:
<cole sua lista de convidados aqui>

Para cada convidado, use o supercall para ligar para ele. Use a persona "Jamie, coordenador de eventos de [seu nome]". O objetivo de cada chamada é confirmar se comparecerão e anotar quaisquer restrições alimentares, acompanhantes ou outros comentários.

Após cada chamada, registre o resultado. Assim que todas as chamadas forem concluídas, me dê um resumo completo:
- Quem confirmou
- Quem recusou
- Quem não atendeu
- Quaisquer notas ou solicitações especiais de cada convidado
```

4. O OpenClaw ligará para cada convidado, um a um, usando o SuperCall, e depois compilará os resultados. Você pode acompanhar o progresso a qualquer momento pedindo uma atualização de status.

## Principais Insights

- **Comece com um teste pequeno**: Experimente primeiro com 2 a 3 convidados para ter certeza de que a persona e a frase de abertura soam naturais. Você pode ajustar o tom antes de ligar para a lista inteira.
- **Fique atento aos horários de ligação**: Não agende chamadas muito cedo ou muito tarde. Você pode instruir o OpenClaw a ligar apenas entre determinados horários.
- **Revise as transcrições**: O SuperCall registra as transcrições em `~/clawd/supercall-logs`. Dê uma olhada rápida nelas após o primeiro lote para ver como foram as conversas.
- **Tratamento de não atendimento**: Se alguém não atender, o OpenClaw pode registrar isso e você decide se deseja tentar novamente mais tarde ou fazer o acompanhamento por mensagem de texto.
- **Chamadas telefônicas reais têm custo**: Cada chamada consome minutos no Twilio. Defina limites apropriados em sua conta Twilio, especialmente para listas grandes de convidados.

## Links Relacionados

- [SuperCall on ClawHub](https://clawhub.ai/xonder/supercall)
- [SuperCall on GitHub](https://github.com/xonder/supercall)
- [Twilio Console](https://console.twilio.com)
- [OpenAI Realtime API](https://platform.openai.com/docs/guides/realtime)
- [ngrok](https://ngrok.com)
