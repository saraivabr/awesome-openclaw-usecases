# Assistente Pessoal Multicanal

Alternar de contexto entre aplicativos para gerenciar tarefas, agendar eventos, enviar mensagens e acompanhar o trabalho é exaustivo. O ideal é ter uma única interface que faça a ponte com todas as suas ferramentas.

Este fluxo de trabalho consolida tudo em um único assistente de IA:

• Telegram como interface principal com roteamento baseado em tópicos (tópicos diferentes para ideias de vídeos, CRM, finanças, configurações, etc.)  
• Integração com o Slack para colaboração em equipe (atribuição de tarefas, salvamento na base de conhecimento, gatilhos de ideias para vídeos)  
• Google Workspace: crie eventos no calendário, gerencie e-mails, faça upload para o Drive — tudo a partir do chat  
• Todoist para captura rápida de tarefas  
• Asana para gerenciamento de projetos  
• Lembretes automatizados: dia do lixo, atualização semanal da empresa, etc.  

## Skills Necessárias

- `gog` CLI (Google Workspace)
- Integração com o Slack (tokens de bot + usuário)
- API ou skill do Todoist
- API ou skill da Asana
- Canal ou supergrupo no Telegram com múltiplos tópicos configurados

## Como Configurar

1. Configure tópicos no Telegram para diferentes contextos:
   - `config` — configurações do bot e depuração
   - `updates` — status e notificações
   - `video-ideas` — pipeline de conteúdo
   - `personal-crm` — gestão de contatos
   - `earnings` — acompanhamento financeiro
   - `knowledge-base` — ingestão e consultas de RAG

2. Conecte todas as suas ferramentas pela configuração do OpenClaw:
   - Google OAuth (Gmail, Agenda, Drive)
   - Slack (tokens de app + usuário)
   - Token de API do Todoist
   - Token de API da Asana

3. Prompt para o OpenClaw:
```text
Você é o meu assistente multicanal. Direcione as solicitações com base no contexto:

Tópicos do Telegram:
- "config" → configurações do sistema, depuração
- "updates" → resumos diários, lembretes, calendário
- "video-ideas" → pipeline de conteúdo e pesquisa
- "personal-crm" → consultas sobre contatos e preparação para reuniões
- "earnings" → acompanhamento financeiro
- "knowledge-base" → salvar e pesquisar conteúdo

Quando eu pedir para você:
- "Adicionar [tarefa] aos meus afazeres" → use o Todoist
- "Criar um card para [tópico]" → use o projeto Video Pipeline na Asana
- "Agendar [evento]" → use o gog calendar
- "Enviar e-mail para [pessoa] sobre [tópico]" → rascunhe o e-mail via gog gmail
- "Fazer upload de [arquivo] para o Drive" → use o gog drive

Configure lembretes automatizados:
- Segunda-feira às 18:00: "🗑️ Dia de colocar o lixo para fora amanhã"
- Sexta-feira às 15:00: "✍️ Hora de escrever o boletim semanal da empresa"
```

4. Teste cada integração individualmente e, depois, teste interações combinadas entre fluxos (por exemplo, salvar um link do Slack na base de conhecimento e em seguida usá-lo em um card de pesquisa de vídeo).
