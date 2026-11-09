# Home Server com Auto-Recuperação (Self-Healing) e Gestão de Infraestrutura

Manter um servidor doméstico (home server) significa estar de plantão 24/7 para sua própria infraestrutura. Serviços caem às 3h da manhã, certificados expiram em silêncio, o disco enche e pods entram em crash-loop — tudo isso enquanto você dorme ou está fora.

Este caso de uso transforma o OpenClaw em um agente de infraestrutura persistente com acesso SSH, tarefas cron automatizadas e a capacidade de detectar, diagnosticar e resolver incidentes antes mesmo de você perceber que há um problema.

## O Problema

Quem mantém um laboratório doméstico (homelab) ou aplicações auto-hospedadas enfrenta uma rotina constante de manutenção:

- Verificações de integridade (health checks), monitoramento de logs e alertas exigem configuração e atenção manuais
- Quando algo quebra, você precisa acessar via SSH, diagnosticar e consertar — muitas vezes diretamente pelo celular
- A infraestrutura como código (Terraform, Ansible, manifestos do Kubernetes) requer atualizações frequentes
- O conhecimento sobre sua infraestrutura fica guardado na sua cabeça, e não em uma documentação pesquisável
- Tarefas rotineiras (triagem de e-mails, checagens de deploy, auditorias de segurança) consomem horas toda semana

## O Que Ele Faz

- **Monitoramento automatizado de integridade**: Checagens periódicas via cron em serviços, deploys e recursos do sistema
- **Auto-recuperação (Self-healing)**: Detecta falhas via health checks e aplica correções de forma autônoma (reinicia pods, escala recursos, corrige configurações)
- **Gestão de infraestrutura**: Cria e aplica manifestos de Terraform, Ansible e Kubernetes
- **Boletins matinais**: Resumo diário da saúde dos sistemas, compromissos na agenda, previsão do tempo e status do quadro de tarefas
- **Triagem de e-mails**: Varre a caixa de entrada, rotula mensagens importantes e arquiva ruídos
- **Extração de conhecimento**: Converte anotações e exportações de conversas em uma base de conhecimento estruturada e consultável
- **Pipeline de publicação de blog**: Rascunho → geração de banner → publicação no CMS → deploy na hospedagem — tudo totalmente automatizado
- **Auditoria de segurança**: Varreduras regulares em busca de segredos expostos no código, contêineres privilegiados e permissões excessivas

## Skills Necessárias

- Acesso via `ssh` às máquinas da rede local
- `kubectl` para administração de clusters Kubernetes
- `terraform` e `ansible` para infraestrutura como código
- CLI do `1password` para gerenciamento de segredos
- CLI `gog` para acesso a e-mails
- Acesso à API de calendário
- Cofre do Obsidian ou diretório de anotações (para a base de conhecimento)
- `openclaw doctor` para auto-diagnóstico

## Como Configurar

### 1. Configuração Principal do Agente

Defina o nome do seu agente e seu escopo de acesso no arquivo AGENTS.md:

```text
## Agente de Infraestrutura

Você é o Reef, um agente de gerenciamento de infraestrutura.

Acesso:
- SSH em todas as máquinas da rede doméstica (192.168.1.0/24)
- kubectl para o cluster K3s
- Cofre do 1Password (somente leitura para credenciais, cofre exclusivo para a IA)
- Gmail via CLI gog
- Calendário (o seu + o da parceira)
- Cofre do Obsidian em ~/Documents/Obsidian/

Regras:
- NUNCA exponha segredos no código — use sempre a CLI do 1Password ou variáveis de ambiente
- NUNCA envie commits direto para a branch main — crie sempre um PR
- Execute `openclaw doctor` como parte das rotinas de auto-diagnóstico
- Registre todas as mudanças de infraestrutura em ~/logs/infra-changes.md
```

### 2. Sistema de Tarefas Cron Automatizadas

O grande poder desta configuração está no agendamento de rotinas. Configure no arquivo HEARTBEAT.md:

```text
## Agendamento Cron

A cada 15 minutos:
- Verificar o quadro kanban em busca de tarefas em andamento → continuar o trabalho

A cada hora:
- Monitorar verificações de integridade (Gatus, ArgoCD, endpoints de serviços)
- Triagem do Gmail (rotular itens acionáveis, arquivar ruídos)
- Checar alertas ou notificações sem resposta

A cada 6 horas:
- Alimentação da base de conhecimento (processar novas notas do Obsidian)
- Auto-diagnóstico (openclaw doctor, uso de disco, memória, logs)

A cada 12 horas:
- Auditoria de qualidade de código e documentação
- Análise de logs via Loki/stack de monitoramento

Diariamente:
- 04:00: Brainstorm noturno (explorar conexões entre anotações)
- 08:00: Boletim matinal (clima, agendas, estatísticas dos sistemas, quadro de tarefas)
- 01:00: Avaliação de velocidade (melhorias nos processos)

Semanalmente:
- Revisão de controle de qualidade (QA) da base de conhecimento
- Auditoria de segurança da infraestrutura
```

### 3. Configuração de Segurança (Crítico)

