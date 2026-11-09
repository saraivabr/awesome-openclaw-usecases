# Pipeline de Conteúdo para YouTube

Para criadores com frequência diária no YouTube, encontrar ideias de vídeos originais e oportunas na web e no X/Twitter é uma tarefa demorada. Acompanhar o que você já produziu evita repetições desnecessárias e ajuda a antecipar tendências.

Este fluxo automatiza todo o pipeline de pesquisa e prospecção de conteúdo:

• Uma tarefa cron executada de hora em hora varre as últimas novidades sobre IA (na web e no X/Twitter) e envia sugestões de pautas para o Telegram  
• Mantém um catálogo dos vídeos dos últimos 90 dias com contagem de visualizações e análise temática para evitar abordar os mesmos assuntos  
• Salva todas as sugestões em um banco de dados SQLite com embeddings vetoriais para deduplicação semântica (garantindo que você nunca receba a mesma ideia duas vezes)  
• Sempre que você compartilha um link no Slack, o OpenClaw pesquisa o assunto, busca publicações relacionadas no X, consulta sua base de conhecimento e cria um cartão no Asana com um roteiro estruturado completo  

## Skills Necessárias

- `web_search` (integrada nativamente)
- [x-research-v2](https://clawhub.ai) ou skill customizada de busca no X/Twitter
- Skill [knowledge-base](https://clawhub.ai) para RAG
- Integração com Asana (ou Todoist)
- CLI `gog` para acesso ao YouTube Analytics
- Tópico no Telegram para receber sugestões de pautas

## Como Configurar

1. Crie um tópico no Telegram para ideias de vídeos e configure-o no OpenClaw.
2. Instale as skills de knowledge-base e x-research.
3. Crie um banco de dados SQLite para rastrear as sugestões:
```sql
CREATE TABLE pitches (
  id INTEGER PRIMARY KEY,
  timestamp TEXT,
  topic TEXT,
  embedding BLOB,
  sources TEXT
);
```
4. Envie este prompt para o OpenClaw:
```text
Execute uma tarefa cron a cada hora para:
1. Buscar notícias urgentes de IA na web e no X/Twitter
2. Comparar com o catálogo dos últimos 90 dias do meu canal (busque no YouTube Analytics via gog)
3. Verificar a similaridade semântica contra todas as sugestões anteriores no banco de dados
4. Se for um conteúdo inédito, envie a proposta para o tópico "ideias de vídeo" no meu Telegram acompanhada das fontes

Além disso: sempre que eu compartilhar um link no Slack em #ai_trends, automaticamente:
1. Pesquise sobre o tópico
2. Busque publicações relacionadas no X
3. Consulte minha base de conhecimento
4. Crie um cartão no Asana em Video Pipeline com uma estrutura completa de roteiro
```
