# Edição de Vídeo com IA via Chat

Editar vídeos geralmente significa abrir um editor de linha do tempo, arrastar clipes de um lado para o outro e navegar por vários menus. Para edições repetitivas — como cortar introduções, adicionar legendas a um lote de clipes ou ajustar cores em 10 vídeos —, esse fluxo manual consome horas.

Este caso de uso transforma a edição de vídeo em uma conversa. Descreva o que você quer alterar, envie o arquivo e receba o resultado de volta. Sem linha do tempo, sem interface gráfica (GUI).

## O Que Você Pode Fazer

- Cortar, aparar e juntar clipes descrevendo marcações de tempo em linguagem natural
- Adicionar música de fundo com ducking automático de áudio (redução do volume durante a fala)
- Gerar e embutir legendas a partir da fala (mais de 50 idiomas)
- Fazer gradação de cores (color grading) nas filmagens ("deixe mais quente", "combine com o visual do primeiro clipe")
- Cortar para o formato vertical voltado para TikTok/Reels/Shorts
- Processar múltiplos arquivos em lote com a mesma edição

## Skills Necessárias

- [video-editor-ai](https://clawhub.ai/skills/video-editor-ai) — edição de vídeo baseada em chat com música de fundo, legendas e exportação
- [ai-subtitle-generator](https://clawhub.ai/skills/ai-subtitle-generator) — legendas automáticas, embutimento de legendas e exportação em SRT

## Como Configurar

1. Instale as skills:
```bash
clawhub install video-editor-ai
clawhub install ai-subtitle-generator
```

2. Envie um arquivo de vídeo no chat e descreva a sua edição:
```text
Corte este vídeo de 0:15 a 1:30, adicione música de fundo (algo animado)
e grave as legendas em inglês.
```

3. Para processamento em lote, descreva o padrão:
```text
Tenho 5 clipes em /videos/raw/. Para cada um:
- Recorte para o formato vertical 9:16
- Adicione legendas geradas automaticamente na parte inferior
- Exporte como mp4
```

O agente lida com as chamadas de API, monitora a conclusão do processamento e entrega os arquivos finalizados de volta no chat.

## Dicas

- Seja específico quanto aos tempos e ao formato de saída ("exporte como mp4 em 1080p")
- Para legendagem, mencione o idioma original caso não seja inglês
- A gradação de cor funciona melhor com descrições de referência ("tons quentes de pôr do sol") do que com valores técnicos
- Evite enviar filmagens sensíveis (rostos, documentos, telas confidenciais) a menos que tenha revisado as políticas de retenção de dados e privacidade do provedor