Isto não é negociável. Antes de liberar o acesso SSH ao seu agente:

```text
## Lista de Checagem de Segurança

1. Hooks de pré-push (pre-push hooks):
   - Instale o TruffleHog ou outro scanner de segredos em TODOS os repositórios
   - Bloqueie qualquer commit que contenha chaves de API, tokens ou senhas expostas no código

2. Fluxo de Git com foco local (Local-first):
   - Use o Gitea (auto-hospedado) para código privado antes de enviar ao GitHub público
   - O pipeline de CI (Woodpecker ou similar) deve rodar antes de qualquer envio público
   - Revisão humana obrigatória antes do merge na branch principal (main)

3. Defesa em profundidade:
   - Cofre exclusivo no 1Password para o agente de IA (escopo restrito)
   - Segmentação de rede para serviços sensíveis
   - Auditorias diárias de segurança automatizadas checando:
     * Contêineres rodando em modo privilegiado
     * Segredos expostos em código ou arquivos de configuração
     * Permissões excessivas de arquivos ou rede
     * Vulnerabilidades conhecidas nas imagens em produção

4. Restrições do agente:
   - Proteção de branches: PR obrigatório para a main, o agente não pode ignorar essa regra
   - Acesso somente leitura onde a escrita não for estritamente necessária
   - Todas as modificações registradas e auditáveis via git
```

### 4. Modelo de Boletim Matinal

```text
## Formato do Boletim Diário

Gerar e entregar às 08:00:

### Previsão do Tempo
- Condições atuais e previsão para [sua localização]

### Calendários
- Seus eventos de hoje
- Eventos da parceira hoje
- Conflitos ou sobreposições sinalizados

### Saúde dos Sistemas
- CPU / Memória RAM / Armazenamento em todas as máquinas
- Serviços: status ONLINE/OFFLINE
- Deploys recentes (ArgoCD)
- Quaisquer alertas emitidos nas últimas 24h

### Quadro de Tarefas
- Cartões concluídos ontem
- Cartões em andamento
- Itens bloqueados que requerem atenção

### Destaques
- Ideias notáveis do brainstorm noturno
- E-mails que exigem ação imediata
- Prazos que vencem esta semana
```

## Principais Insights

- **"Nem acredito que agora tenho um servidor que se conserta sozinho"**: O agente pode rodar comandos via SSH, Terraform, Ansible e kubectl para resolver incidentes de infraestrutura antes mesmo de você saber que houve um problema.
- **A IA colocará segredos direto no código**: Este é o risco de segurança número 1. O agente vai facilmente colocar uma chave de API inline no código se você não implementar barreiras de proteção. Hooks de pre-push e varredura de segredos são obrigatórios.
- **Git com foco local é essencial**: Nunca permita que o agente envie commits diretamente para repositórios públicos. Utilize uma instância privada do Gitea como área de teste (staging) com varredura de CI.
- **As tarefas cron são o verdadeiro produto**: A automação programada (health checks, triagem de e-mails, boletins) gera muito mais valor no dia a dia do que comandos pontuais.
- **A extração de conhecimento se multiplica**: Transformar notas, históricos de chat e e-mails em uma base de conhecimento estruturada fica mais valioso com o tempo — um usuário extraiu 49.079 fatos atômicos apenas do seu histórico no ChatGPT.

## Inspirado Em

Este caso de uso é baseado no relato detalhado de Nathan: ["Everything I've Done with OpenClaw (So Far)"](https://madebynathan.com/2026/02/03/everything-ive-done-with-openclaw-so-far/), onde ele descreve seu agente OpenClaw "Reef", executando em um servidor doméstico com acesso SSH a todas as máquinas, um cluster Kubernetes, integração com 1Password e um cofre do Obsidian com mais de 5.000 notas. O Reef roda 15 tarefas cron ativas, 24 scripts personalizados e já desenvolveu e implantou aplicações de forma autônoma, incluindo uma interface de gerenciamento de tarefas. A lição aprendida a duras penas por Nathan após expor uma chave de API no primeiro dia: "Assistentes de IA vão facilmente chumbar credenciais no código. Eles às vezes não têm os mesmos instintos de segurança que os humanos." Sua configuração de defesa em profundidade (hooks do TruffleHog, Gitea local, varredura de CI, auditorias diárias) é uma leitura indispensável para quem deseja replicar esse padrão.

Também referenciado no [OpenClaw Showcase](https://openclaw.ai/showcase), onde `@georgedagg_` descreveu um padrão similar: monitoramento de deploy, revisão de logs, correções de configuração e envio de PRs — tudo isso enquanto passeava com o cachorro.

## Links Relacionados

- [Artigo Completo de Nathan](https://madebynathan.com/2026/02/03/everything-ive-done-with-openclaw-so-far/)
- [Documentação do OpenClaw](https://github.com/openclaw/openclaw)
- [TruffleHog (Varredura de Segredos)](https://github.com/trufflesecurity/trufflehog)
- [K3s (Kubernetes Leve)](https://k3s.io/)
- [Gitea (Git Auto-Hospedado)](https://gitea.io/)
- [n8n (Automação de Fluxos)](https://n8n.io/)
