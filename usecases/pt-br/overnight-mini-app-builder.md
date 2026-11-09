# Tarefas Autônomas Orientadas a Metas

Seu agente de IA é poderoso, mas reativo — ele só trabalha quando você diz o que fazer. E se ele conhecesse seus objetivos e criasse tarefas proativamente todos os dias para aproximar você deles, sem precisar ser solicitado?

Este fluxo de trabalho transforma o OpenClaw em um colaborador autodirigido. Você descarrega suas metas e missões de uma só vez, e o agente gera, agenda e conclui tarefas autonomamente para fazer seus objetivos avançarem — incluindo o desenvolvimento de mini-aplicativos surpresa durante a noite.

## O Que Ele Faz

- Você descarrega todas as suas metas, missões e objetivos no OpenClaw (pessoais e profissionais)
- Todas as manhãs, o agente gera de 4 a 5 tarefas que pode concluir de forma autônoma no seu computador
- As tarefas vão muito além da criação de apps: pesquisas, redação de scripts, implementação de recursos, criação de conteúdo e análise de concorrentes
- O próprio agente executa as tarefas e as gerencia em um quadro Kanban personalizado construído para você
- Você também pode pedir para ele desenvolver um mini-aplicativo surpresa toda noite — uma nova ideia de SaaS ou uma ferramenta que automatize tarefas repetitivas, entregue como um MVP

## O Problema

A maioria das pessoas tem grandes metas, mas tem dificuldade em desdobrá-las em ações práticas diárias. E mesmo quando conseguem, a execução consome todo o seu tempo. Este sistema delega tanto o planejamento QUANTO a execução ao seu agente de IA. Você define o destino; o agente planeja os passos diários e os executa.

## Skills Necessárias

- Integração com Telegram ou Discord
- `sessions_spawn` / `sessions_send` para execução autônoma de tarefas
- Next.js ou similar (para o quadro Kanban — o OpenClaw constrói para você)

## Como Configurar

### Passo 1: Despejo Mental das Suas Metas (Brain Dump)

Este é o passo mais importante. Envie por mensagem para o OpenClaw tudo o que você pretende alcançar:

```text
Aqui estão minhas metas e missões. Guarde tudo isso:

Carreira:
- Fazer meu canal no YouTube atingir 100 mil inscritos
- Lançar meu produto SaaS até o 3º trimestre
- Construir uma comunidade sobre educação em IA

Pessoal:
- Ler 2 livros por mês
- Aprender espanhol

Negócios:
- Escalar o faturamento para US$ 10.000/mês
- Fechar parcerias com 5 empresas do meu setor
- Automatizar o máximo possível do meu fluxo de trabalho

Use este contexto como base para tudo o que fizer daqui para frente.
```

### Passo 2: Configurar Tarefas Diárias Autônomas

```text
Todas as manhãs, às 08:00, sugira de 4 a 5 tarefas que você possa realizar
no meu computador hoje para me aproximar das minhas metas.

Em seguida, agende e execute essas tarefas você mesmo. Exemplos:
- Pesquisar concorrentes e redigir relatórios analíticos
- Rascunhar roteiros de vídeo a partir de temas em alta
- Construir novas funcionalidades para meus apps
- Escrever e agendar publicações em redes sociais
- Mapear potenciais parcerias de negócios
- Criar um mini-app surpresa em MVP que me aproxime de um dos meus objetivos

Acompanhe tudo em um quadro Kanban. Atualize o quadro à medida que concluir as tarefas.
```

### Passo 3: Construir o Quadro Kanban (Opcional)

```text
Crie para mim um quadro Kanban em Next.js para visualizar todas as tarefas
que você está executando. Mostre colunas para A Fazer, Em Andamento e Concluído.
Atualize em tempo real conforme concluir cada tarefa.
```

## Principais Aprendizados

- O **despejo de ideias (brain dump) é fundamental**. Quanto mais contexto você fornecer sobre suas metas, melhores serão as tarefas diárias do agente. Não economize detalhes.
- O agente descobre tarefas nas quais você nem teria pensado. Ele correlaciona objetivos e identifica oportunidades que passariam despercebidas.
- O quadro Kanban transforma seu agente em um membro de equipe com progresso transparente. Você consegue ver exatamente o que ele produziu e corrigir a rota se necessário.
- Para a criação noturna de aplicativos: instrua-o explicitamente a construir MVPs sem complexidade desnecessária. Você acordará toda manhã com uma novidade pronta.
- Isso gera um efeito composto ao longo do tempo — o agente aprende quais tipos de tarefas geram mais valor e se adapta continuamente.

## Armadilhas e Padrões (Aprendidos em Produção)

### ⚠️ Condição de Corrida (Race Condition): Subagentes Editando Arquivos Compartilhados

Ao rodar esse fluxo com subagentes, tanto a sessão principal quanto os subagentes disparados podem tentar atualizar o mesmo arquivo de tarefas (ex.: seu Kanban/AUTONOMOUS.md). Isso causa falhas silenciosas.

**Por que isso acontece:** A ferramenta `edit` do OpenClaw exige correspondência exata do `oldText`. Se um subagente atualizar uma linha entre o momento em que a sessão principal lê o arquivo e tenta editá-lo, o texto não coincidirá mais — e a edição falhará silenciosamente.

**A solução: dividir o arquivo de tarefas em duas funções:**

1. **`AUTONOMOUS.md`** — mantém-se pequeno e limpo. Contém apenas os objetivos + backlog aberto. Apenas a sessão principal edita este arquivo. Subagentes nunca devem alterá-lo.

2. **`memory/tasks-log.md`** — arquivo de log somente para anexação (append-only). Os subagentes apenas *adicionam novas linhas no final*. Jamais editam linhas já existentes.

```markdown
# tasks-log.md — Tarefas Concluídas (somente adição)
# Subagentes: sempre adicionem ao FINAL. Nunca editem linhas existentes.

### 2026-02-24
- ✅ TASK-001: Pesquisar concorrentes → research/competitors.md
- ✅ TASK-002: Rascunhar artigo de blog → drafts/post-1.md
```

Esse padrão segue a mesma lógica do histórico de commits do Git: você não reescreve o passado, apenas adiciona novas entradas. Isso elimina de vez condições de corrida e traz outro benefício: o `AUTONOMOUS.md` fica leve, poupando tokens toda vez que é lido em uma verificação de rotina (heartbeat).

**Regra para instruir seu agente:** Nas instruções de inicialização de subagentes, sempre adicione:
> "Ao concluir, anexe uma linha com ✅ ao `memory/tasks-log.md`. Nunca edite o `AUTONOMOUS.md` diretamente."

### 💡 Mantenha o AUTONOMOUS.md Leve em Tokens

O arquivo de tarefas é lido a cada verificação de heartbeat. Se ele crescer descontroladamente com tarefas finalizadas, consumirá tokens desnecessários.

Mantenha o `AUTONOMOUS.md` abaixo de ~50 linhas: somente objetivos (em linhas curtas) + backlog em aberto. Arquive tarefas concluídas em um arquivo separado, acessado apenas quando necessário.

## Baseado Em

Inspirado por [Alex Finn](https://www.youtube.com/watch?v=UTCi_q6iuCM&t=414s) e seu [vídeo sobre casos de uso transformadores do OpenClaw](https://www.youtube.com/watch?v=41_TNGDDnfQ).

## Links Relacionados

- [OpenClaw Memory System](https://github.com/openclaw/openclaw)
- [OpenClaw Subagent Docs](https://github.com/openclaw/openclaw)
