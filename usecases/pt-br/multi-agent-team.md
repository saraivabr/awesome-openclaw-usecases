# Equipe Especializada Multi-Agente (Setup para Fundador Solo)

Fundadores solo assumem todos os papéis — estratégia, desenvolvimento, marketing, vendas e operações. A alternância constante de contexto entre essas funções destrói o foco no trabalho profundo. Contratar pessoas é caro e demorado. E se você pudesse criar uma equipe pequena e especializada de agentes de IA, cada um com uma função e personalidade distintas, todos controláveis a partir de uma única interface de chat?

Este caso de uso configura múltiplos agentes OpenClaw como uma equipe coordenada, cada um especializado em uma área, comunicando-se por meio de memória compartilhada e controlados via Telegram.

## O Problema

- **Um único agente não faz tudo bem**: A janela de contexto de um único agente lota rapidamente ao tentar equilibrar estratégia, código, pesquisa de marketing e análise de negócios.
- **Falta de especialização**: Prompts genéricos produzem resultados genéricos — um agente de programação não deveria escrever textos de marketing.
- **Burnout do fundador solo**: Você precisa de uma equipe, não de mais uma ferramenta para gerenciar. Os agentes devem trabalhar em segundo plano e apresentar resultados, em vez de exigir supervisão constante.
- **Silos de conhecimento**: Insights da pesquisa de marketing não informam automaticamente as prioridades de desenvolvimento, a menos que você faça essa ponte manualmente.

## O Que Ele Faz

- **Agentes especializados**: Cada agente possui papel, personalidade e modelo distintos, otimizados para sua respectiva área.
- **Memória compartilhada**: Documentos do projeto, metas e decisões cruciais ficam acessíveis a todos os agentes — nada se perde.
- **Contexto privado**: Cada agente também mantém seu próprio histórico de conversas e anotações específicas do seu domínio.
- **Plano de controle unificado**: Todos os agentes ficam acessíveis através de um único grupo no Telegram — basta marcar o agente que você precisa.
- **Tarefas diárias agendadas**: Os agentes trabalham proativamente sem precisar de comandos manuais — sugestões de conteúdo, monitoramento de concorrentes e rastreamento de métricas.
- **Execução paralela**: Múltiplos agentes podem atuar em tarefas independentes de forma simultânea.

## Exemplo de Configuração de Equipe

### Agente 1: Milo (Líder de Estratégia)

```text
## SOUL.md — Milo

Você é o Milo, o líder da equipe. Confiante, com visão macro e carismático.

Responsabilidades:
- Planejamento estratégico e priorização
- Coordenação dos demais agentes
- Definição de metas semanais e acompanhamento de OKRs
- Sintetizar percepções de todos os agentes em decisões acionáveis

Modelo: Claude Opus
Canal: Telegram (responde a @milo)

Tarefas diárias:
- 08:00: Revisar a atividade noturna dos agentes, postar resumo da reunião matinal (standup)
- 18:00: Recapitulação do fim do dia com o progresso em relação às metas semanais
```

### Agente 2: Josh (Negócios e Crescimento)

```text
## SOUL.md — Josh

Você é o Josh, o analista de negócios. Pragmático, direto ao ponto e orientado a números.

Responsabilidades:
- Estratégia de precificação e análise de concorrência
- Métricas de crescimento e acompanhamento de KPIs
- Modelagem de receita e unit economics
- Análise de feedback dos clientes

Modelo: Claude Sonnet (rápido, analítico)
Canal: Telegram (responde a @josh)

Tarefas diárias:
- 09:00: Coletar e resumir as principais métricas
- Acompanhar mudanças de preços dos concorrentes semanalmente
```

### Agente 3: Agente de Marketing

```text
## SOUL.md — Agente de Marketing

Você é o pesquisador de marketing. Criativo, curioso e atento a tendências.

Responsabilidades:
- Ideação e rascunho de conteúdo
- Monitoramento de redes sociais dos concorrentes
- Rastreamento de tendências no Reddit/HN/X para tópicos relevantes
- Pesquisa de palavras-chave para SEO

Modelo: Gemini (forte em pesquisa na web e análise de contexto longo)
Canal: Telegram (responde a @marketing)

Tarefas diárias:
- 10:00: Apresentar 3 ideias de conteúdo baseadas em tópicos em alta
- Monitorar menções diárias aos concorrentes no Reddit/X
- Rascunho semanal do calendário de conteúdo
```

### Agente 4: Agente Dev

```text
## SOUL.md — Agente Dev

Você é o agente desenvolvedor. Preciso, minucioso e consciente da segurança.

Responsabilidades:
- Decisões de programação e arquitetura
- Revisão de código e verificações de qualidade
- Investigação e correção de bugs
- Documentação técnica

Modelo: Claude Opus / Codex (para implementação)
Canal: Telegram (responde a @dev)

Tarefas diárias:
- Checar a saúde do pipeline de CI/CD
- Revisar PRs abertos
- Apontar itens de débito técnico
```

## Skills Necessárias

