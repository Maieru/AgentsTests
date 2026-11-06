# Usar no GitHub Copilot

## VS Code

Instale ou atualize o VS Code e configure o GitHub Copilot com sua conta. Consulte
o [guia oficial de configuração](https://code.visualstudio.com/docs/copilot/setup).
O acesso depende da conta e das políticas da sua organização.

Abra a raiz da aplicação. No chat com Copilot, escolha **Gendo** no seletor
de agentes para o fluxo completo. Anexe o PDF quando a leitura for suportada ou
indique um caminho acessível no ambiente e envie:

```text
Implemente o pedido descrito em pedidos/solicitacao.pdf.
```

O perfil conduz o fluxo sem precisar de um prompt de coordenação em cada pedido.
A configuração não força a seleção do Gendo em todas as novas conversas:
confira o seletor ao iniciar uma sessão. Para tarefas isoladas, escolha Melchior,
Balthasar ou Casper. Os quatro perfis estão em `.github/agents/*.agent.md`.

Você pode inspecioná-los em **Chat: Open Customizations**, na seção de agentes.
Confira o [guia oficial de custom agents](https://code.visualstudio.com/docs/copilot/customization/custom-agents).

## Copilot CLI

Você pode instalar o CLI via npm com Node.js 22 ou superior. Consulte as
alternativas no [guia oficial](https://docs.github.com/en/copilot/how-tos/set-up/install-copilot-cli).
Na raiz da aplicação, instale e siga a autenticação oferecida:

```bash
npm install -g @github/copilot
copilot --version
copilot
```

Use `/agent` e selecione Gendo. Para uma execução por identificador:

```bash
copilot --agent=gendo --prompt "Implemente o pedido em pedidos/solicitacao.pdf."
```

Consulte a [referência de invocação](https://docs.github.com/en/copilot/how-tos/copilot-cli/use-copilot-cli/invoke-custom-agents).
Essas instruções se referem ao produto `copilot`, e não à antiga extensão `gh copilot`.

## Ferramentas e handoff

Os modelos ficam a seu critério, sem configuração fixa. O cabeçalho YAML declara
as ferramentas esperadas:

| Agente | Ferramentas | Uso esperado |
| --- | --- | --- |
| Gendo | read, search, edit, execute, agent | Ler a fonte, manter o registro e delegar |
| Melchior | read, search | Analisar e planejar |
| Balthasar | read, search, edit, execute | Implementar e validar |
| Casper | read, search, execute | Revisar e executar verificações |

O alias `agent` permite invocar outro custom agent nas experiências que oferecem
essa ferramenta. O Gendo encaminha o registro e aguarda cada resultado;
os especialistas não delegam novamente. Confira o suporte e as ferramentas reais
na [referência oficial](https://docs.github.com/en/copilot/reference/custom-agents-configuration).

A coordenação é descrita no perfil. Não depende de botões `handoffs` ou do campo
`agents` do VS Code: o suporte a esses campos varia entre ferramentas de execução.
Confira a [referência de compatibilidade](https://code.visualstudio.com/docs/agents/reference/custom-agent-file-reference).
Sem ferramenta de delegação, o perfil informa a limitação e executa os papéis
na mesma conversa. Confira a atividade real em vez de presumir contextos separados.

O terminal de Casper pode gerar artefatos e tecnicamente permite escrita; seu
papel proíbe corrigir os arquivos revisados. As permissões da sessão continuam
sendo o limite de execução. O perfil não concede acesso adicional a arquivos ou PDFs.

## Usar em outro repositório

1. Copie os quatro `.agent.md` para `.github/agents/` no projeto de destino.
2. Copie ou mescle `AGENTS.md`, incluindo o fluxo padrão e os acordos compartilhados.
   A seção de manutenção desta coleção pode ser omitida.
3. Copie `templates/tarefa-documento.md`, mantendo esse caminho relativo à raiz.
4. Copie ou mescle `.github/copilot-instructions.md`, preservando as instruções locais.
5. Abra o projeto e escolha **Gendo** em uma nova conversa.

A pasta `tasks/` é criada durante o recebimento de um pedido real. Não é necessário
copiar `.codex/` se o projeto usar somente Copilot. Revise os arquivos antes de
versioná-los na equipe. Veja também o [guia de PDFs](pedidos-em-pdf.md).

## Diagnóstico e outras experiências

Se os nomes não aparecerem, confira raiz aberta, extensão `.agent.md`, sintaxe YAML,
versão e políticas da conta. Reabra o projeto e confira a lista de customizações.
Para um teste sem implementação, peça ao Gendo apenas para explicar o fluxo,
conferir os perfis e informar as ferramentas disponíveis, sem criar uma tarefa.

No GitHub.com, os perfis precisam estar na branch padrão para descoberta pelo
Copilot cloud agent. A interface e a delegação dependem da experiência utilizada;
veja o [guia oficial do cloud agent](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents).
Este fluxo local não controla o ciclo de PRs do cloud agent: os perfis não
publicam nem atribuem tarefas no GitHub automaticamente.
