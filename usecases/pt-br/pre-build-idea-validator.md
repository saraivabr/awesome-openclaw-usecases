# Validador de Ideias Pré-Desenvolvimento

Antes de o OpenClaw começar a programar algo novo, ele verifica automaticamente se a ideia já existe no GitHub, Hacker News, npm, PyPI e Product Hunt — ajustando sua abordagem com base no que encontrar.

## O Que Ele Faz

- Faz uma varredura em 5 fontes reais de dados (GitHub, Hacker News, npm, PyPI, Product Hunt) antes de escrever qualquer linha de código
- Retorna uma pontuação de `reality_signal` (0-100) indicando quão saturado está o mercado
- Exibe os principais concorrentes com contagem de estrelas e descrições
- Sugere direções de pivot quando o nicho está concorrido
- Funciona como um filtro pré-desenvolvimento: sinal alto = pausar e discutir, sinal baixo = prosseguir com a criação

## O Problema

Você diz ao seu agente "crie para mim uma ferramenta de code review com IA" e ele passa 6 horas programando alegremente. Enquanto isso, mais de 143.000 repositórios já existem no GitHub — o mais popular com 53.000 estrelas. O agente nunca verifica porque você não pediu, e ele não sabe que deveria buscar. Você só descobre os concorrentes depois de investir muito tempo. Esse padrão se repete a cada nova ideia de projeto.

## Skills Necessárias

- [idea-reality-mcp](https://github.com/mnemox-ai/idea-reality-mcp) — Servidor MCP que varre fontes de dados reais e retorna uma pontuação de concorrência

## Como Configurar

1. Instale o idea-reality-mcp:

```bash
uvx idea-reality-mcp
```

2. Adicione o servidor MCP à sua configuração do OpenClaw:

```json
{
  "mcpServers": {
    "idea-reality": {
      "command": "uvx",
      "args": ["idea-reality-mcp"]
    }
  }
}
```

3. Adicione isto às instruções do seu agente OpenClaw:

```text
Antes de iniciar qualquer novo projeto, funcionalidade ou ferramenta, execute sempre o idea_check primeiro.

Regras:
- Se reality_signal > 70: PARE. Informe os 3 principais concorrentes com contagem de estrelas.
  Pergunte-me se desejo prosseguir, pivotar ou abandonar a ideia.
- Se reality_signal entre 30 e 70: Mostre-me os resultados e os pivot_hints.
  Sugira um ângulo de nicho que os projetos existentes ainda não cobrem.
- Se reality_signal < 30: Prossiga com o desenvolvimento.
  Mencione que o espaço está aberto.
- Sempre mostre a pontuação de reality_signal e os principais concorrentes antes de escrever qualquer código.
```

## Exemplo Real

**Você:** "Crie uma ferramenta de CLI para code review com IA"

O OpenClaw executa `idea_check("AI code review tool")` e responde:

> **reality_signal: 90/100 (muito alto)**
>
> Principais concorrentes:
> 1. Gitea — 53.940 estrelas
> 2. reviewdog — 9.104 estrelas
> 3. Danger (Ruby) — 5.649 estrelas
>
> Este espaço tem mais de 143.000 repositórios relacionados. Os principais participantes são consolidados e contam com financiamento significativo.
>
> **Sugestões de pivot:**
> - Focar em uma linguagem específica (code review por IA exclusivo para Rust/Go)
> - Ter como alvo um framework específico (revisão de componentes React/Vue)
> - Mirar em um setor específico (conformidade de código para área financeira ou médica)
>
> Devo prosseguir com um ângulo diferenciado ou prefere pivotar para outra coisa?

**Você:** "E que tal um servidor MCP para validação de ideias antes do desenvolvimento?"

> **reality_signal: 8/100 (muito baixo)**
>
> 0 concorrentes diretos encontrados no PyPI ou npm. O GitHub possui alguns repositórios ligeiramente relacionados, mas nenhum resolve exatamente este problema.
>
> O caminho está totalmente livre. Prosseguindo com o desenvolvimento.

## Variações

- **Modo profundo para decisões importantes**: Use `depth="deep"` para escanear todas as 5 fontes em paralelo (GitHub + HN + npm + PyPI + Product Hunt) para decisões estratégicas de grandes projetos.
- **Validação em lote**: Antes de um hackathon, passe ao OpenClaw uma lista com 10 ideias e peça para classificá-las por `reality_signal` — a menor pontuação representa a oportunidade mais original.
- **Demonstração web primeiro**: Experimente sem instalar em [mnemox.ai/check](https://mnemox.ai/check) para verificar se o fluxo atende às suas necessidades.

## Principais Insights

- Isso evita o erro mais caro no desenvolvimento de software: **resolver um problema que já foi resolvido**.
- O `reality_signal` é baseado em dados reais (contagem de repositórios, distribuição de estrelas, volume de discussões no HN), e não em suposições do LLM.
- Uma pontuação alta não significa "não construa" — significa "diferencie-se ou não perca seu tempo".
- Uma pontuação baixa indica a existência de um verdadeiro espaço em branco. É aí que criadores solo têm as maiores chances de sucesso.

## Links Relacionados

- [GitHub do idea-reality-mcp](https://github.com/mnemox-ai/idea-reality-mcp)
- [Demonstração Web](https://mnemox.ai/check) (teste sem instalar)
- [PyPI](https://pypi.org/project/idea-reality-mcp/)