- `telegram` skill para a interface unificada de controle
- `sessions_spawn` / `sessions_send` para coordenação multi-agente
- Sistema de arquivos compartilhado ou ferramenta de notas para a memória da equipe
- Chaves de API individuais para diferentes provedores de modelos (caso utilize modelos mistos)
- Uma VPS ou máquina sempre ativa para rodar os agentes

## Como Configurar

### 1. Estrutura de Memória Compartilhada

```text
team/
├── GOALS.md           # OKRs atuais e prioridades (lido por todos os agentes)
├── DECISIONS.md       # Registro de decisões principais (somente adição)
├── PROJECT_STATUS.md  # Estado atual do projeto (atualizado por todos)
├── agents/
│   ├── milo/          # Contexto privado e anotações do Milo
│   ├── josh/          # Contexto privado do Josh
│   ├── marketing/     # Pesquisas do agente de marketing
│   └── dev/           # Anotações técnicas do agente dev
```

### 2. Roteamento no Telegram

Configure um único grupo no Telegram onde todos os agentes escutam, mas cada um responde apenas quando marcado:

```text
## AGENTS.md — Roteamento no Telegram

Grupo do Telegram: "Team"

Roteamento:
- @milo → Agente de estratégia (inicia/retoma sessão do milo)
- @josh → Agente de negócios (inicia/retoma sessão do josh)
- @marketing → Agente de marketing (inicia/retoma sessão de marketing)
- @dev → Agente dev (inicia/retoma sessão do dev)
- @all → Transmissão para todos os agentes
- Sem marcação → Milo (líder da equipe) assume por padrão

Cada agente:
1. Lê os arquivos compartilhados GOALS.md e PROJECT_STATUS.md para contexto
2. Lê suas próprias anotações privadas
3. Processa a mensagem
4. Responde no Telegram
5. Atualiza arquivos compartilhados se a resposta envolver uma decisão ou mudança de status
```

### 3. Tarefas Agendadas

```text
## HEARTBEAT.md — Cronograma da Equipe

Diário:
- 08:00: Milo posta o standup matinal (agrega atividade noturna dos agentes)
- 09:00: Josh coleta as principais métricas
- 10:00: Marketing apresenta ideias de conteúdo a partir de tópicos em alta
- 18:00: Milo posta a recapitulação do fim do dia

Contínuo:
- Dev: Monitorar a saúde do CI/CD, revisar PRs à medida que chegam
- Marketing: Monitoramento de palavras-chave no Reddit/X (a cada 2 horas)
- Josh: Checagem de preços dos concorrentes (semanal)

Semanal:
- Segunda-feira: Milo rascunha as prioridades semanais (com contribuições de todos os agentes)
- Sexta-feira: Josh compila o relatório semanal de métricas
```

## Principais Aprendizados

- **Personalidade importa mais do que você imagina**: Dar aos agentes nomes e estilos de comunicação distintos torna natural "conversar com sua equipe", em vez de lutar contra uma IA genérica.
- **Memória compartilhada + contexto privado**: Essa combinação é fundamental — os agentes precisam de uma base comum (metas, decisões), mas também do seu próprio espaço para acumular conhecimento especializado.
- **O modelo certo para a tarefa certa**: Não use um modelo de raciocínio caro para monitoramento de palavras-chave. Ajuste a capacidade do modelo à complexidade da tarefa.
- **Tarefas agendadas são o verdadeiro motor**: O valor real surge quando os agentes apresentam percepções de forma proativa, e não apenas quando solicitados.
- **Comece com 2, não com 4**: Inicie com um líder + um especialista, e adicione mais agentes conforme identificar gargalos.

## Inspirado Por

Este padrão foi descrito por [Trebuh no X](https://x.com/iamtrebuh/status/2011260468975771862), um fundador solo que configurou 4 agentes OpenClaw — Milo (líder de estratégia), Josh (negócios), um agente de marketing e um agente dev — todos controlados através de um único chat do Telegram em uma VPS. Cada agente tem sua própria personalidade, modelo e tarefas agendadas, enquanto compartilham a memória do projeto. Ele descreveu isso como "uma equipe pequena de verdade disponível 24 horas por dia, 7 dias por semana".

O padrão também foi confirmado no [OpenClaw Showcase](https://openclaw.ai/showcase), onde `@jdrhyne` relatou operar "mais de 15 agentes, 3 máquinas, 1 servidor Discord — a área técnica construiu a maior parte disso apenas conversando no chat", e `@nateliason` descreveu um pipeline multi-modelo (prototipar → resumir → otimizar → implementar → repetir) usando modelos diferentes em cada estágio. Outro usuário, `@danpeguine`, opera duas instâncias distintas do OpenClaw colaborando no mesmo grupo de WhatsApp.

## Links Relacionados

- [OpenClaw Subagent Documentation](https://github.com/openclaw/openclaw)
- [OpenClaw Telegram Skill](https://github.com/openclaw/openclaw)
- [OpenClaw Showcase](https://openclaw.ai/showcase)
- [Anthropic: Building Effective Agents](https://www.anthropic.com/research/building-effective-agents)
