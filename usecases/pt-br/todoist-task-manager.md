# Gerenciador de Tarefas no Todoist: Visibilidade das Tarefas do Agente

Aumente a transparência em fluxos de trabalho autônomos de longa duração sincronizando o raciocínio interno e os registros de progresso diretamente no Todoist.

## O Problema

Quando agentes executam tarefas complexas de múltiplas etapas (como desenvolver uma aplicação full-stack ou realizar uma pesquisa aprofundada), o usuário com frequência perde a noção do que o agente está fazendo no momento, quais etapas já foram concluídas e onde ele pode ter travado. Checar históricos de chat manualmente é exaustivo para tarefas em segundo plano.

## O Que Ele Faz

Este caso de uso utiliza a abordagem do `todoist-task-manager` para:
1.  **Visualizar o Estado**: Criar tarefas em seções específicas como `🟡 In Progress` ou `🟠 Waiting`.
2.  **Externalizar o Raciocínio**: Publicar o "Plano" interno do agente na descrição da tarefa.
3.  **Transmitir Logs de Progresso**: Adicionar a conclusão de cada subetapa como comentários na tarefa em tempo real.
4.  **Auto-Reconciliação**: Um script de heartbeat verifica tarefas paralisadas e notifica o usuário.

## Skills Necessárias

Você não precisa de uma skill pré-instalada. Basta pedir ao seu agente OpenClaw para criar os scripts em bash descritos no **Guia de Configuração** abaixo. Como o OpenClaw pode gerenciar seu próprio sistema de arquivos e executar comandos de shell, ele efetivamente "construirá" a skill para você sob demanda.

## Guia Detalhado de Configuração

### 1. Configurar o Todoist
Crie um projeto (por exemplo, "OpenClaw Workspace") e obtenha seu ID. Crie seções para os diferentes estados:
- `🟡 In Progress`
- `🟠 Waiting`
- `🟢 Done`

### 2. Implementação: A Skill "Construída pelo Agente"
Em vez de instalar uma skill de terceiros, você pode pedir ao OpenClaw que crie estes scripts para você. Cada script lida com uma parte diferente da comunicação com a API do Todoist.

**`scripts/todoist_api.sh`** (Wrapper Principal):
```bash
#!/bin/bash
# Uso: ./todoist_api.sh <endpoint> <method> [data_json]
ENDPOINT=$1
METHOD=$2
DATA=$3
TOKEN="YOUR_TODOIST_API_TOKEN"

if [ -z "$DATA" ]; then
  curl -s -X "$METHOD" "https://api.todoist.com/rest/v2/$ENDPOINT" \
    -H "Authorization: Bearer $TOKEN"
else
  curl -s -X "$METHOD" "https://api.todoist.com/rest/v2/$ENDPOINT" \
    -H "Authorization: Bearer $TOKEN" \
    -H "Content-Type: application/json" \
    -d "$DATA"
fi
```

**`scripts/sync_task.sh`** (Gerenciamento de Tarefas e Status):
```bash
#!/bin/bash
# Uso: ./sync_task.sh <task_content> <status> [task_id] [description] [labels_json_array]
CONTENT=$1
STATUS=$2
TASK_ID=$3
DESCRIPTION=$4
LABELS=$5
PROJECT_ID="YOUR_PROJECT_ID"

case $STATUS in
  "In Progress") SECTION_ID="SECTION_ID_PROGRESS" ;;
  "Waiting")     SECTION_ID="SECTION_ID_WAITING" ;;
  "Done")        SECTION_ID="SECTION_ID_DONE" ;;
  *)             SECTION_ID="" ;;
esac

PAYLOAD="{\"content\": \"$CONTENT\""
[ -n "$SECTION_ID" ] && PAYLOAD="$PAYLOAD, \"section_id\": \"$SECTION_ID\""
[ -n "$PROJECT_ID" ] && [ -z "$TASK_ID" ] && PAYLOAD="$PAYLOAD, \"project_id\": \"$PROJECT_ID\""
if [ -n "$DESCRIPTION" ]; then
  ESC_DESC=$(echo "$DESCRIPTION" | sed ':a;N;$!ba;s/\n/\\n/g' | sed 's/"/\\"/g')
  PAYLOAD="$PAYLOAD, \"description\": \"$ESC_DESC\""
fi
[ -n "$LABELS" ] && PAYLOAD="$PAYLOAD, \"labels\": $LABELS"
PAYLOAD="$PAYLOAD}"

if [ -n "$TASK_ID" ]; then
  ./scripts/todoist_api.sh "tasks/$TASK_ID" POST "$PAYLOAD"
else
  ./scripts/todoist_api.sh "tasks" POST "$PAYLOAD"
fi
```

**`scripts/add_comment.sh`** (Registro de Progresso):
```bash
#!/bin/bash
# Uso: ./add_comment.sh <task_id> <comment_text>
TASK_ID=$1
TEXT=$2
ESC_TEXT=$(echo "$TEXT" | sed ':a;N;$!ba;s/\n/\\n/g' | sed 's/"/\\"/g')
PAYLOAD="{\"task_id\": \"$TASK_ID\", \"content\": \"$ESC_TEXT\"}"
./scripts/todoist_api.sh "comments" POST "$PAYLOAD"
```

### 3. Prompt de Uso
Você pode passar este prompt ao seu agente tanto para **configurar** quanto para **utilizar** o sistema de visibilidade:

```text
Quero que você construa um sistema de visibilidade de tarefas baseado no Todoist para suas próprias execuções.

Primeiro, crie três scripts em bash em uma pasta 'scripts/':
1. todoist_api.sh (um wrapper com curl para a API REST do Todoist)
2. sync_task.sh (para criar ou atualizar tarefas com section_ids específicos para In Progress, Waiting e Done)
3. add_comment.sh (para publicar logs de progresso como comentários)

Utilize estas variáveis para a configuração:
- Token: [Seu Token de API do Todoist]
- Project ID: [O ID do seu Projeto]
- Section IDs: [ID de In Progress, ID de Waiting, ID de Done]

Depois de criados, para cada tarefa complexa que eu lhe passar:
1. Crie uma tarefa em 'In Progress' com o seu PLANO completo na descrição.
2. A cada subetapa concluída, chame add_comment.sh com um registro detalhado do que foi feito.
3. Mova a tarefa para 'Done' quando terminar.
```

## Links Relacionados
- [Documentação da API REST do Todoist](https://developer.todoist.com/rest/v2/)
