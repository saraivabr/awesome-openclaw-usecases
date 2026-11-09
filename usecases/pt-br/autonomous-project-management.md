# Gestão Autônoma de Projetos com Subagentes

Gerenciar projetos complexos com múltiplos fluxos de trabalho paralelos é exaustivo. Você acaba alternando de contexto constantemente, monitorando o status em várias ferramentas e coordenando repasses de tarefas (handoffs) manualmente.

Este caso de uso implementa um padrão descentralizado de gestão de projetos no qual subagentes trabalham de forma autônoma nas tarefas, coordenando-se por meio de arquivos de estado compartilhados em vez de depender de um orquestrador central.

## O Problema

Padrões tradicionais com orquestrador central geram gargalos — o agente principal vira um guarda de trânsito. Para projetos complexos (refatorações em múltiplos repositórios, sprints de pesquisa, pipelines de conteúdo), você precisa de agentes que possam trabalhar em paralelo sem supervisão constante.

## O Que Ele Faz

- **Coordenação descentralizada**: Os agentes leem e escrevem em um arquivo compartilhado `STATE.yaml`
- **Execução paralela**: Múltiplos subagentes trabalham em tarefas independentes simultaneamente
- **Sem sobrecarga de orquestrador**: A sessão principal permanece enxuta (padrão CEO — apenas estratégia)
- **Autodocumentado**: Todo o estado das tarefas persiste em arquivos com controle de versão

## Padrão Central: STATE.yaml

Cada projeto mantém um arquivo `STATE.yaml` que serve como fonte única da verdade:

```yaml
# STATE.yaml - Arquivo de coordenação do projeto
project: website-redesign
updated: 2026-02-10T14:30:00Z

tasks:
  - id: homepage-hero
    status: in_progress
    owner: pm-frontend
    started: 2026-02-10T12:00:00Z
    notes: "Trabalhando no layout responsivo"
    
  - id: api-auth
    status: done
    owner: pm-backend
    completed: 2026-02-10T14:00:00Z
    output: "src/api/auth.ts"
    
  - id: content-migration
    status: blocked
    owner: pm-content
    blocked_by: api-auth
    notes: "Aguardando novo schema do endpoint"

next_actions:
  - "pm-content: Retomar migração agora que api-auth foi concluído"
  - "pm-frontend: Revisar hero com a equipe de design"
```

## Como Funciona

1. **Agente principal recebe a tarefa** → instancia um subagente com escopo específico
2. **Subagente lê o STATE.yaml** → localiza suas tarefas atribuídas
3. **Subagente trabalha de forma autônoma** → atualiza o STATE.yaml conforme progride
4. **Outros agentes consultam o STATE.yaml** → assumem tarefas desbloqueadas
5. **Agente principal faz checagens periódicas** → revisa o estado e ajusta prioridades

## Skills Necessárias

- `sessions_spawn` / `sessions_send` para gerenciamento de subagentes
- Acesso ao sistema de arquivos para o STATE.yaml
- Git para versionamento de estado (opcional, mas recomendado)

## Configuração: Definição em AGENTS.md

```text
## Padrão de Delegação de PM

Sessão principal = APENAS coordenador. Toda a execução vai para subagentes.

Fluxo de trabalho:
1. Nova tarefa chega
2. Verifica em PROJECT_REGISTRY.md se já existe um PM
3. Se o PM existir → sessions_send(label="pm-xxx", message="[tarefa]")
4. Se for novo projeto → sessions_spawn(label="pm-xxx", task="[tarefa]")
5. O PM executa, atualiza o STATE.yaml e reporta o resultado
6. O agente principal resume para o usuário

Regras:
- Sessão principal: máximo de 0-2 chamadas de ferramentas (apenas spawn/send)
- Os PMs são donos de seus arquivos STATE.yaml
- Os PMs podem instanciar sub-subagentes para subtarefas paralelas
- Todas as alterações de estado são commitadas no git
```

## Exemplo: Instanciando um PM

```text
Usuário: "Refatore o módulo de autenticação e atualize a documentação"

Agente principal:
1. Verifica o registro → nenhum pm-auth ativo
2. Instancia: sessions_spawn(
     label="pm-auth-refactor",
     task="Refatorar módulo de autenticação, atualizar docs. Acompanhar em STATE.yaml"
   )
3. Responde: "Instanciei pm-auth-refactor. Avisarei assim que for concluído."

Subagente PM:
1. Cria o STATE.yaml com o detalhamento das tarefas
2. Executa as tarefas, atualizando o status
3. Commita as alterações
4. Reporta a conclusão para o agente principal
```

## Principais Insights

- **STATE.yaml > orquestrador**: Coordenação baseada em arquivos escala melhor do que troca de mensagens
- **Git como log de auditoria**: Commite as alterações de STATE.yaml para manter o histórico completo
- **Convenções de label importam**: Use `pm-{projeto}-{escopo}` para facilitar o rastreamento
- **Sessão principal enxuta**: Quanto menos o agente principal fizer, mais rápido ele responde

## Baseado Em

Este padrão é inspirado na [abordagem de Nicholas Carlini](https://nicholas.carlini.com/) para agentes de programação autônomos — permitir que os agentes se auto-organizem em vez de microgerenciá-los.

## Links Relacionados

- [Documentação de Subagentes do OpenClaw](https://github.com/openclaw/openclaw)
- [Anthropic: Construindo Agentes Eficazes](https://www.anthropic.com/research/building-effective-agents)
