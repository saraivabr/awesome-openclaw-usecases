# Agregador Multi-Fonte de Notícias de Tecnologia

Agregue, pontue e receba automaticamente notícias de tecnologia a partir de mais de 109 fontes entre RSS, Twitter/X, lançamentos no GitHub e buscas na web — tudo gerenciado por linguagem natural.

## O Problema

Manter-se atualizado sobre IA, código aberto e tecnologias de ponta exige conferir diariamente dezenas de feeds RSS, perfis no Twitter/X, repositórios no GitHub e sites de notícias. A curadoria manual consome muito tempo, e a maioria das ferramentas existentes peca na filtragem de qualidade ou exige configurações excessivamente complexas.

## O Que Ele Faz

Um pipeline de dados em quatro camadas executado de forma agendada:

1. **Feeds RSS** (46 fontes) — OpenAI, Hacker News, MIT Tech Review, etc.
2. **KOLs no Twitter/X** (44 contas influentes) — @karpathy, @sama, @VitalikButerin, etc.
3. **Releases no GitHub** (19 repositórios) — vLLM, LangChain, Ollama, Dify, etc.
4. **Busca na Web** (4 buscas temáticas) — via Brave Search API

Todos os artigos são consolidados, desduplicados por similaridade de título e pontuados por critérios de qualidade (fonte prioritária +3, múltiplas fontes +5, atualidade +2, engajamento +1). O resumo final é entregue no Discord, por e-mail ou no Telegram.

O framework é totalmente personalizável — adicione seus próprios feeds RSS, perfis do Twitter, repositórios do GitHub ou termos de busca em 30 segundos.

## Prompts

**Instalar e configurar o resumo diário:**
```text
Instale a skill tech-news-digest do ClawHub. Configure um resumo diário de tecnologia às 9h para o canal #tech-news do Discord. Envie também para o meu e-mail em myemail@example.com.
```

**Adicionar fontes personalizadas:**
```text
Adicione estas fontes ao meu resumo de tecnologia:
- RSS: https://my-company-blog.com/feed
- Twitter: @myFavResearcher
- GitHub: my-org/my-framework
```

**Gerar sob demanda:**
```text
Gere um resumo de tecnologia das últimas 24 horas e envie aqui.
```

## Skills Necessárias

- [tech-news-digest](https://clawhub.ai/skills/tech-news-digest) — Instale via `clawhub install tech-news-digest`
- [gog](https://clawhub.ai/skills/gog) (opcional) — Para envio de e-mails via Gmail

## Variáveis de Ambiente (Opcionais)

- `X_BEARER_TOKEN` — Token bearer da API do Twitter/X para monitoramento de KOLs
- `BRAVE_API_KEY` — Chave da API do Brave Search para a camada de busca na web
- `GITHUB_TOKEN` — Token do GitHub para limites de taxa (rate limits) mais altos na API

## Links Relacionados

- [Repositório no GitHub](https://github.com/draco-agent/tech-news-digest)
- [Página no ClawHub](https://clawhub.ai/skills/tech-news-digest)
