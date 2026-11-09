# Dashboard Dinâmico com Inicialização de Subagentes

Dashboards estáticos exibem dados desatualizados e exigem constantes atualizações manuais. Você quer visibilidade em tempo real em múltiplas fontes de dados sem precisar criar um frontend personalizado ou estourar os limites de requisição (rate limits) das APIs.

Este fluxo de trabalho cria um painel dinâmico que instancia subagentes para buscar e processar dados em paralelo:

• Monitora múltiplas fontes de dados simultaneamente (APIs, bancos de dados, GitHub, redes sociais)
• Instancia subagentes para cada fonte de dados para evitar bloqueios e distribuir a carga de requisições às APIs
• Agrega os resultados em um painel unificado (texto, HTML ou Canvas)
• Atualiza a cada N minutos com dados em tempo real
• Envia alertas quando as métricas ultrapassam limites definidos
• Mantém o histórico de tendências em um banco de dados para visualização

## O Problema

Construir um dashboard personalizado leva semanas. Quando fica pronto, os requisitos já mudaram. Fazer requisições sequenciais a várias APIs é lento e atinge limites de taxa. Você precisa de insights agora, não após um fim de semana inteiro programando.

## O Que Ele Faz

Você define o que deseja monitorar em linguagem natural: "Monitore as estrelas do GitHub, menções no Twitter, volume na Polymarket e a integridade do sistema." O OpenClaw instancia subagentes para consultar cada fonte de dados em paralelo, consolida os resultados e entrega um dashboard formatado no Discord ou como um arquivo HTML. As atualizações rodam automaticamente via agendamento cron.

Exemplo de seções do dashboard:
- **GitHub**: estrelas, forks, issues abertas, commits recentes
- **Redes Sociais**: menções no Twitter, discussões no Reddit, atividade no Discord
- **Mercados**: volume na Polymarket, tendências de previsões
- **Saúde do Sistema**: CPU, memória, uso de disco, status de serviços

## Skills Necessárias

- Inicialização de subagentes para execução paralela
- `github` (gh CLI) para métricas do GitHub
- `bird` (Twitter) para dados sociais
- `web_search` ou `web_fetch` para APIs externas
- `postgres` para armazenar histórico de métricas
- Discord ou Canvas para renderização do painel
- Tarefas cron para atualizações agendadas

## Como Configurar

1. Configure um banco de dados para as métricas:
```sql
CREATE TABLE metrics (
  id SERIAL PRIMARY KEY,
  source TEXT, -- ex.: "github", "twitter", "polymarket"
  metric_name TEXT,
  metric_value NUMERIC,
  timestamp TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE alerts (
  id SERIAL PRIMARY KEY,
  source TEXT,
  condition TEXT,
  threshold NUMERIC,
  last_triggered TIMESTAMPTZ
);
```

2. Crie um canal no Discord para as atualizações do painel (ex.: #dashboard).

3. Envie o prompt para o OpenClaw:
```text
Você é o meu gerenciador de dashboard dinâmico. A cada 15 minutos, execute uma tarefa cron para:

1. Instanciar subagentes em paralelo para buscar dados de:
   - GitHub: estrelas, forks, issues abertas, commits (últimas 24h)
   - Twitter: menções a "@username", análise de sentimento
   - Polymarket: volume dos mercados monitorados
   - Sistema: uso de CPU, memória e disco via comandos shell

2. Cada subagente grava os resultados no banco de dados de métricas.

3. Agregue todos os resultados e formate um painel:

📊 **Atualização do Painel** — [timestamp]

**GitHub**
- ⭐ Estrelas: [count] (+[change])
- 🍴 Forks: [count]
- 🐛 Issues Abertas: [count]
- 💻 Commits (24h): [count]

**Redes Sociais**
- 🐦 Menções no Twitter: [count]
- 📈 Sentimento: [positivo/negativo/neutro]

**Mercados**
- 📊 Volume Polymarket: $[amount]
- 🔥 Em Alta: [market names]

**Saúde do Sistema**
- 💻 CPU: [usage]%
- 🧠 Memória: [usage]%
- 💾 Disco: [usage]%

4. Publique no canal #dashboard do Discord.

5. Verifique as condições de alerta:
   - Se as estrelas do GitHub variarem > 50 em 1 hora → me notifique
   - Se o uso de CPU do sistema for > 90% → dispare um alerta
   - Se houver um pico de sentimento negativo no Twitter → avise-me

Armazene todas as métricas no banco de dados para análises históricas.
```

4. Opcional: Use o Canvas para renderizar um painel HTML com gráficos e visualizações.

5. Consulte os dados históricos: "Mostre o crescimento de estrelas no GitHub nos últimos 30 dias."

## Links Relacionados

- [Processamento Paralelo com Subagentes](https://docs.openclaw.ai/subagents)
- [Princípios de Design de Painéis e Dashboards](https://www.nngroup.com/articles/dashboard-design/)
