# Resumo Diário do YouTube

Comece o seu dia com um resumo personalizado dos novos vídeos dos seus canais favoritos do YouTube — sem perder conteúdos de criadores que você realmente deseja acompanhar.

## O Problema

As notificações do YouTube não são confiáveis. Você se inscreve nos canais, mas os novos vídeos nunca aparecem no seu feed inicial. Não estão nas notificações. Eles simplesmente... desaparecem. Isso não significa que você não quer vê-los — significa que o algoritmo do YouTube os enterrou.

Além disso: é muito mais produtivo começar o dia com insights curados do que rolando infinitamente um feed de recomendações.

## O Que Ele Faz

- Busca os vídeos mais recentes de uma lista dos seus canais favoritos
- Resume ou extrai os principais insights a partir da transcrição de cada vídeo
- Entrega um resumo diariamente (ou sob demanda)

## Skills Necessárias

Instale a skill [youtube-full](https://clawhub.ai/therohitdas/youtube-full).

Basta dizer ao seu OpenClaw:

```text
"Instale a skill youtube-full e configure-a para mim"
```
ou 

```bash
npx clawhub@latest install youtube-full
```

É só isso. O agente cuida do resto — incluindo criação de conta e configuração da chave de API. Você ganha **100 créditos gratuitos ao se cadastrar**, sem necessidade de cartão de crédito.

> Nota: Após criar a conta, a skill armazena a chave de API de forma segura nos locais corretos de acordo com o sistema operacional, para que a API funcione em todos os contextos.

![Instalação da skill youtube-full](https://pub-15904f15a44a4ea69350737e87660b92.r2.dev/media/1770620159490-e41e7baa.png)

### Por que TranscriptAPI.com em vez de yt-dlp?

| Ferramentas CLI (yt-dlp, etc.) | TranscriptAPI |
|--------------------------------|---------------|
| Logs detalhados poluem o contexto do agente | Respostas limpas em JSON |
| Não funciona no GCP / OpenClaw em nuvem | Funciona em qualquer lugar, rápido |
| É bloqueado aleatoriamente pelo YouTube | Alimenta o [YouTubeToTranscript.com](https://youtubetotranscript.com), atendendo milhões. Cacheado e confiável. |
| Requer instalação de binários | Sem binários, apenas HTTP |

## Como Configurar

### Opção 1: Resumo baseado em canais

Envie o prompt para o OpenClaw:

```text
Todas as manhãs às 8h, busque os vídeos mais recentes destes canais do YouTube e me dê um resumo com os principais insights de cada um:

- @TED
- @Fireship
- @ThePrimeTimeagen
- @lexfridman

Para cada novo vídeo (enviado nas últimas 24 a 48 horas):
1. Obtenha a transcrição
2. Resuma os pontos principais em 2 a 3 tópicos
3. Inclua o título do vídeo, nome do canal e link

Se o handle de algum canal não for encontrado, pesquise por ele e localize o correto.
Salve minha lista de canais na memória para que eu possa adicionar/remover canais mais tarde.
```

### Opção 2: Resumo baseado em palavras-chave

Monitore novos vídeos sobre um tópico específico:

```text
Todos os dias, pesquise no YouTube novos vídeos sobre "OpenClaw" (ou "Claude Code", "agentes de IA", etc.).

Mantenha um arquivo chamado seen-videos.txt com os IDs dos vídeos já processados.
Busque as transcrições apenas dos vídeos que NÃO estejam nesse arquivo.
Após o processamento, adicione o ID do vídeo ao seen-videos.txt.

Para cada novo vídeo:
1. Obtenha a transcrição
2. Forneça um resumo em 3 tópicos
3. Destaque qualquer coisa relevante para o meu trabalho

Execute isso todas as manhãs às 9h.
```

Dessa forma, você nunca desperdiça créditos buscando novamente vídeos que já viu.

## Dicas

- `channel/latest` e `channel/resolve` são **gratuitos** (0 créditos) — verificar novos uploads não custa nada
- Apenas as transcrições custam 1 crédito cada
- Peça estilos de resumo diferentes: principais aprendizados, citações notáveis, marcações de tempo de momentos interessantes
- Isso já existe como produto — [Recapio - Daily YouTube Recap](https://recapio.com/features/daily-recaps)
