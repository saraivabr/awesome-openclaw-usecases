# Atendimento ao Cliente Multicanal com IA

Pequenas empresas precisam se desdobrar entre WhatsApp, DMs do Instagram, e-mails e avaliações no Google Reviews em múltiplos aplicativos. Os clientes esperam respostas imediatas 24 horas por dia, 7 dias por semana, mas contratar pessoal para cobertura ininterrupta é caro.

Este caso de uso consolida todos os pontos de contato com o cliente em uma única caixa de entrada potencializada por IA que responde com inteligência em seu nome.

## O Que Ele Faz

- **Caixa de entrada unificada**: WhatsApp Business, DMs do Instagram, Gmail e Google Reviews em um só lugar
- **Respostas automáticas por IA**: Atende dúvidas frequentes (FAQs), solicitações de agendamento e perguntas comuns automaticamente
- **Transbordo humano (Human handoff)**: Encaminha problemas complexos para atendimento humano ou sinaliza para revisão
- **Modo de teste**: Apresente demonstrações do sistema para clientes sem impactar clientes reais
- **Contexto de negócio**: Treinado sobre seus serviços, preços e políticas

## Exemplo Real de Negócio

Na Futurist Systems, implementamos isso para empresas de serviços locais (restaurantes, clínicas, salões de beleza). Um restaurante reduziu o tempo de resposta de mais de 4 horas para menos de 2 minutos, atendendo 80% das mensagens de forma 100% automática.

## Skills Necessárias

- Integração com a WhatsApp Business API
- Instagram Graph API (via Meta Business)
- `gog` CLI para Gmail
- Google Business Profile API para avaliações
- Lógica de roteamento de mensagens no AGENTS.md

## Como Configurar

1. **Conecte os canais** pela configuração do OpenClaw:
   - WhatsApp Business API (via 360dialog ou API oficial)
   - Instagram via Meta Business Suite
   - Gmail via OAuth do `gog`
   - Token da Google Business Profile API

2. **Crie a base de conhecimento do negócio**:
   - Serviços e preços
   - Horários de funcionamento e localização
   - Respostas para perguntas frequentes (FAQs)
   - Gatilhos de escalonamento humano (ex.: reclamações, solicitações de reembolso)

3. **Configure o AGENTS.md** com a lógica de roteamento:

```text
## Modo de Atendimento ao Cliente

Ao receber mensagens de clientes:

1. Identifique o canal (WhatsApp/Instagram/E-mail/Avaliação)
2. Verifique se o modo de teste está ativado para este cliente
3. Classifique a intenção:
   - FAQ → responder a partir da base de conhecimento
   - Agendamento → checar disponibilidade, confirmar reserva
   - Reclamação → marcar para revisão humana, acusar recebimento
   - Avaliação → agradecer pelo feedback, tratar preocupações levantadas

Estilo de resposta:
- Amigável, profissional e conciso
- Adaptar ao idioma do cliente (PT/ES/EN/UA)
- Nunca inventar informações que não estejam na base de conhecimento
- Assinar com o nome da empresa

Modo de teste:
- Prefixar respostas com [TESTE]
- Registrar em log sem enviar para canais reais
```

4. **Configure verificações periódicas (heartbeat)** para monitoramento de respostas:

```text
## Heartbeat: Verificação de Atendimento ao Cliente

A cada 30 minutos:
- Verificar mensagens não respondidas há mais de 5 minutos
- Alertar se a fila de respostas estiver acumulando
- Registrar métricas diárias de atendimento
```

## Principais Aprendizados

- **Detecção de idioma é fundamental**: Detecte e responda automaticamente na língua em que o cliente se comunica.
- **Modo de teste é indispensável**: Clientes precisam ver o funcionamento prático antes de ativar em produção.
- **Regras claras de transbordo**: Defina critérios objetivos de escalonamento para evitar respostas inadequadas da IA.
- **Modelos de resposta**: Mantenha padrões pré-aprovados para assuntos sensíveis (reembolsos, queixas).

## Links Relacionados

- [WhatsApp Business API](https://developers.facebook.com/docs/whatsapp)
- [Instagram Messaging API](https://developers.facebook.com/docs/instagram-api/guides/messaging)
- [Google Business Profile API](https://developers.google.com/my-business)
