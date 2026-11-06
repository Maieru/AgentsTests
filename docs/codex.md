# Usar no Codex

## Instalar a ferramenta

Se você já usa o aplicativo ou a extensão do Codex, abra a pasta do projeto e
inicie uma conversa nela. Para instalar o CLI via npm:

```bash
npm install -g @openai/codex
codex --version
codex
```

Esse método exige Node.js e npm compatíveis com o pacote. Siga a autenticação
oferecida pelo cliente. Consulte as opções e os requisitos atuais na
[documentação oficial do CLI](https://developers.openai.com/codex/cli/).

## Implementar um pedido sem repetir o fluxo

Com os arquivos instalados no projeto da aplicação, envie:

```text
Implemente o pedido descrito em pedidos/solicitacao.pdf.
```

O agente principal lê as instruções do projeto e assume o papel de Gendo. Ele deve
criar o registro, encaminhar as etapas aos especialistas e apresentar a entrega.
O PDF precisa existir nesse caminho, ou estar disponível como anexo que o cliente
consiga ler. Veja as [orientações de leitura de PDF](pedidos-em-pdf.md).

O Codex descobre `AGENTS.md` como instruções do projeto. É nele que este repositório
solicita a coordenação e a delegação para pedidos de implementação. Os Markdown
em `.github/agents/` são lidos pelos adaptadores e pelas instruções do projeto;
seu YAML é uma configuração do Copilot. Veja a
[referência de AGENTS.md](https://developers.openai.com/codex/guides/agents-md/).

## Agentes nativos e disponibilidade

Versões atuais do Codex local aceitam agentes em `.codex/agents/*.toml`.
Incluímos `melchior`, `balthasar`, `casper` e `gendo`. Os adaptadores apontam
para os mesmos perfis Markdown e herdam modelo e permissões da sessão.

No fluxo padrão, a conversa principal já coordena; não é necessário criar um
subagente Gendo para depois ele delegar novamente. O adaptador
`gendo.toml` fica disponível para uso explícito em clientes que permitirem
esse tipo de execução. Prefira manter a coordenação na conversa principal.

As etapas dependentes são sequenciais. Se a delegação não estiver disponível,
o agente deve informar e executar os papéis na mesma conversa, sem alegar que
houve uma revisão em contexto separado. Delegação consome trabalho adicional de
modelo e tokens. Consulte a
[documentação oficial de subagentes](https://developers.openai.com/codex/multi-agent/)
para disponibilidade e configuração da sua versão.

## Confirmar a instalação sem implementar

Inicie uma nova conversa na raiz do projeto e envie:

```text
Confira AGENTS.md, o perfil Gendo e os três especialistas.
Explique o fluxo padrão e informe se esta sessão tem ferramentas de
subagentes. Não crie uma tarefa, não implemente e não inicie subagentes.
```

Para avaliar a execução completa, use um pedido pequeno em um projeto de teste
com requisitos que você conhece. Confira o registro gerado, as etapas realizadas
e o comportamento real do código. Ler os arquivos não comprova o fluxo completo.

## Usar um especialista isoladamente

Pedidos explícitos continuam prevalecendo sobre o padrão:

```text
Melchior, só planeje como adicionar paginação à listagem de produtos.
Entregue o plano na conversa e não altere arquivos.
```

Você também pode pedir “direto com Balthasar” ou “Casper, revise estas alterações”.
Para retomar o fluxo coordenado, informe o caminho do registro da tarefa.

## Usar em outro repositório

Na raiz desta coleção, ajuste o destino e execute em Bash ou Zsh:

```bash
destino="../meu-projeto"
mkdir -p "$destino/.github/agents" "$destino/.codex/agents" "$destino/templates"
cp -i .github/agents/*.agent.md "$destino/.github/agents/"
cp -i .codex/agents/*.toml "$destino/.codex/agents/"
cp -i templates/tarefa-documento.md "$destino/templates/"
```

Copie também `AGENTS.md` para a raiz do destino **se ainda não existir**. Se existir,
mescle a tabela de papéis, a escolha de papel, o fluxo padrão e os acordos
compartilhados. O fluxo padrão precisa ser copiado para que a delegação não dependa
de um prompt repetido. Preserve comandos e convenções reais do projeto.
A seção “Manutenção deste repositório” é específica desta coleção e pode ser omitida.

No PowerShell ou pela interface do editor, copie os mesmos arquivos mantendo a
estrutura. A pasta `tasks/` é criada pelo Gendo quando houver uma tarefa.
Abra o projeto de destino em uma nova conversa para carregar as instruções.

Prefira instalação por projeto para começar. Copiar apenas os TOML para
`~/.codex/agents/` deixa a instalação incompleta: eles dependem dos perfis e do
template no projeto, além dos acordos de `AGENTS.md`.

## Diagnóstico e permissões

| Sintoma | O que conferir |
| --- | --- |
| Preciso repetir o fluxo | Confirme o fluxo padrão em AGENTS.md no projeto aberto e inicie nova conversa |
| O perfil não foi encontrado | Confirme os quatro Markdown em .github/agents/ |
| Instruções parecem diferentes | Confira pasta, instruções globais, AGENTS.override.md e instruções locais |
| Delegação não está disponível | Confira cliente, versão e configuração; registre a execução na mesma conversa |
| PDF não está acessível | Confira se o arquivo existe no ambiente que executa a tarefa |
| Casper não consegue testar | Confira dependências, ambiente e permissões; registre o critério não verificado |

O YAML do Copilot não restringe ferramentas do Codex. Instruções de papel também
não criam um sandbox separado. Configure as permissões no cliente. Para uma
análise isolada com restrição de escrita no CLI, use `codex --sandbox read-only`;
o fluxo de implementação com registro em arquivo exige acesso de escrita adequado.
