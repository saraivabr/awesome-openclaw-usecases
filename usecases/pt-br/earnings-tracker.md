# Rastreador de Balanços Financeiros com IA

Acompanhar a temporada de divulgação de resultados (earnings season) em dezenas de empresas de tecnologia significa verificar múltiplas fontes e lembrar datas de divulgação de relatórios. Você quer ficar por dentro dos resultados do setor de IA e tecnologia sem precisar rastrear manualmente cada empresa.

Este fluxo de trabalho automatiza o acompanhamento e a entrega dos resultados financeiros:

• Prévia semanal aos domingos: faz a varredura do calendário de balanços da semana seguinte e publica as empresas de tecnologia/IA relevantes no Telegram
• Você escolhe quais empresas quer acompanhar, e o OpenClaw agenda tarefas cron pontuais (one-shot) para a data e o horário de cada balanço
• Assim que o relatório é divulgado, o OpenClaw pesquisa os resultados, formata um resumo detalhado (se superou ou ficou abaixo das projeções [beat/miss], métricas principais, destaques em IA) e faz o envio

## Skills Necessárias

- `web_search` (integrada)
- Suporte a agendamentos cron no OpenClaw
- Tópico no Telegram para atualizações de resultados

## Como Configurar

1. Crie um tópico no Telegram chamado "earnings" para atualizações.
2. Envie o prompt para o OpenClaw:
```text
Todo domingo às 18h, execute uma tarefa cron para:
1. Buscar o calendário de balanços da próxima semana para empresas de tecnologia e IA
2. Filtrar as empresas que acompanho (NVDA, MSFT, GOOGL, META, AMZN, TSLA, AMD, etc.)
3. Publicar a lista no meu tópico "earnings" do Telegram
4. Aguardar minha confirmação sobre quais delas desejo acompanhar

Quando eu responder indicando as empresas a monitorar:
1. Agende tarefas cron pontuais (one-shot) para cada data/hora de divulgação
2. Assim que cada relatório for divulgado, pesquise pelos resultados
3. Formate um resumo contendo: superou/abaixo das expectativas (beat/miss), receita, LPA (lucro por ação / EPS), métricas-chave, destaques relacionados a IA e projeções futuras (guidance)
4. Publique no tópico "earnings" do Telegram

Mantenha uma memória das empresas que costumo acompanhar para sugeri-las automaticamente a cada semana.
```
