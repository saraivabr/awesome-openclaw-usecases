# Busca Semântica em Memória

O sistema de memória nativo do OpenClaw armazena tudo em arquivos markdown — mas, conforme as memórias se acumulam ao longo de semanas e meses, encontrar aquela decisão específica da última terça-feira se torna uma tarefa quase impossível. Não há um mecanismo de busca integrado, apenas a rolagem manual por arquivos.

Este caso de uso adiciona **busca semântica orientada a vetores** sobre os arquivos de memória markdown existentes do OpenClaw usando o [memsearch](https://github.com/zilliztech/memsearch), permitindo encontrar instantaneamente qualquer memória passada pelo seu significado, e não apenas por palavras-chave.

## O Que Ele Faz

- Indexa todos os seus arquivos de memória markdown do OpenClaw em um banco de dados vetorial (Milvus) com um único comando
- Busca por significado: "qual solução de cache nós escolhemos?" localiza a anotação relevante mesmo que a palavra "cache" não apareça explicitamente
- Busca híbrida (vetores densos + texto completo BM25) com reclassificação RRF (Reciprocal Rank Fusion) para obter os melhores resultados
- Hashing de conteúdo com SHA-256 garante que arquivos inalterados nunca sejam reincorporados — zero chamadas de API desperdiçadas
- Monitor de arquivos (file watcher) que reindexa automaticamente sempre que os arquivos de memória mudam, mantendo o índice sempre atualizado
- Funciona com qualquer provedor de embeddings: OpenAI, Google, Voyage, Ollama ou totalmente local (sem necessidade de chave de API)

## O Problema

A memória do OpenClaw é armazenada em arquivos markdown simples. Isso é excelente para portabilidade e legibilidade humana, mas carece de uma busca eficiente. À medida que o histórico de memórias cresce, você é forçado a usar o grep nos arquivos (apenas palavras-chave, perdendo correspondências semânticas) ou carregar arquivos inteiros no contexto (desperdiçando tokens com conteúdo irrelevante). Você precisa de uma forma de perguntar "o que eu decidi sobre X?" e obter o trecho exato relevante, independentemente da formulação da frase.

## Skills Necessárias

- Nenhuma skill do OpenClaw é necessária — o memsearch é uma CLI e biblioteca Python independente
- Python 3.10+ com pip ou uv

## Como Configurar

1. Instale o memsearch:
```bash
pip install memsearch
```

2. Execute o assistente de configuração interativo:
```bash
memsearch config init
```

3. Indexe o diretório de memória do OpenClaw:
```bash
memsearch index ~/caminho/para/sua/memoria/
```

4. Busque nas suas memórias pelo significado:
```bash
memsearch search "qual solucao de cache escolhemos?"
```

5. Para sincronização em tempo real, inicie o monitor de arquivos — ele indexa automaticamente a cada alteração de arquivo:
```bash
memsearch watch ~/caminho/para/sua/memoria/
```

6. Para uma configuração 100% local (sem chaves de API), instale o provedor local de embeddings:
```bash
pip install "memsearch[local]"
memsearch config set embedding.provider local
memsearch index ~/caminho/para/sua/memoria/
```

## Principais Insights

- **O Markdown continua sendo a fonte da verdade.** O índice vetorial é apenas um cache derivado — você pode reconstruí-lo a qualquer momento com `memsearch index`. Seus arquivos de memória originais nunca são alterados.
- **Deduplicação inteligente economiza custos.** Cada trecho é identificado por um hash de conteúdo SHA-256. Executar novamente o comando `index` só gera embeddings para conteúdos novos ou modificados, permitindo executá-lo sempre que quiser sem custos adicionais de API.
- **A busca híbrida supera a busca exclusivamente vetorial.** A combinação de similaridade semântica (vetores densos) com correspondência por palavras-chave (BM25) via Reciprocal Rank Fusion atende tanto a consultas conceituais quanto a buscas por termos exatos.

## Links Relacionados

- [GitHub do memsearch](https://github.com/zilliztech/memsearch) — biblioteca utilizada neste caso de uso
- [Documentação do memsearch](https://zilliztech.github.io/memsearch/) — referência completa de CLI, API Python e arquitetura
- [OpenClaw](https://github.com/openclaw/openclaw) — arquitetura de memória que inspirou o memsearch
- [Milvus](https://milvus.io/) — banco de dados vetorial de backend
