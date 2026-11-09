# Notificações por Ligação Telefônica

Seu agente já monitora coisas para você — cotações, e-mails, dispositivos inteligentes, agenda —, mas notificações comuns são fáceis de ignorar. Notificações push se acumulam. Mensagens no chat ficam perdidas. Para os assuntos que realmente exigem atenção imediata, você precisa de algo impossível de descartar com um simples deslize de tela.

Este caso de uso dá ao seu agente uma chamada telefônica como canal de notificação. Quando uma situação for suficientemente urgente, o agente disca para o seu telefone de verdade, explica o que está acontecendo e você pode interagir com ele. É uma conversa em tempo real de duas vias, e não uma gravação eletrônica (robocall).

## O Que Ele Faz

- O agente identifica que algo requer sua atenção urgente (alerta de preço, e-mail prioritário, lembrete crítico de compromisso)
- O agente liga para o seu telefone via [clawr.ing](https://clawr.ing), um serviço gerenciado de telefonia — sem necessidade de configurar Twilio ou gerenciar chaves de API complexas
- Você atende, ouve o alerta e pode fazer perguntas de acompanhamento na hora
- A chamada termina de forma natural ao fim da conversa

A ideia central: o agente liga para VOCÊ, em vez de você ligar para ele. Isso se integra com checagens de rotina (heartbeat), agendamentos via cron ou qualquer outro gatilho — o agente avalia se o fato é digno de uma ligação telefônica e age proativamente.

## Por Que Isso Funciona

Os agentes OpenClaw já possuem iniciativa própria — verificações de rotina (heartbeat), tarefas no cron e gatilhos de eventos. No entanto, seus canais de saída costumam ficar restritos a chats que você pode não estar lendo no momento. Uma ligação telefônica é o canal que captura sua atenção de maneira certeira, especialmente longe do computador.

O clawr.ing gerencia toda a infraestrutura de telefonia. Basta colar um único prompt de configuração para conceder ao agente o poder de fazer chamadas. Sem conta na Twilio, sem compra de números e sem configuração de webhooks. O serviço oferece cobertura em mais de 100 países com chamadas telefônicas reais (PSTN), sem gambiarras de VoIP.

## Skills Necessárias

- [clawr.ing](https://clawhub.ai/marcospgp/clawring) — instale colando o prompt de configuração disponível no [painel da clawr.ing](https://clawr.ing) no chat do OpenClaw. Não necessita de instalação via terminal.

Apenas isso. Sem dependências adicionais. O prompt de configuração já inclui a chave de API e links para a documentação — o próprio agente lê e cuida do restante.

## Exemplo: Chamada de Briefing Matinal

Configure uma rotina no cron que ligue para você todas as manhãs com um briefing personalizado:

```text
Todos os dias úteis, às 07:30, ligue para mim com um briefing matinal. Inclua:
- Previsão do tempo para a minha cidade
- Minha agenda para hoje
- Quaisquer e-mails urgentes que tenham chegado durante a noite
- As 3 principais manchetes de notícias relevantes para os meus interesses

Seja conciso — tente manter abaixo de 2 minutos. Se eu fizer perguntas, responda.
Se eu não atender, não tente ligar novamente.
```

## Exemplo: Alerta de Preço

Oriente o agente sobre o que monitorar e quando ligar:

```text
Monitore o preço das ações da NVDA. Se caírem mais de 5% em um único dia,
ligue para mim imediatamente e explique o que aconteceu. Inclua notícias
relevantes que ajudem a justificar a queda.
```

## Exemplo: Filtro de E-mails Urgentes

```text
Durante o horário comercial, verifique minha caixa de entrada a cada 15 minutos.
Se houver um e-mail do meu chefe ou marcado como urgente,
ligue para mim com um resumo. Para todo o resto, apenas envie uma mensagem no chat.
```

## Principais Aprendizados

- **Não abuse do recurso.** O grande valor da ligação é sinalizar que "isso realmente importa". Se o agente ligar dez vezes por dia, você passará a ignorá-lo. Estabeleça critérios rígidos para o que merece uma ligação em comparação a uma mensagem de chat.
- **Combine com rotinas de heartbeat ou cron.** O clawr.ing é o meio de entrega — você ainda precisa do gatilho. Checagens periódicas (heartbeat a cada 30 min) servem para monitoramento ativo. Tarefas cron cuidam de briefings com horário marcado.
- **Conversação bidirecional.** Ao contrário de uma notificação estática, você pode tirar dúvidas durante a ligação: "Qual e-mail específico?" ou "Qual o valor agora?" — o agente responde na hora.
- **Modelo rápido para chamadas.** Conversas por telefone exigem respostas instantâneas. Se o seu OpenClaw suportar roteamento de modelo por skill, configure o clawr.ing com um modelo de baixa latência (classe Haiku).
- **Privacidade.** O clawr.ing não grava chamadas nem armazena transcrições. O áudio é criptografado em trânsito e descartado após o processamento.

## Links Relacionados

- [clawr.ing](https://clawr.ing)
- [clawr.ing no ClawHub](https://clawhub.ai/marcospgp/clawring)
- [OpenClaw](https://github.com/openclaw/openclaw)
