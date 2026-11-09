# Framework de CRM Local com DenchClaw

Configurar um CRM que realmente funcione com o OpenClaw costuma ser trabalhoso. Você precisa conectar bancos de dados, criar interfaces visuais, configurar automação de navegador, integrar plataformas de mensagens e, de alguma forma, fazer o agente entender seu esquema de dados. A maioria das pessoas desiste no meio do caminho e acaba com uma integração do Notion que mal funciona.

O DenchClaw é um framework de código aberto que transforma o OpenClaw em uma plataforma totalmente local de CRM, automação de vendas e produtividade — instalado com um único comando e rodando inteiramente na sua máquina.

## Dores e Desafios

O OpenClaw é incrivelmente poderoso como primitiva, mas utilizá-lo para fluxos reais de negócios (rastreamento de leads, prospecção outbound, gestão de pipeline) exige juntar uma dúzia de ferramentas: banco de dados, interface do usuário (UI), acesso ao navegador, integrações de mensagens, gerenciamento de arquivos. Cada nova integração significa mais configuração manual, mais credenciais para gerenciar e mais coisas que podem quebrar. Você quer uma experiência de usuário (UX) no nível do Cursor para as operações do seu negócio, e não um monte de scripts em shell.

## O Que Ele Faz

- **Configuração com um comando**: `npx denchclaw` instala tudo — banco de dados DuckDB, interface web, perfil do OpenClaw, automação de navegador, skills — e abre em `localhost:3100`
- **CRM em linguagem natural**: Pergunte "mostre-me empresas com mais de 5 funcionários" e ele atualiza a visualização em tempo real. Sem necessidade de filtros manuais
- **Automação completa do navegador**: Copia seu perfil do Chrome para que o agente tenha o mesmo estado de autenticação — diga "importe tudo do meu HubSpot" e ele faz login, exporta e importa
- **Múltiplas visualizações**: Visualizações em Tabela, Kanban, Calendário, Linha do Tempo (Gantt), Galeria e Lista — todas configuráveis pelo agente via YAML
- **Construtor de aplicativos (App builder)**: O OpenClaw constrói aplicações web independentes (dashboards, ferramentas, jogos) que rodam dentro do workspace com acesso aos seus dados
- **Orientado ao sistema de arquivos (File-system-first)**: Filtros de tabela, visualizações, alternância de colunas, configurações de calendário — tudo é um arquivo, para que o OpenClaw possa ler e modificar diretamente
- **Funciona como um agente de código**: O DenchClaw construiu o próprio DenchClaw. Ele também é um navegador de árvore de arquivos completo e editor de código para o seu Mac

## Como Configurar

1. Instale com um único comando:

```bash
npx denchclaw
```

2. Conclua o assistente de introdução (onboarding). O DenchClaw cria um perfil dedicado do OpenClaw chamado `dench` e inicia um gateway na porta 19001.

3. Abra `localhost:3100` no seu navegador. No Safari, adicione-o ao Dock como um PWA.

4. Comece a conversar com ele:

```text
Olá, crie um objeto "Leads" com os campos: Nome, E-mail, Empresa, Status (Novo/Contatado/Qualificado/Ganho/Perdido) e Observações. Importe este CSV de leads que baixei do Apollo.
```

```text
Mostre-me todos os leads onde o Status é "Contatado" e ordene pela data da última atualização. Mude para a visualização Kanban agrupada por Status.
```

```text
Acesse meu LinkedIn, encontre as últimas 20 pessoas que visualizaram meu perfil e adicione-as como leads com as informações da empresa enriquecidas.
```

```text
Crie o rascunho de um e-mail de prospecção personalizado para cada lead no status "Novo", com base nas notícias recentes de suas empresas. Salve os rascunhos em um novo documento chamado "Rascunhos de Prospecção".
```

5. Você também pode usá-lo a partir do Telegram ou de qualquer plataforma de mensagens conectada — gerencie seu pipeline pelo celular onde você estiver.

## Skills Necessárias

O DenchClaw já inclui todas as skills necessárias por padrão:

- **CRM Skill** — Gestão de dados estruturados suportada por DuckDB com objetos, campos, registros e múltiplos tipos de visualização
- **App Builder Skill** — Criação de web apps (dashboards, ferramentas) executados dentro do workspace com acesso ao seu banco de dados
- **Browser Automation Skill** — Navegador Chromium com o estado de autenticação existente do seu Chrome para web scraping, importações e prospecção outbound

Nenhuma instalação ou configuração adicional de skills é necessária.

## Principais Insights

- **Sistema de arquivos = UI nativa para agentes**: Como cada configuração, filtro e visualização é armazenada como um arquivo YAML/markdown, o OpenClaw pode modificar a interface com a mesma naturalidade com que edita código. Sem necessidade de wrappers de API.
- **O DuckDB é o ponto de equilíbrio ideal**: O menor e mais performático banco de dados embarcado que ainda oferece suporte completo a SQL. Sem processo de servidor separado, sem credenciais, sem rede — apenas um arquivo.
- **Clonar o perfil do Chrome é um superpoder**: Em vez de lutar com fluxos OAuth e limites de taxa de API, o DenchClaw copia o estado de autenticação do seu navegador. O agente enxerga o que você enxerga e faz o que você faz.
- **Um comando `npx` supera um fim de semana inteiro de configuração**: Toda a stack (banco de dados, interface web, perfil do OpenClaw, gateway, navegador, skills) se auto-instala e configura. Sem Docker, sem arquivos de variáveis de ambiente (.env), sem pesadelo de dependências.

## Demonstração

[Assista ao vídeo de demonstração](https://www.youtube.com/watch?v=pfACTbc3Bh4#t=43) — mostra o fluxo completo, desde a instalação até o gerenciamento de um pipeline de CRM em tempo real por meio de linguagem natural.

## Links Relacionados

- [DenchClaw GitHub](https://github.com/DenchHQ/DenchClaw) — Licença MIT, código aberto
- [DenchClaw Website](https://denchclaw.com)
- [Discord Community](https://discord.gg/PDFXNVQj9n)
- [Skills Store](https://skills.sh)
