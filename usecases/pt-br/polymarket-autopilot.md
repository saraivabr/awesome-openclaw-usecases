# Piloto Automático para Polymarket: Operações Simuladas Automatizadas

Monitorar manualmente mercados de predição em busca de oportunidades de arbitragem e executar operações consome muito tempo e exige atenção constante. Você quer testar e aprimorar estratégias de trading sem arriscar capital real.

Este fluxo automatiza operações simuladas (paper trading) no Polymarket com estratégias personalizadas:

• Monitora dados de mercado via API (preços, volume, spreads)  
• Executa trades simulados usando estratégias TAIL (seguidor de tendência) e BONDING (contrária)  
• Acompanha o desempenho da carteira, P&L (lucros e perdas) e taxa de acerto  
• Envia resumos diários para o Discord com registros de operações e insights  
• Aprende com padrões: ajusta parâmetros de estratégia com base nos resultados de backtesting  

## O Problema

Mercados de predição se movem rapidamente. Operar manualmente significa perder oportunidades, tomar decisões emocionais e ter dificuldade para acompanhar o que funciona. Testar estratégias com dinheiro real traz riscos de perdas antes mesmo de você compreender o comportamento do mercado.

## O Que Ele Faz

O piloto automático faz varreduras contínuas no Polymarket em busca de oportunidades, simula negociações usando estratégias configuráveis e registra tudo para análise. Você acorda com um resumo do que foi "negociado" durante a noite, o que funcionou e o que não deu certo.

Exemplos de estratégias:
- **TAIL**: Segue tendências quando há picos de volume e o momento está claro
- **BONDING**: Compra posições contrárias quando o mercado reage de forma exagerada a notícias
- **SPREAD**: Identifica mercados com preços incorretos que oferecem potencial de arbitragem

## Skills Necessárias

- `web_search` ou `web_fetch` (para dados da API do Polymarket)
- `postgres` ou SQLite para registros de negociações e acompanhamento de carteira
- Integração com Discord para relatórios diários
- Tarefas cron para monitoramento contínuo
- Criação de subagentes para análise paralela de mercados

## Como Configurar

1. Configure um banco de dados para as operações simuladas:
```sql
CREATE TABLE paper_trades (
  id SERIAL PRIMARY KEY,
  market_id TEXT,
  market_name TEXT,
  strategy TEXT,
  direction TEXT,
  entry_price DECIMAL,
  exit_price DECIMAL,
  quantity DECIMAL,
  pnl DECIMAL,
  timestamp TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE portfolio (
  id SERIAL PRIMARY KEY,
  total_value DECIMAL,
  cash DECIMAL,
  positions JSONB,
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

2. Crie um canal no Discord para atualizações (por exemplo, #polymarket-autopilot).

3. Envie este prompt para o OpenClaw:
```text
Você é um piloto automático de paper trading para o Polymarket. Execute continuamente (via cron a cada 15 minutos):

1. Busque dados de mercado atualizados da API do Polymarket
2. Analise oportunidades usando estas estratégias:
   - TAIL: Siga tendências fortes (>60% de probabilidade + pico de volume)
   - BONDING: Operações contrárias em reações exageradas (quedas repentinas >10% diante de notícias)
   - SPREAD: Arbitragem quando SIM+NÃO > 1.05
3. Execute negociações simuladas no banco de dados (capital inicial: $10.000)
4. Acompanhe o estado da carteira e atualize as posições

Todas as manhãs, às 8h, publique um resumo no Discord em #polymarket-autopilot:
- Negociações de ontem (preços de entrada/saída, P&L)
- Valor atual da carteira e posições abertas
- Taxa de acerto e desempenho das estratégias
- Insights de mercado e recomendações

Utilize subagentes para analisar múltiplos mercados em paralelo durante períodos de alto volume.

Nunca utilize dinheiro real. Este é apenas um ambiente de simulação (paper trading).
```

4. Faça iterações nas estratégias de acordo com o desempenho. Ajuste limites, adicione novas estratégias e faça backtests com dados históricos.

## Links Relacionados

- [Polymarket API](https://docs.polymarket.com/)
- [Paper Trading Best Practices](https://www.investopedia.com/articles/trading/11/paper-trading.asp)
