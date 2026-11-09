# Leitor de Artigos do arXiv

Ler artigos do arXiv geralmente envolve baixar PDFs, perder o contexto ao alternar entre múltiplos artigos e lutar para interpretar a densa notação em LaTeX. Você quer ler, analisar e comparar artigos de forma conversacional, sem precisar sair do seu workspace.

Este fluxo de trabalho transforma o seu agente em um assistente de leitura e pesquisa científica:

- Busque qualquer artigo do arXiv pelo ID e receba um texto limpo e legível (o LaTeX é processado e simplificado automaticamente)
- Navegue primeiro pela estrutura do artigo — liste as seções para decidir o que ler antes de carregar o texto completo
- Faça uma triagem rápida de resumos em vários artigos para filtrar uma lista de leitura
- Peça ao agente para resumir, comparar ou criticar seções específicas
- Os resultados ficam em cache local — revisitar um artigo já lido é instantâneo

## Skills Necessárias

- Skill [arxiv-reader](https://github.com/Prismer-AI/Prismer/tree/main/skills/arxiv-reader) (3 ferramentas: `arxiv_fetch`, `arxiv_sections`, `arxiv_abstract`)

Não requer Docker nem Python — a skill roda de forma independente usando recursos nativos do Node.js. Ela faz o download direto do arXiv, descompacta o código-fonte em LaTeX e processa as inclusões automaticamente.

## Como Configurar

1. Instale a skill `arxiv-reader` a partir do [repositório Prismer](https://github.com/Prismer-AI/Prismer/tree/main/skills/arxiv-reader) — copie o diretório `skills/arxiv-reader/` para a pasta de skills do seu OpenClaw.

2. A skill está pronta para uso. Envie o prompt para o OpenClaw:
```text
Estou pesquisando sobre [tópico]. Este é o meu fluxo de trabalho:

1. Quando eu fornecer um ID do arXiv (como 2301.00001):
   - Primeiro busque o resumo (abstract) para que eu possa decidir se é relevante
   - Se eu disser "leia", busque o artigo completo (remova os apêndices por padrão)
   - Resuma as principais contribuições, metodologia e resultados

2. Quando eu fornecer múltiplos IDs:
   - Busque todos os resumos e monte uma tabela comparativa
   - Classifique-os por relevância para o meu tópico de pesquisa

3. Quando eu perguntar sobre uma seção específica:
   - Liste as seções do artigo primeiro
   - Em seguida, busque e explique a seção relevante em detalhes

Mantenha uma lista atualizada dos artigos que já li e suas principais conclusões.
```

3. Experimente: "Leia 2401.04088 — qual é a principal contribuição?"
