# Base de Conhecimento Pessoal (RAG)

Você lê artigos, tweets e assiste a vídeos o dia todo, mas nunca consegue encontrar aquilo que viu na semana passada. Os favoritos se acumulam e acabam se tornando inúteis.

Este fluxo constrói uma base de conhecimento consultável a partir de tudo o que você salva:

• Envie qualquer URL no Telegram ou Slack e o conteúdo é indexado automaticamente (artigos, tweets, transcrições do YouTube, PDFs)  
• Busca semântica sobre tudo o que você salvou: "O que eu salvei sobre memória de agentes?" retorna resultados ranqueados com fontes  
• Alimenta outros fluxos de trabalho — por exemplo, o pipeline de ideias de vídeo consulta a base de conhecimento em busca de conteúdos salvos relevantes ao criar cartões de pesquisa  

## Skills Necessárias

- Skill [knowledge-base](https://clawhub.ai) (ou crie um RAG personalizado com embeddings)
- `web_fetch` (embutido)
- Tópico no Telegram ou canal no Slack para indexação

## Como Configurar

1. Instale a skill knowledge-base a partir do ClawHub.
2. Crie um tópico no Telegram chamado "knowledge-base" (ou use um canal no Slack).
3. Dê o prompt ao OpenClaw:
```text
Quando eu enviar uma URL no tópico "knowledge-base":
1. Baixe o conteúdo (artigo, tweet, transcrição do YouTube, PDF)
2. Indexe-o na base de conhecimento com metadados (título, URL, data, tipo)
3. Responda confirmando: o que foi indexado e a quantidade de fragmentos (chunks)

Quando eu fizer uma pergunta neste tópico:
1. Pesquise semanticamente na base de conhecimento
2. Retorne os melhores resultados com fontes e trechos relevantes
3. Se não houver boas correspondências, me avise

Além disso: quando outros fluxos de trabalho precisarem de pesquisa (ex.: ideias de vídeo, preparação para reuniões), consulte automaticamente a base de conhecimento em busca de conteúdos relevantes já salvos.
```

4. Teste enviando algumas URLs e fazendo perguntas como: "O que tenho salvo sobre memória de LLMs?"
