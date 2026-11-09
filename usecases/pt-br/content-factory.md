# Fábrica de Conteúdo Multiagente

Você é um criador de conteúdo equilibrando pesquisa, redação e design em várias plataformas. Cada etapa — encontrar tópicos em alta, escrever roteiros, gerar miniaturas (thumbnails) — consome horas do seu dia. E se uma equipe de agentes especializados cuidasse de tudo isso durante a noite?

Este fluxo de trabalho estrutura uma fábrica de conteúdo multiagente dentro do Discord, onde diferentes agentes cuidam da pesquisa, redação e recursos visuais em canais dedicados.

## O Que Ele Faz

- **Agente de Pesquisa**: analisa notícias em alta, conteúdo de concorrentes e redes sociais em busca das melhores oportunidades de conteúdo todas as manhãs
- **Agente de Redação**: pega as melhores ideias e escreve roteiros completos, threads ou rascunhos de newsletters
- **Agente de Miniaturas/Thumbnails**: gera miniaturas com IA ou imagens de capa para o conteúdo
- Cada agente trabalha em seu próprio canal no Discord, mantendo tudo organizado e fácil de revisar
- Executa automaticamente de forma agendada (ex.: diariamente às 8h), para que você acorde com o conteúdo pronto

## O Problema

A criação de conteúdo tem três fases — pesquisa, redação e design — e a maioria dos criadores faz as três manualmente. Mesmo usando ferramentas de escrita com IA, você ainda precisa interagir com cada uma delas individualmente. Este sistema conecta agentes em um pipeline contínuo, onde a saída de um alimenta a entrada do próximo, sem necessidade de intervenção manual.

## Skills Necessárias

- Integração com o Discord com suporte a múltiplos canais
- `sessions_spawn` / `sessions_send` para orquestração multiagente
- [x-research-v2](https://clawhub.ai) ou similar para pesquisa em redes sociais
- Geração local de imagens (ex.: Nano Banana) ou uma API de geração de imagens
- Skill [knowledge-base](https://clawhub.ai) (opcional, para pesquisa potencializada com RAG)

## Como Configurar

1. Configure um servidor no Discord (ou peça ao OpenClaw para fazer isso por você — basta dizer "Configure um Discord para nós").

2. Crie canais para cada agente:
   - `#research` — tópicos em alta e oportunidades de conteúdo
   - `#scripts` — rascunhos de textos e roteiros
   - `#thumbnails` — imagens geradas e capas

3. Envie o prompt para o OpenClaw:
```text
Quero que você construa uma fábrica de conteúdo dentro do Discord.
Configure canais para diferentes agentes:

1. Agente de Pesquisa (#research): Todas as manhãs às 8h, pesquise as principais notícias
   em alta, conteúdo de concorrentes e o que está performando bem nas redes sociais
   no meu nicho. Publique as 5 melhores oportunidades de conteúdo com as fontes.

2. Agente de Redação (#scripts): Pegue a melhor ideia do agente de pesquisa
   e escreva um roteiro completo, thread ou rascunho de newsletter. Publique no canal #scripts.

3. Agente de Miniaturas (#thumbnails): Gere miniaturas com IA ou imagens de capa
   para o conteúdo. Publique no canal #thumbnails.

Mantenha todo o trabalho deles organizado em canais separados.
Execute esse pipeline automaticamente todas as manhãs.
```

4. Personalize para a sua plataforma:
```text
Meu foco são threads no X/Twitter, não vídeos no YouTube. Altere o agente de redação
para produzir sequências de tweets (threads) em vez de roteiros de vídeo.
```

## Principais Insights

- O verdadeiro poder está no **encadeamento de agentes** — pesquisa alimenta a redação, redação alimenta as miniaturas. Você não precisa instruir cada etapa manualmente.
- Os canais do Discord facilitam revisar o trabalho de cada agente separadamente e fornecer feedbacks como "os roteiros estão muito longos" ou "foque mais em notícias de IA".
- Você pode adaptar isso para qualquer formato de conteúdo: tweets, newsletters, posts no LinkedIn, pautas de podcast ou artigos de blog.
- Rodar um modelo local para geração de imagens (como Nano Banana em um Mac Studio) reduz custos e oferece mais controle.

## Baseado Em

Inspirado no [vídeo de Alex Finn sobre casos de uso transformadores do OpenClaw](https://www.youtube.com/watch?v=41_TNGDDnfQ).

## Links Relacionados

- [Documentação de Subagentes do OpenClaw](https://github.com/openclaw/openclaw)
- [Configuração de Bot do Discord](https://discord.com/developers/docs)
