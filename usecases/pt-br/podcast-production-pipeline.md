# Pipeline de Produção de Podcasts

Você tem uma ideia de podcast, talvez até um acervo de temas para novos episódios. Mas entre pesquisar convidados, montar roteiros, escrever introduções, estruturar notas do episódio (show notes) e criar posts para redes sociais — todo esse trabalho operacional de produção drena o ritmo de criação. E se você pudesse apenas definir um tema e receber um pacote de produção completo pronto para publicação?

Este caso de uso conecta agentes em cadeia para gerenciar todo o fluxo de produção de um podcast, desde a pauta inicial até os materiais finais prontos para publicação.

## O Problema

Podcasters solo e equipes enxutas gastam mais tempo cuidando de tarefas de produção do que gravando de fato. A pesquisa prévia consome horas, as notas do episódio costumam ser improvisadas às pressas e a divulgação nas redes sociais acaba sendo esquecida. A parte criativa — a conversa — representa cerca de 30% do esforço total. Este agente assume os outros 70%.

## O Que Ele Faz

- **Pesquisa do Episódio** — a partir de um tema ou do nome do convidado, compila histórico, tópicos de discussão e sugestões de perguntas
- **Pauta e Roteiro** — elabora uma pauta estruturada com gancho de introdução, transições entre blocos e considerações de encerramento
- **Notas do Episódio (Show Notes)** — após a gravação, converte a transcrição em notas com marcações de tempo (timestamps) e links para tudo o que foi citado
- **Kit para Redes Sociais** — gera publicações promocionais para X, LinkedIn e Instagram com citações de impacto e momentos marcantes
- **Descrição do Episódio** — redige descrições otimizadas para SEO no Spotify, Apple Podcasts e YouTube

## Skills Necessárias

- Skill de busca na web / pesquisa (para pesquisar convidados e aprofundar temas)
- Acesso ao sistema de arquivos (para leitura de transcrições e gravação de arquivos gerados)
- Integração com Slack, Discord ou Telegram (para entrega dos materiais)
- Opcional: `sessions_spawn` para rodar agentes de pesquisa e redação em paralelo
- Opcional: skill de feed RSS (para monitorar podcasts concorrentes)

## Como Configurar

1. Pré-gravação — gerar pesquisa e roteiro:
```text
Vou gravar um episódio de podcast sobre [TÓPICO]. Meu convidado é [NOME].

Por favor:
1. Pesquise o convidado — trajetória, trabalhos recentes, pontos de vista marcantes
   e declarações relevantes ou controversas feitas em público.
2. Pesquise o tema — tendências principais, notícias recentes, equívocos comuns
   e o que o público provavelmente já sabe versus o que seria uma surpresa.
3. Gere a pauta do episódio:
   - Gancho de abertura (cold open) de 1 a 2 frases para prender a atenção
   - Roteiro de introdução (30 segundos, tom descontraído)
   - 5 a 7 perguntas para a entrevista, partindo de quebra-gelo/rapport até perguntas profundas/provocativas
   - 2 a 3 perguntas reserva caso a conversa perca o ritmo
   - Bloco de encerramento com chamada para ação (CTA)

Salve tudo em ~/podcast/episodes/[numero-do-episodio]/prep/
```

2. Pós-gravação — gerar notas do episódio e material promocional:
```text
Aqui está a transcrição do Episódio [NÚMERO]: [cole o texto ou aponte o arquivo]

Por favor:
1. Crie notas do episódio com marcação de tempo (timestamps) — cada mudança relevante
   de assunto deve ter um timestamp e um resumo de uma linha. Inclua links para tudo o que
   for citado (ferramentas, livros, artigos, pessoas).
2. Escreva uma descrição do episódio (máximo de 200 palavras) otimizada para busca nas plataformas.
   Inclua de 3 a 5 palavras-chave relevantes de forma natural.
3. Crie posts para redes sociais:
   - X/Twitter: 3 tweets — uma citação marcante, um aprendizado principal e uma pergunta
     para estimular debate. Cada um com menos de 280 caracteres.
   - LinkedIn: 1 post, tom profissional, de 100 a 150 palavras.
   - Instagram: 1 legenda com emojis, tom casual e hashtags pertinentes.
4. Destaque os 3 momentos mais surpreendentes ou interessantes acompanhados dos respectivos timestamps.

Salve tudo em ~/podcast/episodes/[numero-do-episodio]/publish/
```

3. Opcional — monitoramento de concorrência:
```text
Monitore diariamente estes feeds RSS de podcasts:
- [URL do feed 1]
- [URL do feed 2]

Quando sair um episódio novo abordando um tema relevante para o meu podcast,
envie um alerta no Telegram com:
- Título do episódio e link
- Resumo em uma frase
- Sugestão se vale a pena gravar uma resposta ou cobrir sob a minha ótica
```

## Principais Aprendizados

- A **pesquisa pré-gravação** concentra o maior valor. Entrar na gravação com pleno domínio da trajetória do convidado transforma a dinâmica da conversa — algo impossível de consertar na pós-produção.
- Notas do episódio com marcações de tempo aumentam muito a retenção e o engajamento dos ouvintes. A maioria dos podcasters pula essa etapa por dar trabalho; o agente torna o processo automático.
- O kit para redes sociais é o que mais economiza tempo *recorrente*. Todo episódio exige divulgação contínua com uma estrutura padronizada — cenário ideal para automação.
- Combina perfeitamente com a [Fábrica de Conteúdo Multi-Agente](content-factory.md) para desdobrar o conteúdo do podcast em artigos de blog, newsletters ou cortes de vídeo.

## Links Relacionados

- [Especificação de Feed RSS para Podcasts (Apple)](https://podcasters.apple.com/support/823-podcast-requirements)
- [Spotify for Podcasters](https://podcasters.spotify.com/)
- [Whisper (OpenAI)](https://github.com/openai/whisper) — para transcrição de áudio local
