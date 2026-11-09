# Sistema de Gestão de Estado de Projetos: Alternativa a Quadros Kanban Orientada a Eventos

Quadros Kanban tradicionais são estáticos e exigem atualizações manuais. Você esquece de mover os cards, perde o contexto entre as sessões de trabalho e não consegue registrar o "porquê" por trás das mudanças de estado. Os projetos acabam perdendo o rumo sem uma visibilidade clara.

Este fluxo substitui o Kanban por um sistema orientado a eventos que monitora o estado dos projetos automaticamente:

• Armazena o estado do projeto em um banco de dados com histórico completo  
• Captura contexto: decisões, impedimentos (blockers), próximos passos e principais aprendizados  
• Atualizações orientadas a eventos: "Acabei de finalizar X, bloqueado em Y" → transição automática de estado  
• Consultas em linguagem natural: "Qual é o status do [projeto]?", "Por que pivotamos em [recurso]?"  
• Resumos diários de alinhamento (daily standup): O que aconteceu ontem, o que está planejado para hoje, o que está travado  
• Integração com Git: associa commits a eventos do projeto para rastreabilidade  

## O Problema

Quadros Kanban ficam desatualizados. Você perde tempo atualizando cartões em vez de produzir. O contexto se perde — três meses depois, ninguém lembra por que tomou uma decisão crítica. Além disso, não há uma ligação automática entre alterações de código e o progresso do projeto.

## O Que Ele Faz

Em vez de arrastar cartões, você simplesmente conversa com o seu assistente: "Finalizei o fluxo de autenticação e estou começando o painel de controle." O sistema registra o evento, atualiza o estado do projeto e preserva o contexto. Quando você pergunta "Em que ponto estamos no painel?", ele apresenta a história completa: o que foi feito, o que vem a seguir, o que está travando o avanço e o motivo.

Os commits no Git são varridos automaticamente e associados aos projetos. Seu resumo de daily standup se escreve sozinho.

## Skills Necessárias

- `postgres` ou SQLite para o banco de dados de estado do projeto
- `github` (gh CLI) para rastreamento de commits
- Discord ou Telegram para atualizações e consultas
- Tarefas cron para resumos diários
- Criação de subagentes para análise paralela de projetos

## Como Configurar

1. Configure um banco de dados para o estado dos projetos:
```sql
CREATE TABLE projects (
  id SERIAL PRIMARY KEY,
  name TEXT UNIQUE,
  status TEXT, -- ex.: "active", "blocked", "completed"
  current_phase TEXT,
  last_update TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE events (
  id SERIAL PRIMARY KEY,
  project_id INTEGER REFERENCES projects(id),
  event_type TEXT, -- ex.: "progress", "blocker", "decision", "pivot"
  description TEXT,
  context TEXT,
  timestamp TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE blockers (
  id SERIAL PRIMARY KEY,
  project_id INTEGER REFERENCES projects(id),
  blocker_text TEXT,
  status TEXT DEFAULT 'open', -- "open", "resolved"
  created_at TIMESTAMPTZ DEFAULT NOW(),
  resolved_at TIMESTAMPTZ
);
```

2. Crie um canal no Discord para atualizações de projetos (por exemplo, #project-state).

3. Envie este prompt para o OpenClaw:
```text
Você é o meu gestor de estado de projetos. Em vez de usar Kanban, vou dizer no que estou trabalhando de forma conversacional.

Quando eu disser coisas como:
- "Terminei [tarefa]" → registre um evento de "progress", atualize o estado do projeto
- "Travado em [problema]" → crie uma entrada de impedimento (blocker), altere o status do projeto para "blocked"
- "Iniciando [nova tarefa]" → registre um evento de "progress", atualize a fase atual
- "Decidi [decisão]" → registre um evento de "decision" com o contexto completo
- "Pivotando para [nova abordagem]" → registre um evento de "pivot" com a justificativa

Quando eu perguntar:
- "Qual é o status do [projeto]?" → busque os eventos mais recentes, impedimentos e a fase atual
- "Por que decidimos [X]?" → pesquise os eventos para encontrar o contexto da decisão
- "O que está nos travando?" → liste todos os impedimentos em aberto em todos os projetos

Todas as manhãs, às 9h, execute uma tarefa cron para:
1. Varrer os commits do git das últimas 24 horas (via gh CLI)
2. Vincular os commits aos projetos com base nos nomes das branches ou mensagens de commit
3. Publicar um resumo de daily standup no canal do Discord #project-state:
   - O que aconteceu ontem (eventos + commits)
   - O que está planejado para hoje (com base na fase atual e nas conversas recentes)
   - O que está bloqueado (impedimentos em aberto)

Quando eu estiver planejando uma sprint, crie um subagente para analisar o estado de cada projeto e sugerir prioridades.
```

4. Integre à sua rotina diária: basta conversar naturalmente com o assistente sobre o que você está fazendo. O sistema registra tudo automaticamente.

## Links Relacionados

- [Padrão Event Sourcing](https://martinfowler.com/eaaDev/EventSourcing.html)
- [Por Que o Kanban Falha para Desenvolvedores Solo](https://blog.nuclino.com/why-kanban-doesnt-work-for-me)
