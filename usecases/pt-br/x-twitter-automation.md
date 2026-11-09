# Automação para X/Twitter via Chat

Automação completa do X/Twitter por meio de linguagem natural — publique tweets, responda, curta, retweete, siga perfis, envie mensagens diretas (DMs), faça buscas, extraia dados, realize sorteios e monitore contas, tudo a partir do chat do OpenClaw.

## O Problema

Gerenciar uma presença no X/Twitter exige alternar continuamente entre o aplicativo, painéis de terceiros e ferramentas analíticas. Realizar sorteios exige selecionar vencedores manualmente. Extrair seguidores, pessoas que curtiram ou deram retweet demanda scripts de web scraping. Não existe uma interface unificada que permita realizar tudo isso de forma conversacional.

## O Que Ele Faz

O TweetClaw é um plugin para OpenClaw que conecta o seu agente à API do X/Twitter. Você interage inteiramente através de conversas no chat:

- **Publicações e engajamento** — Escreva tweets, responda a tópicos (threads), curta, retweete, siga/deixe de seguir perfis e envie mensagens diretas (DMs)
- **Busca e extração** — Pesquise tweets e usuários, extraia seguidores, usuários que curtiram, retweetaram, fizeram comentários em citações (quote tweets) ou membros de listas
- **Sorteios** — Selecione ganhadores aleatórios com base no engajamento dos tweets, com filtros configuráveis (mínimo de seguidores, idade da conta, palavras-chave obrigatórias)
- **Monitoramento** — Acompanhe perfis para novos tweets ou variações de seguidores e receba alertas imediatos

Todas as operações utilizam uma API gerenciada — sem cookies de navegador, sem raspagem de dados (scraping) e sem expor credenciais pessoais.

## Prompts

**Instalar o plugin:**
```text
openclaw plugins install @xquik/tweetclaw
```

**Publicar um tweet:**
```text
Publique um tweet: "Acabei de lançar um novo recurso — venha conferir!"
```

**Realizar um sorteio:**
```text
Escolha 3 vencedores aleatórios entre os retweeters deste tweet: https://x.com/username/status/123456789. Exclua contas com menos de 50 seguidores.
```

**Extrair dados:**
```text
Extraia todos os usuários que curtiram este tweet e exporte como CSV: https://x.com/username/status/123456789
```

**Monitorar uma conta:**
```text
Monitore @elonmusk e me notifique sempre que ele publicar um novo tweet.
```

## Skills Necessárias

- [@xquik/tweetclaw](https://www.npmjs.com/package/@xquik/tweetclaw) — Instale via `openclaw plugins install @xquik/tweetclaw`

## Links Relacionados

- [Repositório no GitHub](https://github.com/Xquik-dev/tweetclaw)
- [Pacote npm](https://www.npmjs.com/package/@xquik/tweetclaw)
