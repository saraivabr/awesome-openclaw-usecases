# Notas de Reunião e Ações Automatizadas

Você acabou de finalizar uma reunião de equipe de 45 minutos. Agora precisa redigir o resumo, extrair os itens de ação e distribuí-los no Jira, Linear ou Todoist — tudo manualmente. Quando termina, a próxima reunião já está começando. E se o seu agente cuidasse de tudo isso no exato instante em que a transcrição ficasse disponível?

Este caso de uso transforma qualquer transcrição de reunião em notas estruturadas e cria tarefas automaticamente na sua ferramenta de gestão de projetos.

## Dores e Desafios

Fazer atas de reunião é cansativo, mas crucial. A maioria das pessoas ou pula essa etapa (e perde o contexto) ou gasta mais de 20 minutos redigindo. Os itens de ação acabam sendo esquecidos porque ficam na cabeça de alguém ou soterrados em conversas de chat. Este agente elimina a distância entre "nós discutimos isso" e "está registrado, priorizado e atribuído".

## O Que Ele Faz

- **Monitora** novas transcrições de reuniões (via exportação do Otter.ai, transcrição do Google Meet, resumo de gravação do Zoom ou colando diretamente no chat)
- **Extrai** decisões importantes, tópicos discutidos e itens de ação com responsáveis e prazos
- **Cria tarefas** no Jira, Linear, Todoist ou Notion — atribuídas à pessoa certa com o contexto da reunião
- **Publica um resumo** no Slack ou Discord para que toda a equipe tenha o registro
- **Faz o acompanhamento (follow-up)** — opcionalmente notifica os responsáveis antes dos prazos por meio de lembretes agendados

## Skills Necessárias

- Integração com Jira, Linear, Todoist ou Notion (para criação de tarefas)
- Integração com Slack ou Discord (para publicação de resumos)
- Acesso ao sistema de arquivos (para leitura de arquivos de transcrição)
- Agendamento / cron (para lembretes de acompanhamento)
- Opcional: API do Otter.ai, Fireflies.ai ou Google Meet para obtenção automática de transcrições

## Como Configurar

1. Escolha a fonte da sua transcrição. A abordagem mais simples é colar a transcrição diretamente no chat. Para automação completa, configure o monitoramento de uma pasta ou integração via API.

2. Dê o prompt ao OpenClaw:
```text
Acabei de finalizar uma reunião. Aqui está a transcrição:

[cole a transcrição ou aponte para o arquivo]

Por favor:
1. Escreva um resumo conciso (máximo de 5 tópicos) cobrindo as decisões e assuntos principais.
2. Extraia TODOS os itens de ação. Para cada um, identifique:
   - O que precisa ser feito
   - Quem é o responsável (associe aos nomes da minha equipe)
   - Prazo (se tiver sido mencionado, caso contrário marque como "A definir")
3. Crie um ticket no Jira para cada item de ação, atribuído à pessoa certa.
4. Publique o resumo completo no canal #meeting-notes no Slack.
```

3. Para um pipeline totalmente automatizado (monitoramento de pasta de transcrições):
```text
Configure uma tarefa recorrente: a cada 30 minutos, verifique ~/meeting-transcripts/ em busca de
novos arquivos .txt ou .vtt. Ao encontrar um:

1. Processe a transcrição em um resumo estruturado com itens de ação.
2. Crie tarefas no Linear para cada item de ação.
3. Publique o resumo no canal #team-updates no Slack.
4. Mova o arquivo processado para ~/meeting-transcripts/processed/.

Para cada item de ação com prazo definido, agende um lembrete para alertar o responsável
no Slack um dia antes da data de entrega.
```

4. Personalize o formato de saída:
```text
Ao escrever resumos de reuniões, use sempre esta estrutura:
- **Data e Participantes** no topo
- **Principais Decisões** — lista numerada do que foi decidido
- **Itens de Ação** — tabela com as colunas: Tarefa, Responsável, Prazo, Status
- **Perguntas Abertas** — qualquer questão não resolvida que precise de acompanhamento
```

## Principais Insights

- O verdadeiro valor não está no resumo — está na **criação automática de tarefas**. Atas de reunião que não se transformam em tarefas acompanhadas são apenas burocracia documental.
- Combine isso com o caso de uso [Gerenciador de Tarefas no Todoist](todoist-task-manager.md) para visibilidade total das tarefas criadas pelo agente.
- Arquivos de legenda VTT/SRT do Zoom ou Google Meet funcionam muito bem como entrada — eles incluem marcadores de tempo (timestamps), o que ajuda o agente a atribuir falas a cada interlocutor.
- Comece de forma simples (cole a transcrição, obtenha o resumo) e automatize aos poucos. Não complique a arquitetura do pipeline antes de validar a qualidade das saídas geradas.

## Links Relacionados

- [Otter.ai API](https://otter.ai/)
- [Jira REST API](https://developer.atlassian.com/cloud/jira/platform/rest/v3/)
- [Linear API](https://developers.linear.app/)
- [Slack API](https://api.slack.com/)
