# Contribuindo

As contribuições devem melhorar a utilidade dos agentes e facilitar o aprendizado.
Prefira mudanças baseadas em um problema observado: uma decisão sem fundamento,
uma implementação fora do escopo ou uma revisão que deixou passar um bug.

## Onde alterar

- Comportamento de um papel: `.github/agents/<nome>.agent.md`.
- Acordos compartilhados e roteamento: `AGENTS.md`.
- Integração do Copilot: `.github/copilot-instructions.md` e o YAML dos perfis.
- Integração nativa do Codex: `.codex/agents/<nome>.toml`.
- Recebimento de documentos e coordenação: `.github/agents/coordenador.agent.md`
  e `templates/tarefa-documento.md`.
- Instruções de instalação, uso e exemplos: `README.md`, `docs/` e `templates/`.

Mantenha os adaptadores do Codex pequenos e use os Markdown como fonte de verdade.
Evite regras redundantes, modelos obrigatórios e instruções específicas de uma
stack nos perfis gerais. Convenções de um projeto devem ficar no projeto.

## Como verificar uma mudança

1. Confirme que os links locais apontam para arquivos e seções existentes.
2. Valide a sintaxe do YAML dos perfis e dos TOML quando eles forem alterados.
3. Confira comandos e caminhos com a documentação oficial da ferramenta.
4. Para mudanças de comportamento, experimente uma tarefa pequena que você conhece,
   se houver um cliente autenticado disponível. Compare a versão anterior e a nova
   com o mesmo contexto, modelo e permissões.
5. Registre o resultado real e o que não foi possível verificar. Uma verificação
   de sintaxe não comprova a descoberta ou o comportamento no cliente.

Exemplos úteis para avaliar o fluxo em um cliente autenticado:

- Um PDF pequeno e legível deve produzir requisitos com origem e um registro;
  um pedido de resumo do mesmo PDF não deve iniciar implementação.
- Uma página essencial ilegível ou uma regra contraditória deve gerar uma dúvida
  específica, sem requisito inventado ou bloqueio das partes independentes.
- Um defeito conhecido deve voltar de Casper para Balthasar, seguido de nova revisão.
  Falhas após a rodada 2 devem permanecer registradas, sem reiniciar a contagem.
- Um pedido de etapa isolada deve respeitar o escopo. Um especialista delegado
  não deve iniciar outro Coordenador ou outros subagentes.
- Uma retomada deve ler o registro e conferir os arquivos atuais. Sem subagentes,
  o relatório deve identificar a execução dos papéis na mesma conversa.

Registre o cliente, o modelo, as ferramentas disponíveis e o que foi observado.
Sem uma execução real, descreva esses cenários como propostos, sem alegar validação
de comportamento. Não é preciso criar uma aplicação ou suíte para mudar documentação.

Ao propor uma mudança, descreva o problema, o comportamento esperado, o ajuste
realizado e a validação. Se uma alteração renomear arquivos ou agentes, atualize
também o roteamento, os adaptadores, os links e os exemplos.
