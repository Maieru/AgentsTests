# Melchior, Balthasar e Casper

Três especialistas para desenvolver software com IA e aprender com o processo: **Melchior pensa a solução, Balthasar constrói e Casper verifica**. O orquestrador **Gendo** recebe seus pedidos em PDF ou texto e conduz o trabalho entre eles.

Este repositório reúne perfis em Markdown, integração com Codex e GitHub Copilot e um fluxo de trabalho reutilizável. O objetivo é melhorar a qualidade das decisões, do código e da sua compreensão sobre o que está sendo entregue.

Os agentes são instruções para a ferramenta de IA que você já utiliza. Ao receber um pedido de implementação, o Gendo organiza os requisitos, delega as etapas e registra os resultados. A execução depende das ferramentas e permissões do cliente; não há serviço monitorando arquivos em segundo plano.

## Conheça os agentes

| Agente | Papel | Quando chamar | Entrega principal |
| --- | --- | --- | --- |
| [Gendo](.github/agents/gendo.agent.md) | Coordenação | Implementar um pedido em PDF ou texto sem transportar contexto manualmente | Registro da tarefa, handoffs, controle de correções e entrega |
| [Melchior](.github/agents/melchior.agent.md) | Arquiteto | Entender um problema, avaliar alternativas, planejar mudanças | Plano com decisões, arquivos envolvidos, riscos e critérios de aceite |
| [Balthasar](.github/agents/balthasar.agent.md) | Implementador | Criar funcionalidades, corrigir bugs, refatorar | Mudança de código acompanhada de validação e explicação |
| [Casper](.github/agents/casper.agent.md) | QA e revisor | Investigar regressões, revisar código e verificar requisitos | Achados reproduzíveis, evidências e limitações da revisão |

Melchior devolve o plano ao Gendo. Balthasar altera os arquivos. Casper revisa e executa verificações, deixando as correções para Balthasar. O Gendo mantém o registro compartilhado e encaminha os resultados. Os três especialistas também podem ser usados isoladamente.

## Comece por aqui

```bash
git clone https://github.com/Maieru/AgentsTests.git
cd AgentsTests
```

Escolha a ferramenta:

- **Codex:** siga o [guia de instalação e uso](docs/codex.md). O `AGENTS.md` define a coordenação como padrão para pedidos de implementação; os adaptadores em `.codex/agents/` permitem usar os especialistas em versões compatíveis.
- **GitHub Copilot:** siga o [guia para VS Code e CLI](docs/github-copilot.md). Selecione **Gendo** para conduzir o fluxo completo.
- **Outro assistente:** forneça o conteúdo de um perfil e as instruções do projeto como contexto da conversa.

Depois de instalar os perfis no projeto em que o código será desenvolvido, anexe o PDF ou indique seu caminho e peça:

```text
Implemente o pedido descrito em pedidos/solicitacao.pdf.
```

Você não precisa repetir o fluxo nem preencher templates: o Gendo cria `tasks/<id>/tarefa.md`, extrai os requisitos com referências à fonte e conduz as etapas. Veja [como trabalhar com PDFs](docs/pedidos-em-pdf.md), inclusive quando o arquivo contém imagens ou exige OCR.

## Um fluxo que cabe no seu dia a dia

```mermaid
flowchart LR
    U[Você: pedido e PDF] --> O[Gendo: requisitos e registro]
    O --> M[Melchior: plano]
    M --> B[Balthasar: implementação]
    B --> C[Casper: revisão e QA]
    C -->|Até duas rodadas de correção| B
    C --> O2[Gendo: consolida evidências e pendências]
    O2 --> R[Você: revisão final]
```

Por padrão, um pedido de implementação passa pelos três especialistas. Diga “direto com Balthasar” para uma tarefa pequena, “só planeje” para obter análise ou “sem subagentes” para trabalhar na mesma conversa. Perguntas e resumos não iniciam implementação.

O pedido original autoriza avançar entre plano, implementação e QA sem confirmações rotineiras. O Gendo pede esclarecimento quando faltar uma decisão essencial e encerra com impedimentos registrados se as verificações não forem suficientes. Sem suporte a subagentes, ele executa os papéis sequencialmente na mesma conversa e informa essa limitação.

Você define o objetivo, acompanha as decisões e revisa a entrega. Uma revisão de IA acrescenta evidências; a responsabilidade pela aceitação continua com você.

## Use no seu projeto

Copie os perfis para o repositório em que o código será desenvolvido. Mescle as instruções com os arquivos que já existirem, preservando as convenções locais. Os guias de [Codex](docs/codex.md#usar-em-outro-repositório) e [Copilot](docs/github-copilot.md#usar-em-outro-repositório) mostram quais arquivos são necessários.

Não há dependências de aplicação, build ou servidor neste repositório. Git ajuda a revisar e desfazer mudanças; a ferramenta de IA exige instalação e acesso próprios.

## Como aprender com os agentes

- Peça a Melchior a justificativa da decisão e uma alternativa que valha comparar.
- Peça a Balthasar para relacionar a mudança aos critérios de aceite e explicar um trecho que você ainda não domina.
- Peça a Casper um caso concreto que quebraria a implementação e como reproduzi-lo.
- Antes de aceitar uma mudança, tente explicar com suas palavras por que ela funciona e qual teste confirma isso.

As respostas incluem explicações curtas por padrão. Para praticar antes de receber a solução, diga: “modo estudo: apresente uma pista e aguarde minha tentativa”.

## Documentação

| Documento | Conteúdo |
| --- | --- |
| [Codex](docs/codex.md) | Instalação, perfis, delegação e diagnóstico |
| [GitHub Copilot](docs/github-copilot.md) | Configuração no VS Code, CLI e outros projetos |
| [Fluxo de trabalho](docs/fluxo-de-trabalho.md) | Passagem de contexto, critérios de conclusão e revisão |
| [Pedidos em PDF](docs/pedidos-em-pdf.md) | Recebimento, extração, páginas, OCR e retomada |
| [Exemplos de prompts](docs/exemplos.md) | Funcionalidade, bug, revisão e estudo |
| [Template de tarefa](templates/tarefa.md) | Objetivo, contexto e critérios de aceite |
| [Registro de pedido em documento](templates/tarefa-documento.md) | Requisitos, plano, implementação, rodadas de QA e evidências |
| [Template de passagem de contexto](templates/handoff.md) | Informações para trocar de agente ou conversa |
| [Contribuindo](CONTRIBUTING.md) | Como adaptar e avaliar os perfis |

## Limites e evolução

O registro em arquivo permite retomar uma tarefa em outra conversa, desde que o workspace continue disponível. Os perfis não criam memória fora dele nem garantem ausência de bugs ou isolamento de permissões. Modelos podem concordar com um erro anterior; Casper deve verificar o código e os requisitos diretamente. O acesso às ferramentas depende do cliente, da versão e das permissões da sessão.

Comece com uma tarefa pequena e conhecida. Registre quais decisões melhoraram, quais erros escaparam e quanto retrabalho houve. Ajuste os perfis com base nesses resultados, mantendo o mesmo exemplo para comparar as versões.
