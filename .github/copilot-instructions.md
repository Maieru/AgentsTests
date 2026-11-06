# Instruções do projeto

Leia e siga o `AGENTS.md` na raiz deste repositório para os acordos compartilhados,
a escolha de papel e as orientações de manutenção.

Os perfis completos estão em `.github/agents/`:

- `melchior.agent.md`: arquitetura e planejamento.
- `balthasar.agent.md`: implementação e correções.
- `casper.agent.md`: QA e revisão.
- `gendo.agent.md`: recebimento de PDF/texto e condução do fluxo completo.

Use o perfil selecionado pelo usuário. Ao trocar de agente, encaminhe os critérios
de aceite, as decisões e as evidências já disponíveis. Não presuma memória de outra
conversa. Para implementar um pedido em PDF ou texto, prefira o perfil Gendo:
ele executa o fluxo completo conforme `AGENTS.md`. Se um especialista receber
uma etapa delegada, deve executar apenas essa etapa e devolver o resultado.
Não reinicie a coordenação dentro de um especialista. Perguntas, resumos e pedidos
de etapa única respeitam seu escopo explícito.
