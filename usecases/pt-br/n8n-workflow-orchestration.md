# Orquestração de Fluxos com OpenClaw + n8n

Permitir que seu agente de IA gerencie diretamente chaves de API e faça chamadas para serviços externos é uma receita para incidentes de segurança. Cada nova integração significa mais uma credencial exposta em `.env.local` e mais uma superfície para o agente vazar ou utilizar dados de forma indevida.

Este caso de uso descreve um padrão no qual o OpenClaw delega todas as interações com APIs externas para fluxos do n8n por meio de webhooks — o agente nunca toca diretamente nas credenciais, e cada integração permanece visualmente inspecionável e passível de bloqueio.

## O Problema

Quando o OpenClaw lida com tudo diretamente, surgem três problemas cumulativos:

- **Falta de visibilidade**: É difícil inspecionar o que o agente realmente construiu quando as rotinas ficam perdidas em arquivos de skill JavaScript ou scripts shell.
- **Proliferação de credenciais**: Cada chave de API reside no ambiente do agente, a apenas um commit equivocado de ser exposta publicamente.
- **Desperdício de tokens**: Subtarefas determinísticas (como enviar um e-mail ou atualizar uma planilha) consomem tokens de raciocínio de LLM quando poderiam rodar como fluxos simples.

## O Que Ele Faz

- **Padrão de Proxy**: O OpenClaw cria fluxos no n8n com webhooks de entrada e, a partir de então, aciona esses webhooks para todas as interações com APIs.
- **Isolamento de credenciais**: As chaves de API ficam guardadas com segurança no cofre de credenciais do n8n — o agente só conhece a URL do webhook.
- **Depuração visual**: Cada fluxo de trabalho pode ser inspecionado na interface de arrastar e soltar do n8n.
- **Fluxos bloqueáveis**: Após construir e testar um fluxo, você pode bloqueá-lo para impedir que o agente altere a forma de comunicação com a API.
- **Etapas de proteção (Safeguards)**: É possível adicionar validações, limitação de taxa (rate limiting) e etapas de aprovação manual no n8n antes que qualquer chamada externa seja disparada.

## Como Funciona

1. **O agente projeta o fluxo**: Diga ao OpenClaw o que você precisa (ex.: "crie um fluxo que envie uma mensagem no Slack quando uma nova issue no GitHub receber a etiqueta `urgent`").
2. **O agente cria o fluxo no n8n**: O OpenClaw cria o fluxo pela API do n8n, incluindo um gatilho de webhook de entrada.
3. **Você adiciona as credenciais**: Abra a interface do n8n e insira seus tokens do Slack e GitHub manualmente.
4. **Você bloqueia o fluxo**: Impede alterações posteriores por parte do agente.
5. **O agente aciona o webhook**: A partir daí, o OpenClaw faz chamadas para `http://n8n:5678/webhook/my-workflow` enviando um JSON — ele nunca tem acesso à chave de API.

```text
┌──────────────┐    chamada webhook    ┌─────────────────┐    chamada de API   ┌──────────────┐
│   OpenClaw   │ ───────────────────→  │    Fluxo n8n    │ ─────────────────→  │   Serviço    │
│   (agente)   │   (sem credenciais)   │  (bloqueado,    │  (credenciais ficam │   Externo    │
│              │                       │   com chaves)   │   apenas aqui)      │ (Slack, etc) │
└──────────────┘                       └─────────────────┘                     └──────────────┘
```

## Skills Necessárias

- Acesso à API do `n8n` (para criar e acionar fluxos)
- `fetch` ou `curl` para chamadas de webhook
- Docker (caso utilize a stack pré-configurada)
- Gerenciamento de credenciais no n8n (configuração manual única por integração)

## Como Configurar

### Opção 1: Stack Docker Pré-configurada

Um ambiente Docker Compose mantido pela comunidade ([openclaw-n8n-stack](https://github.com/caprihan/openclaw-n8n-stack)) já entrega tudo conectado em uma rede Docker compartilhada:

```bash
git clone https://github.com/caprihan/openclaw-n8n-stack.git
cd openclaw-n8n-stack
cp .env.template .env
# Adicione sua chave de API da Anthropic ao .env
docker-compose up -d
```

Essa configuração disponibiliza:
- OpenClaw na porta 3456
- n8n na porta 5678
- Rede Docker compartilhada para que o OpenClaw acesse `http://n8n:5678/webhook/...` diretamente
- Modelos de fluxos pré-construídos (checagem de fatos multi-LLM, triagem de e-mails, monitoramento de redes sociais)

### Opção 2: Configuração Manual

1. Instale o n8n (`npm install n8n -g` ou execute via Docker)
2. Configure o OpenClaw com a URL base da sua instância do n8n
3. Adicione o trecho a seguir ao seu AGENTS.md:

```text
## Padrão de Integração com n8n

Quando eu precisar interagir com APIs externas:

1. NUNCA armazene chaves de API no meu ambiente ou em arquivos de skills
2. Verifique se já existe um fluxo no n8n para esta integração
3. Se não existir, crie um via API do n8n com um gatilho de webhook
4. Notifique o usuário para adicionar as credenciais e bloquear o fluxo
5. Para todas as chamadas futuras, use a URL do webhook com uma carga útil JSON

Padrão de nome do fluxo: openclaw-{serviço}-{ação}
Exemplo: openclaw-slack-send-message

Formato de chamada do webhook:
curl -X POST http://n8n:5678/webhook/{nome-do-fluxo} \
  -H "Content-Type: application/json" \
  -d '{"channel": "#general", "message": "Olá do OpenClaw"}'
```

## Principais Aprendizados

- **Três benefícios em um**: Observabilidade (interface gráfica), segurança (isolamento de credenciais) e desempenho (fluxos determinísticos não consomem tokens).
- **Bloqueie após testar**: O ciclo "construir → testar → bloquear" é fundamental — sem o bloqueio, o agente pode alterar os fluxos silenciosamente.
- **O n8n possui mais de 400 integrações**: A maioria dos serviços que você queira integrar já conta com nós prontos no n8n, poupando o agente de programar chamadas de API personalizadas.
- **Trilha de auditoria nativa**: O n8n registra cada execução de fluxo com o histórico detalhado de entradas e saídas.

## Inspirado Por

Este padrão foi descrito por [Simon Høiberg](https://x.com/SimonHoiberg/status/2020843874382487959), que destacou três motivos pelos quais essa abordagem supera dar acesso direto de API ao OpenClaw: observabilidade pela interface visual do n8n, segurança pelo isolamento de credenciais e eficiência ao rodar subtarefas determinísticas como fluxos de trabalho em vez de chamadas de LLM. O repositório [openclaw-n8n-stack](https://github.com/caprihan/openclaw-n8n-stack) disponibiliza um setup pronto via Docker Compose implementando esse modelo.

## Links Relacionados

- [Documentação do n8n](https://docs.n8n.io/)
- [openclaw-n8n-stack (Setup Docker)](https://github.com/caprihan/openclaw-n8n-stack)
- [Documentação do Gatilho Webhook do n8n](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.webhook/)
