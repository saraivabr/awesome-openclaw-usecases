# Assistente Familiar e Calendário Doméstico

Famílias modernas equilibram cinco ou mais calendários — profissional, pessoal, familiar compartilhado, escola dos filhos, atividades extracurriculares — em diferentes plataformas e formatos. Eventos importantes acabam passando despercebidos porque não existe uma visão centralizada. Enquanto isso, a coordenação doméstica (listas de compras, controle de despensa, agendamento de consultas) acontece por meio de mensagens de texto dispersas que se perdem no histórico.

Este caso de uso transforma o OpenClaw em um coordenador doméstico sempre ativo: consolidando calendários em um boletim matinal, monitorando mensagens em busca de itens acionáveis e gerenciando a logística da casa por meio de uma interface de chat compartilhada.

## Dores e Desafios

- **Fragmentação de calendários**: Calendários corporativos têm restrições de segurança que impedem o compartilhamento. Calendários escolares chegam como PDFs ou sites feitos à mão. Agendas de colônia de férias ficam em e-mails. Checar manualmente cada um toda manhã é insustentável — e "copiar eventos entre calendários funciona bem até que eu esqueça e um passe despercebido."
- **Sobrecarga na coordenação doméstica**: "Quanto leite ainda temos?" exige checar fisicamente a geladeira, depois a despensa ou depósito, e então responder por mensagem. Multiplique isso por uma semana inteira de idas ao supermercado.
- **Consultas e compromissos perdidos**: Confirmações de agendamento chegam via SMS e ficam lá esquecidas — sem evento na agenda, sem margem de deslocamento no trânsito, sem lembrete.

## O Que Ele Faz

- **Boletim matinal**: Consolida todos os calendários da família em um resumo diário único entregue pelo seu canal preferido
- **Monitoramento de mensagens em segundo plano (ambiente)**: Monitora conversas de iMessage/SMS e cria eventos no calendário automaticamente ao detectar agendamentos (confirmações de dentista, planos de reuniões, etc.)
- **Margens de tempo para deslocamento**: Adiciona blocos de tempo para deslocamento antes e depois dos compromissos detectados
- **Inventário doméstico**: Mantém um controle contínuo dos itens na despensa/geladeira que qualquer um dos parceiros pode consultar de qualquer lugar
- **Coordenação de compras**: Desduplica ingredientes entre receitas, monitora o que está acabando e gera listas de compras
- **Entrada baseada em fotos**: Tire uma foto do calendário escolar ou do conteúdo do freezer e o agente processa em dados estruturados

## Skills Necessárias

- Acesso à API de Calendário (Google Calendar, Apple Calendar via `ical`)
- Skill `imessage` para monitoramento de mensagens (somente macOS)
- Telegram ou Slack para a interface de chat familiar compartilhada
- Acesso ao sistema de arquivos para controle de inventário
- Processamento de câmera/foto para OCR de calendários físicos

## Como Configurar

### 1. Centralização de Calendários

Configure o OpenClaw para coletar dados de todas as fontes de calendário da família:

```text
## Fontes de Calendário

No boletim matinal (08:00):

1. Buscar meu Google Calendar de Trabalho (OAuth somente leitura)
2. Buscar Google Calendar Familiar compartilhado
3. Buscar calendário do parceiro/parceira (visão compartilhada)
4. Verificar ~/Documents/school-calendars/ em busca de novos PDFs → OCR e extrair eventos
5. Checar e-mails recentes por anexos de calendário ou convites de eventos

Compilar em um único resumo:
- Eventos de hoje (todos os calendários, destacados com cores por fonte)
- Previsão dos próximos 3 dias para identificar conflitos
- Novos eventos adicionados desde ontem
- Contexto meteorológico para eventos ao ar livre

Entregar via canal familiar no Telegram/Slack.
```

### 2. Monitoramento de Mensagens em Segundo Plano

Este é o grande diferencial — o agente observa passivamente e age ao reconhecer algo acionável:

