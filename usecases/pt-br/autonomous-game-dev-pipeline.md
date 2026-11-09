# Pipeline Autônomo de Desenvolvimento de Jogos Educacionais

## O Problema
**A História de Origem:** Um pai "LANero das antigas" queria criar um portal de jogos seguro, sem anúncios e de alta qualidade para suas filhas, Susana (3) e Julieta (em breve). Os sites existentes estavam cheios de spam, anúncios agressivos e botões enganosos (dark patterns) que frustravam sua filha pequena.

**O Desafio:** Construir um portal "limpo, rápido e simples" era a parte fácil. O verdadeiro desafio era povoá-lo com **mais de 40 jogos educativos** sob medida para faixas etárias específicas (0 a 15 anos) sem contar com uma equipe de desenvolvedores. O desenvolvimento manual era lento demais para um pai desenvolvedor solo, e manter a consistência em dezenas de jogos estava se tornando um pesadelo.

## O Que Ele Faz
Este caso de uso define um "Agente Desenvolvedor de Jogos" que gerencia autonomamente todo o ciclo de vida de criação e manutenção de jogos. O fluxo de trabalho impõe uma política de **"Bugs Primeiro"** (Bugs First), na qual o agente deve verificar e resolver bugs relatados antes de implementar novos recursos.

**Eficiência:** Este pipeline é capaz de produzir **1 novo jogo ou correção de bug a cada 7 minutos**. O agente itera incansavelmente pelo backlog de mais de 41 jogos planejados, alternando entre criar novos conteúdos e corrigir problemas detectados em ciclos anteriores.

Quando o caminho estiver livre de bugs, o agente:
1.  **Seleciona**: Identifica o próximo jogo de uma fila (`development-queue.md`) com base em uma estratégia "Round Robin" para equilibrar o conteúdo entre as faixas etárias.
2.  **Implementa**: Escreve o código HTML5/CSS3/JS do jogo, seguindo rigorosamente o `game-design-rules.md` (sem frameworks, mobile-first, suporte offline).
3.  **Registra**: Adiciona automaticamente os metadados do jogo ao registro central (`games-list.json`).
4.  **Documenta**: Atualiza o status no `CHANGELOG.md` e no `master-game-plan.md`.
5.  **Faz o Deploy**: Cuida do fluxo Git: busca atualizações da master, cria uma branch de feature, commita alterações com conventional commits e faz o merge de volta.

## Prompts

O núcleo deste fluxo de trabalho são as **Instruções de Sistema** fornecidas ao agente. Esse prompt transforma o LLM em um desenvolvedor disciplinado que respeita a estrutura rígida do projeto.

*(**Nota:** Os prompts reais usados em produção estão em **espanhol** (`es-419`) para se alinhar ao público-alvo do projeto (crianças latino-americanas) e aos potenciais futuros colaboradores da região. A versão abaixo foi traduzida para esta documentação.)*

```text
Atue como um Especialista em Desenvolvimento de Jogos Web e UX Infantil.
Seu objetivo é desenvolver o próximo jogo na fila de produção.

Por favor, leia e analise os seguintes arquivos de contexto antes de começar:

1.  CONTEXTO DE BUGS (Prioridade Máxima - CRÍTICO):
    @[bugs/]
    (Verifique esta pasta. Se houver arquivos, SUA TAREFA É CORRIGIR **APENAS O PRIMEIRO ARQUIVO** (em ordem alfabética). Ignore o restante dos bugs e a fila de jogos por enquanto).

2.  CONTEXTO DA FILA (Qual jogo é o próximo):
    @[development-queue.md]
    (Identifique o jogo marcado como [NEXT] na seção "Next Games". SOMENTE se não houver bugs).

3.  REGRAS DE DESIGN (Padrões Técnicos):
    @[game-design-rules.md]
    (Siga rigorosamente estas regras: HTML/CSS/JS puro, estrutura de pastas, responsividade mobile)

4.  ESPECIFICAÇÕES DO JOGO (Mecânicas e Recursos):
    (Identifique o arquivo correspondente em games-backlog/ com base no ID do jogo)

5.  REGISTRO CENTRAL (Integração):
    @[public/js/games-list.json]
    (Arquivo onde você DEVE registrar o novo jogo para que ele apareça na página inicial)

TAREFA:
0. **BUGS PRIMEIRO!**: Se a pasta `bugs/` contiver arquivos, sua única prioridade é corrigir **o primeiro bug em ordem alfabética**. Crie uma branch `fix/...`, resolva **esse** bug, atualize o status e faça o merge. **Não tente corrigir múltiplos bugs de uma vez.**
   - SE NÃO HOUVER BUGS, prossiga com o próximo jogo:

1. **Sincronização**: `git fetch && git pull origin master` (CRÍTICO).
2. Crie uma nova branch: `git checkout -b feature/[game-id]`.
3. Crie a pasta e os arquivos em 'public/games/[game-id]/'.
4. Implemente a lógica e o design de acordo com o backlog e as regras de design.
5. Registre o jogo em 'games-list.json' (CRÍTICO).
6. Ao finalizar:
    - Atualize o `CHANGELOG.md` incrementando a versão.
    - Atualize o `master-game-plan.md` e o `development-queue.md`.
    - Documente as alterações: `git commit -m "feat: add [game-id]"`.
7. **Entrega**:
    - Envie: `git push origin feature/[game-id]`.
    - Solicite merge para a master.
    - Uma vez na master, envie as alterações (`git push origin master`).
```

## Skills Necessárias
-   **Git**: Para gerenciar branches, commits e merges.
  
## Links Relacionados
-   [História de Origem do Projeto (LinkedIn)](https://www.linkedin.com/feed/update/urn:li:activity:7429739200301772800/) - Como este projeto surgiu após a configuração do OpenClaw.
-   [Repositório El Bebe Games](https://github.com/duberblockito/elbebe/tree/master) - Código-fonte.
-   [Site ao Vivo El Bebe Games](https://elbebe.co/) - O resultado deste pipeline.
-   [Boas Práticas de Desenvolvimento de Jogos em HTML5](https://developer.mozilla.org/en-US/docs/Games)
