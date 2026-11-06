# Exemplos de prompts

Depois de instalar a configuração na aplicação, o fluxo completo exige apenas
um pedido curto. No Copilot, selecione **Gendo**. No Codex, o agente principal
segue o fluxo padrão de `AGENTS.md`. Os exemplos pressupõem arquivos acessíveis
e não representam resultados de execução.

## Implementar um pedido em PDF

```text
Implemente o pedido descrito em pedidos/solicitacao.pdf.
```

Ou, se o cliente conseguir ler o anexo:

```text
Implemente a funcionalidade descrita no PDF anexado.
```

O Gendo cria o registro, encaminha os requisitos aos especialistas e
controla as rodadas de correção. Veja [o guia de PDFs](pedidos-em-pdf.md) e
[o fluxo completo](fluxo-de-trabalho.md).

## Retomar uma tarefa com uma decisão nova

```text
Continue a tarefa tasks/pedido-123/tarefa.md.
A exportação deve usar CSV e incluir somente os itens filtrados.
```

## Obter somente o resumo de um documento

```text
Resuma o PDF anexado e liste as dúvidas de negócio. Não implemente.
```

## Escolher uma etapa isolada

Os exemplos seguintes são alternativas explícitas ao fluxo padrão. Substitua os
trechos entre colchetes pelo contexto real. No Copilot, escolha o especialista
correspondente para a etapa; não é preciso voltar ao Gendo para um pedido isolado.

## Refinar um pedido com Herta

No Copilot, selecione Herta. No Codex, envie:

```text
Herta, só refine o pedido descrito em pedidos/solicitacao.pdf.
Leia .github/agents/herta.agent.md. Identifique regras de negócio,
atores, permissões, cenários e exceções pertinentes. Escreva critérios
de aceite verificáveis e destaque as dúvidas com impacto no comportamento.
Separe requisitos explícitos, hipóteses e sugestões opcionais.
Entregue o refinamento na conversa, sem definir arquitetura ou implementar.
```

No fluxo completo, você não precisa enviar esse prompt: Gendo chama Herta e
encaminha o refinamento a Melchior. Para fornecer uma resposta posteriormente:

```text
Continue a tarefa tasks/pedido-123/tarefa.md.
Respondendo à dúvida de Herta: somente administradores podem exportar
o relatório, e a exportação deve respeitar os filtros atuais.
```

## Planejar uma funcionalidade

```text
Atue como Melchior e leia .github/agents/melchior.agent.md.
Quero paginação na listagem de produtos de [projeto].
Critérios: preservar o filtro atual, usar páginas de 20 itens por padrão,
aceitar no máximo 100 e rejeitar parâmetros inválidos de forma consistente.
Investigue os contratos e padrões existentes e proponha como tratar
páginas vazias. Entregue o plano e os critérios de teste na conversa.
Não altere arquivos. Explique uma alternativa e por que a recomenda ou descarta.
```

## Implementar a partir do plano

```text
Atue como Balthasar e leia .github/agents/balthasar.agent.md.
Implemente a paginação conforme o objetivo, os critérios e o plano abaixo.
Confira as premissas no código. Preserve o comportamento dos filtros.
Execute as verificações pertinentes e informe resultados e limitações.
Explique como um dos testes detecta uma regressão.

[Cole o plano de Melchior e as decisões tomadas.]
```

## Revisar a implementação

```text
Atue como Casper e leia .github/agents/casper.agent.md.
Revise [alterações locais ou comparação com uma referência explícita].
Verifique os critérios abaixo e confira os caminhos de execução afetados.
Execute as verificações disponíveis. Priorize bugs, limites e regressões.
Não corrija os arquivos. Traga achados com evidência e um parecer com limitações.

[Cole o objetivo, os critérios, o escopo e os comandos conhecidos.]
```

## Corrigir um bug sem plano formal

```text
Atue como Balthasar e leia .github/agents/balthasar.agent.md.
O cálculo do total do pedido falha ao aplicar [tipo de desconto].
Reprodução: [passos e entrada mínima, sem dados sensíveis].
Esperado: [resultado]. Observado: [resultado ou erro].
Investigue a causa, implemente a correção e crie um teste de regressão
quando viável. Preserve o comportamento de pedidos sem desconto.
```

## Encaminhar uma correção encontrada pelo QA

```text
Atue como Balthasar e leia .github/agents/balthasar.agent.md.
Corrija o achado abaixo mantendo os critérios de aceite originais.
Reproduza a falha quando possível e valide a correção.

[Cole localização, impacto, reprodução e evidência de Casper.]
```

## Estudar antes de implementar

```text
Atue como Melchior e leia .github/agents/melchior.agent.md.
Modo estudo: quero aprender a separar regra de negócio e acesso a dados.
Use [arquivo ou exemplo pequeno] para explicar o problema.
Apresente uma pista e um exercício; aguarde minha tentativa antes
de apresentar a solução. Não altere arquivos.
```

## Delegar uma etapa no Codex

```text
Use o subagente casper para revisar [escopo]. Encaminhe os critérios
abaixo e aguarde a revisão. Não faça correções nesta etapa.
Consolide os achados com suas evidências e limitações.

[Objetivo, critérios de aceite e referência da mudança.]
```

Esse último exemplo exige suporte a agentes nativos no cliente. Para executar
o fluxo sem delegação, você também pode pedir “implemente este pedido sem
subagentes”. O Gendo deve identificar os papéis na mesma conversa e
registrar que a revisão não teve um contexto separado.
