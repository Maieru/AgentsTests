# Fluxo de trabalho

## O padrão para implementar

Depois da instalação, você fornece o pedido em texto ou PDF e solicita a
implementação. O Gendo organiza e encaminha o trabalho:

1. **Recebimento:** confere o documento, identifica requisitos e cria o registro.
2. **Herta:** refina requisitos, regras de negócio, cenários e critérios de aceite.
3. **Melchior:** analisa o código e planeja a solução técnica para os requisitos refinados.
4. **Balthasar:** implementa e executa as verificações pertinentes.
5. **Casper:** revisa a mudança real, os requisitos e as evidências.
6. **Gendo:** encaminha correções quando necessário e consolida a entrega.

As etapas são sequenciais. O Gendo aguarda cada resultado, atualiza
`tasks/<id>/tarefa.md` e fornece o contexto ao próximo especialista. Você não
precisa colar os resultados entre agentes ou aprovar cada passagem.

No Codex, `AGENTS.md` orienta o agente principal a coordenar e solicita delegação
para esse tipo de pedido. No Copilot, selecione o perfil **Gendo**. O suporte
real à delegação depende do cliente; confira os guias de [Codex](codex.md) e
[Copilot](github-copilot.md).

## O que inicia o fluxo

| Pedido | Comportamento |
| --- | --- |
| “Implemente o pedido deste PDF” | Coordenação completa |
| “Corrija o bug descrito abaixo” | Coordenação completa |
| “Resuma este PDF” ou apenas um anexo | Leitura/resumo, sem implementação |
| “Herta, só refine este pedido…” | Refinamento com dúvidas e critérios de aceite, sem implementação |
| “Melchior, só planeje…” | Uma etapa de planejamento |
| “Casper, revise estas alterações…” | Uma etapa de QA |
| “Direto com Balthasar…” | Implementação direta no escopo solicitado |
| “Sem subagentes…” | Papéis na mesma conversa, com a limitação explícita |
| “Continue a tarefa tasks/pedido-123/tarefa.md” | Retoma o estado registrado e confere o workspace atual |

Uma dúvida ou um pedido de explicação durante uma tarefa não reinicia a
coordenação. No modo estudo, respeite o ritmo solicitado: uma pista e espera
pela tentativa do usuário, quando esse for o pedido.

## Um registro para os quatro especialistas

O Gendo usa [o template de documento](../templates/tarefa-documento.md)
para registrar:

- A fonte, versão disponível, páginas lidas e limitações de extração.
- Requisitos `R01…` e critérios observáveis `CA01…`, com origem e evidências.
- Dúvidas, decisões, refinamento de Herta, plano, arquivos alterados e estado efetivo da mudança.
- Verificações executadas, pareceres, achados e rodadas de correção.
- Estado atual e próxima ação para retomada.

Só o Gendo edita o registro. Herta, Melchior e Casper devolvem suas análises;
Balthasar altera o código e devolve seu relatório. Isso evita atualizações
concorrentes do mesmo documento. Se a sessão não puder escrever, o Gendo
mantém o registro na conversa e informa a ausência de persistência em arquivo.

Os especialistas recebem a raiz do projeto e o caminho do registro, sem depender
de acesso ao histórico do chat. Quando o PDF não estiver compartilhado, o registro
inclui os trechos relevantes e indica a limitação. Para revisar a extração, Casper
precisa ter acesso à fonte; um resumo sozinho não comprova sua fidelidade.

## Dúvidas e permissões

O pedido de implementação já autoriza avançar do plano ao código e ao QA. Não há
aprovação adicional obrigatória por etapa. Se o PDF disser, por exemplo, “exportar
relatório” sem definir dados e formato, Herta investiga o contexto e aponta as dúvidas
essenciais. Gendo consolida as perguntas ao usuário e registra as respostas antes
do planejamento dependente. Uma parte já clara pode avançar sem aguardar as demais.

Herta define a especificação de comportamento; Melchior define a solução técnica.
Requisitos não precisam de uma aprovação formal extra quando já estiverem claros.
Se uma nova dúvida de negócio surgir depois, Gendo devolve a questão a Herta,
registra a decisão e reavalia o plano e os testes afetados. IDs e decisões anteriores
são preservados; atualizar requisitos não reinicia o limite de rodadas de QA.

As partes independentes podem avançar. A parte dependente fica em
`aguardando_esclarecimento`. Permissões do ambiente continuam valendo: configurar
um fluxo não concede acesso adicional nem autoriza publicação externa.

## Revisão e limite de correções

A revisão inicial é a **rodada 0**. Achados acionáveis no escopo ou critérios não
atendidos retornam a Balthasar. Após a correção, Casper revisa a versão atual.
São permitidas até duas rodadas adicionais, **1 e 2**, preservando o histórico.
Melhorias opcionais P3 não exigem retrabalho automático.

Se a rodada 2 ainda deixar defeitos ou evidências insuficientes, a tarefa termina
como `bloqueada`, com o diagnóstico e a próxima ação. Retomar uma conversa não
zera a contagem. Rodadas adicionais dependem de solicitação do usuário. Falhas
de ambiente não justificam repetir comandos indefinidamente sem uma mudança útil.

## Critério de conclusão

O Gendo marca `concluida` quando todos os critérios obrigatórios estiverem
atendidos com evidência e não houver defeitos relevantes em aberto. Código escrito
ou uma execução de teste isolada não bastam para afirmar todos os requisitos.

A entrega informa o resultado, a validação, os riscos, as pendências e o caminho
do registro. Inclui uma explicação curta para seu aprendizado. Você faz a revisão
final e decide sobre commit, PR, merge ou publicação.

Uma revisão em outro subagente reduz a influência da narrativa de implementação,
mas o mesmo modelo ainda pode repetir erros. Quando todos os papéis estiverem na
mesma conversa, o relatório precisa deixar isso explícito.

## Etapas manuais e avaliação

Os [templates simples de tarefa](../templates/tarefa.md) e de
[handoff](../templates/handoff.md) continuam úteis para usar um especialista
isolado ou trocar de ferramenta. Não é necessário preenchê-los a cada pedido
no fluxo coordenado.

Para avaliar o ganho, compare tarefas conhecidas: critérios atendidos, erros que
escaparam, retrabalho, tempo de revisão e o que você aprendeu. Use a opção direta
quando o custo de coordenação não compensar para a tarefa.
