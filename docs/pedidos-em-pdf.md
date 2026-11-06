# Pedidos de implementação em PDF

## Uso diário

Instale a configuração no repositório da aplicação. No Codex, abra uma conversa
nesse projeto. No Copilot, selecione **Gendo**. Anexe o PDF, se o cliente
permitir sua leitura, ou coloque-o em um caminho acessível ao ambiente e envie:

```text
Implemente o pedido descrito em pedidos/solicitacao.pdf.
```

Se o pedido for um anexo disponível no chat:

```text
Implemente a funcionalidade descrita no PDF anexado.
```

Esses exemplos pressupõem um arquivo real e o projeto de destino aberto.
Esta coleção não inclui um PDF de cliente nem uma aplicação para implementar.
Você não precisa escrever os prompts individuais ou preencher os templates.

## O que o Gendo deve fazer

Ele lê a fonte, cria `tasks/<id>/tarefa.md`, registra requisitos com referências
de página e chama Herta, Melchior, Balthasar e Casper sequencialmente. Herta refina
o pedido antes de Melchior planejar a solução técnica. Gendo avança até a
entrega ou até um impedimento que não consiga resolver no escopo autorizado.
Um detalhe técnico reversível pode ser assumido e registrado; uma regra de
negócio essencial ausente precisa ser esclarecida.

O requisito fica ligado à verificação. Exemplo **ilustrativo**, sem PDF lido ou
teste executado:

| Requisito | Origem ilustrativa | Critério | Verificação prevista |
| --- | --- | --- | --- |
| R01: limitar a página a 100 produtos | solicitação fictícia, página física 2 | CA01: rejeitar limite acima de 100 | Enviar uma requisição com limite 101 e conferir o erro definido pela API |

Herta verifica se os comportamentos de negócio e os critérios estão definidos
ou se precisam de esclarecimento. Melchior investiga o contrato existente para
planejar como implementá-los. A coluna de evidências só é preenchida depois da
verificação real. Inferências ficam separadas dos requisitos explícitos.

Por exemplo, “exportar relatório” pode deixar em aberto quais dados exportar, quem
tem permissão e se os filtros atuais se aplicam. Herta identifica essas lacunas e
Gendo encaminha as perguntas necessárias a você. Herta não escolhe uma regra
ausente como se tivesse sido aprovada nem define frameworks ou arquitetura.

## PDF com texto, tabelas ou imagens

A leitura depende do cliente. O Gendo pode usar leitura nativa ou um
extrator local disponível. Se o ambiente tiver `pdftotext`, uma inspeção possível é:

```bash
pdftotext -layout "pedidos/solicitacao.pdf" -
```

O comando imprime o texto sem modificar o PDF. Não instala dependências e não
garante interpretar tabelas, diagramas, imagens ou ordem de leitura corretamente.
O Gendo deve conferir esses elementos pelas ferramentas disponíveis ou
registrar o que ficou sem leitura. Em PDFs digitalizados, pode ser necessário
OCR; esse recurso não é instalado nem fornecido pelos perfis.

Se não houver leitura suficiente, a parte afetada aguarda esclarecimento. Não
há promessa de implementar corretamente a partir de páginas que o agente não
conseguiu analisar. Não é necessário transcrever todo o PDF: forneça apenas o
trecho faltante ou uma versão legível quando houver esse impedimento.

## Anexos e ambientes diferentes

Um anexo visível no chat pode não estar disponível ao subagente ou ao terminal.
O Gendo confere esse acesso e preserva no registro os requisitos e trechos
necessários, com origem e limitações. Se você colocar o arquivo no seu computador,
uma sessão remota só poderá lê-lo quando o arquivo também estiver acessível nela.

O PDF original é preservado. Não há envio automático a serviços de conversão nem
inclusão automática do documento em commits. O Gendo registra o mínimo
necessário e respeita as permissões e o destino definidos para a tarefa.

## Retomar sem repetir o pedido

Se uma tarefa parou por uma decisão ou ambiente, responda ao esclarecimento ou
abra outra conversa no mesmo workspace:

```text
Continue a tarefa tasks/pedido-123/tarefa.md.
O formato da exportação deve ser CSV.
```

O Gendo lê o registro, confirma a fonte e o estado atual do código e retoma
a etapa pendente. Alterações no PDF ou nos requisitos são registradas como uma
nova versão/decisão e exigem reavaliar as partes afetadas. O registro não reinicia
o limite de correções nem torna válido um QA feito antes de uma mudança posterior.

## O que “automático” significa aqui

O fluxo inicia quando você envia um pedido de implementação. Copiar um PDF para
uma pasta não dispara uma tarefa em segundo plano. Não há monitoramento de arquivos,
integração com e-mail ou agendamento configurado. Uma sessão ativa e uma ferramenta
com suporte aos recursos necessários continuam sendo requisitos de execução.
