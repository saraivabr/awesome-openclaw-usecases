# Descoberta de Pesquisas com Hugging Face Papers

Acompanhar a pesquisa em Machine Learning (ML) significa atualizar a página do Hugging Face Papers todos os dias, ler dezenas de títulos, clicar em cada um para ler o resumo e cruzar manualmente com repositórios no GitHub. O que você precisa é de uma maneira conversacional de descobrir, priorizar e aprofundar a leitura de artigos em alta sem precisar sair do seu espaço de trabalho.

Este fluxo de trabalho combina duas skills para criar um pipeline completo de descoberta e pesquisa:

- Navegue pelos artigos em alta de hoje no Hugging Face — ordenados por votos da comunidade (upvotes) ou data
- Pesquise artigos por palavra-chave para encontrar trabalhos relevantes sobre qualquer assunto
- Obtenha metadados completos dos artigos: resumo (abstract), autores, repositórios no GitHub, votos da comunidade e resumos gerados por IA
- Leia discussões e comentários da comunidade sobre qualquer artigo
- Faça uma leitura profunda do código-fonte completo em LaTeX de qualquer artigo via ID do arXiv (usando arxiv-source)

## Skills Necessárias

- Skill [hf-papers](https://github.com/openclaw/skills/tree/main/skills/willamhou/hf-papers) (4 ferramentas: `hf_daily_papers`, `hf_search_papers`, `hf_paper_detail`, `hf_paper_comments`)
- Skill [arxiv-source](https://github.com/openclaw/skills/tree/main/skills/willamhou/arxiv-source) (3 ferramentas: `arxiv_fetch`, `arxiv_sections`, `arxiv_abstract`) — para o texto completo dos artigos

Não é necessário Docker ou autenticação — ambas as skills utilizam APIs públicas com cache local.

## Como Configurar

1. Instale ambas as skills:
```bash
clawhub install hf-papers
clawhub install arxiv-source
```

2. Dê o prompt ao OpenClaw com seu fluxo de pesquisa:
```text
Quero ficar por dentro das pesquisas em Machine Learning. Aqui está meu fluxo diário:

1. Todas as manhãs, mostre-me os 10 principais artigos em alta no Hugging Face (ordenados por votos)
   - Para cada artigo, exiba: título, votos, repositório no GitHub (se houver) e um resumo de 1 linha gerado por IA

2. Quando eu disser "pesquisar [tópico]":
   - Busque no HF Papers e mostre os resultados mais relevantes
   - Destaque os artigos com repositórios no GitHub vinculados ou com alto número de votos

3. Quando eu escolher um artigo (pelo ID):
   - Mostre o resumo completo (abstract), autores e recursos vinculados
   - Mostre os comentários da comunidade, se houver
   - Pergunte se quero fazer uma leitura profunda

4. Para leituras profundas:
   - Baixe o artigo completo via arxiv-source
   - Resuma as principais contribuições, metodologia e resultados
   - Destaque repositórios de código vinculados que eu deva conferir

Mantenha uma lista contínua dos artigos que revisei hoje com as conclusões em uma linha.
```

3. Experimente perguntar: "O que está em alta no Hugging Face Papers hoje?"