```text
## Monitoramento de Mensagens (HEARTBEAT.md)

A cada 15 minutos:
1. Verificar novas iMessages em todas as conversas
2. Detectar padrões que lembrem agendamentos:
   - "Sua consulta está confirmada para..."
   - "Podemos nos encontrar no [dia] às [horas]?"
   - "O treino mudou para sábado às 15h"
3. Quando detectado:
   - Criar evento no calendário com os detalhes
   - Adicionar margem de deslocamento de 30 minutos ANTES e DEPOIS
   - Enviar confirmação no Telegram da família: "Criado: Consulta no dentista, Terça 14h. Margem de deslocamento adicionada: 13:30-14:00 e 15:00-15:30."
   - Se for relevante para o parceiro, adicionar convite
4. Detectar padrões de promessas/compromissos:
   - "Vou te enviar isso até sexta-feira"
   - "Vamos jantar na semana que vem"
   → Criar reserva no calendário ou lembrete
```

### 3. Inventário Doméstico

```text
## Controle de Despensa

Manter ~/household/inventory.json com:
- Nome do item, quantidade, localização (geladeira/despensa/porão)
- Data/hora da última atualização
- Limite mínimo para estoque baixo

Métodos de atualização:
- Foto: Usuário envia foto da geladeira/despensa → modelo de visão extrai os itens
- Texto: "Acabaram os ovos" / "Comprei 2 galões de leite"
- Recibo/Nota Fiscal: Foto do comprovante de compras → atualiza inventário

Consulta: Qualquer um dos parceiros pode perguntar via Telegram:
- "Temos manteiga?" → Checa o inventário, responde com localização e quantidade
- "O que está acabando?" → Lista itens abaixo do limite mínimo
- "Gere a lista de compras" → Compila itens em baixa + ingredientes necessários para receitas
```

## Principais Insights

- **Ambiente > ativo**: O maior divisor de águas é o agente agir sem precisar ser acionado diretamente. Detectar um compromisso em uma mensagem de texto e criar o evento na agenda com margem de deslocamento — "Eu não pedi para ele fazer isso. Ele apenas sabia que era exatamente o que eu gostaria."
- **O Mac Mini é o cenário ideal**: Este caso de uso se beneficia enormemente de rodar em um Mac Mini residencial — integração nativa com iMessage, Apple Calendar e disponibilidade 24/7
- **Comece em modo somente leitura**: Inicie com a leitura de calendários e monitoramento de mensagens antes de liberar ações de escrita (criar eventos, enviar mensagens)
- **Canal compartilhado no Telegram**: Oferece a ambos os parceiros visibilidade sobre o que o agente está fazendo — gera confiança e ajuda a pegar erros logo no início
- **A entrada por foto é subestimada**: Tirar uma foto do calendário escolar em PDF ou do conteúdo do freezer é muito mais rápido do que digitar — e os modelos de visão lidam com isso com facilidade

## Inspirado Por

Este caso de uso combina diversos padrões da comunidade:

- **Centralização de calendários**: Descrito pelo usuário `angiolillo` do HN em [uma discussão no Hacker News](https://news.ycombinator.com/item?id=46872465), que detalhou a frustração de checar separadamente toda manhã calendários de trabalho, pessoal, familiar e da escola dos filhos.
- **Monitoramento de mensagens em segundo plano**: Documentado pelo [Sparkry AI](https://sparkryai.substack.com/p/24-hours-with-openclaw-the-ai-setup) — quando sua esposa recebeu um SMS de consulta no dentista, o OpenClaw criou automaticamente o evento na agenda com 30 minutos de margem para deslocamento, sem que ninguém precisasse pedir. Também confirmado no [OpenClaw Showcase](https://openclaw.ai/showcase), onde `@theaaron` chamou a gestão de calendário via chat de "um dos melhores usos de um LLM que já experimentei."
- **Coordenação doméstica**: Brandon Wang com o [Clawdbot "Linguini"](https://brandon.wang/2026/clawdbot) rodando em um Mac Mini em casa — gerenciando follow-ups de SMS, criando eventos a partir de fotos, monitorando preços do Airbnb, processando fotos de estoque do freezer e coordenando a logística da casa via iMessage e Slack.
- **Controle de despensa**: Vários usuários do HN discutiram o valor (e o desafio) de manter o inventário da casa, com `dns_snek` destacando: "Esqueço onde coloquei as coisas 5 segundos atrás... É um problema real para mim porque acabo deixando coisas vencerem."

## Links Relacionados

- [OpenClaw iMessage Skill](https://github.com/openclaw/openclaw)
- [Google Calendar API](https://developers.google.com/calendar)
- [Apple Calendar (EventKit)](https://developer.apple.com/documentation/eventkit)
- [OpenClaw Showcase — Calendar Testimonials](https://openclaw.ai/showcase)
