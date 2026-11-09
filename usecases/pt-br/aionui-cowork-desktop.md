# OpenClaw como Desktop Cowork (AionUi) — Resgate Remoto e Hub Multiagente

Use o OpenClaw a partir de uma interface desktop de Cowork, acesse-o pelo Telegram ou WebUI quando estiver longe e repare-o remotamente quando ele não conectar. O AionUi é um aplicativo gratuito e de código aberto que executa o **OpenClaw como um agente de primeira classe** ao lado de mais de 12 outros (Claude Code, Codex, Qwen Code, etc.), com um **especialista integrado em implantação do OpenClaw** para instalar, diagnosticar e reparar — incluindo **resgate remoto** quando o OpenClaw estiver fora do ar e você não estiver em frente à máquina.

## Por Que OpenClaw + AionUi

| Se você deseja… | O AionUi oferece… |
|-----------------|-------------------|
| **Usar o OpenClaw com uma interface desktop de verdade** | Workspace de Cowork onde você vê o OpenClaw (e outros agentes) ler/escrever arquivos, executar comandos e navegar na web — e não apenas no terminal/chat. |
| **Reparar o OpenClaw quando ele falhar e você estiver remoto** | Abra o AionUi via **Telegram ou WebUI** de qualquer lugar → use o **especialista em implantação do OpenClaw** para executar `openclaw doctor`, corrigir configurações e reiniciar o gateway. Muitos usuários dependem disso. |
| **Um único lugar para OpenClaw + outros agentes** | OpenClaw, agente integrado, Claude Code, Codex, etc. em um único app; alterne entre eles ou execute em paralelo, com a mesma configuração de MCP para todos. |
| **Acesso remoto ao seu OpenClaw** | WebUI, Telegram, Lark, DingTalk — converse com a mesma instância do AionUi (e, portanto, com o OpenClaw) pelo celular ou outro dispositivo. |

## O Problema

Você já usa o OpenClaw pelo terminal (CLI) ou Telegram, mas:

- Quer **ver** o que o agente está fazendo (arquivos, terminal, web) em vez de deduzir pelos logs.
- Quando o **OpenClaw não conecta** e você não está na máquina, você não tem como rodar `openclaw doctor` ou corrigir configurações — você precisa de acesso remoto a algo capaz de reparar o OpenClaw.
- Você usa vários agentes CLI (OpenClaw, Claude Code, Codex, …) e não quer ficar alternando entre aplicativos ou reconfigurando o MCP para cada um.

## O Que Ele Faz

- **OpenClaw como um agente de Cowork**: Instale o AionUi e o OpenClaw; o AionUi detecta o OpenClaw automaticamente. Use o OpenClaw na mesma interface de Cowork — workspace contextualizado com arquivos e ações visíveis.
- **Resgate remoto do OpenClaw**: Quando o OpenClaw apresentar falhas ou estiver inacessível, abra o AionUi via **Telegram ou WebUI** e use o **especialista em implantação do OpenClaw** integrado. Ele auxilia na instalação, executa `openclaw doctor`, corrige configurações, reinicia o gateway e orienta você durante a recuperação. Um padrão comum para usuários que executam o OpenClaw em modo headless ou em outra máquina.
- **Múltiplos agentes em um só app**: Execute o OpenClaw ao lado do agente integrado (Gemini/OpenAI/Anthropic/Ollama), Claude Code, Codex e mais de 12 outros — interface única, sessões paralelas.
- **MCP configurado uma vez para todos os agentes**: Configure os servidores MCP no AionUi uma única vez; eles são sincronizados com o OpenClaw e com todos os outros agentes — sem necessidade de configurar o MCP individualmente.
- **Acesso remoto**: Use WebUI, Telegram, Lark ou DingTalk para acessar sua instância do AionUi (e o OpenClaw) de qualquer lugar.
- **Automação opcional**: O agendador cron do AionUi pode executar o OpenClaw (ou outros agentes) de forma programada para tarefas 24/7.

## Skills Necessárias

- **OpenClaw** (ex.: `npm install -g openclaw@latest`). O assistente de configuração **OpenClaw Setup** do AionUi pode guiar a instalação, gateway e configuração.
- Chaves de API ou autenticação para seus modelos (configuração do OpenClaw + quaisquer chaves do agente integrado no AionUi).

## Como Configurar

1. **Instale o AionUi**: [AionUi Releases](https://github.com/iOfficeAI/AionUi/releases) (macOS / Windows / Linux).
2. **Instale o OpenClaw** (se necessário):
   ```bash
   npm install -g openclaw@latest
   openclaw onboard --install-daemon   # opcional: daemon para operação 24/7
   ```
3. **Abra o AionUi**: Ele detecta o OpenClaw automaticamente. Se não detectar, use o assistente integrado **OpenClaw Setup**.
4. **Crie uma sessão de Cowork** e selecione o OpenClaw. Mesmo workspace, MCP e (se habilitado) canais remotos.

Para acesso remoto ou agendamentos cron, configure os canais e automações nas configurações do AionUi.

## Links Relacionados

- [AionUi GitHub](https://github.com/iOfficeAI/AionUi)
- [AionUi Website](https://www.aionui.com)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [OpenClaw Docs](https://docs.openclaw.ai)
