# Redação de Artigos Acadêmicos em LaTeX

Configurar um ambiente local de LaTeX costuma ser demorado e desgastante — instalar o TeX Live consome gigabytes, depurar erros de compilação é tedioso e alternar entre o editor e o visualizador de PDF quebra o fluxo de trabalho. O que você precisa é escrever e compilar artigos em LaTeX de maneira conversacional, sem nenhuma configuração local.

Este fluxo de trabalho transforma seu agente em um assistente de redação em LaTeX com compilação instantânea:

- Escreva em LaTeX de forma colaborativa com o agente — descreva o que deseja e ele gera o código-fonte
- Compile para PDF instantaneamente com pdflatex, xelatex ou lualatex (sem necessidade de instalação local do TeX)
- Visualize PDFs diretamente no fluxo sem alternar para outro aplicativo
- Use modelos iniciais prontos (article, IEEE, beamer, artigo em chinês) para pular a parte burocrática de configuração
- Suporte a bibliografia com BibTeX/BibLaTeX — basta colar o conteúdo do seu .bib

## Skills Necessárias

- Skill [latex-compiler](https://github.com/Prismer-AI/Prismer/tree/main/skills/latex-compiler) (4 ferramentas: `latex_compile`, `latex_preview`, `latex_templates`, `latex_get_template`)
- Contêiner de workspace Prismer (executa o servidor LaTeX na porta 8080 com a distribuição completa do TeX Live)

## Como Configurar

1. Clone e faça o deploy do [Prismer](https://github.com/Prismer-AI/Prismer) com Docker (o servidor LaTeX com TeX Live completo inicia automaticamente):
```bash
git clone https://github.com/Prismer-AI/Prismer.git && cd Prismer
docker compose -f docker/docker-compose.dev.yml up
```

2. A skill `latex-compiler` já é embutida — nenhuma instalação adicional é necessária. Dê o prompt ao OpenClaw:
```text
Me ajude a escrever um artigo científico em LaTeX. Aqui está meu fluxo de trabalho:

1. Comece a partir do modelo IEEE (ou article/beamer, dependendo do que eu precisar)
2. Quando eu descrever uma seção, gere o código-fonte em LaTeX para ela
3. Após cada edição importante, compile e gere a pré-visualização do PDF para que eu possa verificar a formatação
4. Se houver erros de compilação, leia o log e corrija-os automaticamente
5. Quando eu fornecer entradas BibTeX, adicione-as à bibliografia e recompile

Use xelatex se eu precisar de suporte a chinês/CJK, caso contrário, use pdflatex como padrão.
Sempre execute 2 passos de compilação (passes) para referências cruzadas.
```

3. Experimente: "Inicie um novo artigo IEEE intitulado 'A Survey of LLM Agents'. Me forneça o modelo com as seções de resumo e introdução preenchidas, e depois compile."
