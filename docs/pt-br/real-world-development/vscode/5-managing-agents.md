---
title: "Exercício 5 - Monitoramento e gerenciamento de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Quando você usa o modo de agente do GitHub Copilot, ele trabalha de forma autônoma — explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra as chamadas de ferramentas e as edições de arquivo enquanto ocorrem. Você pode revisar cada diferença no editor, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts de acompanhamento para refinar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade;
- revisar e aceitar as edições propostas nos arquivos;
- orientar o agente durante a sessão para adicionar uma alternância para o modo claro junto ao alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design fez uma solicitação adicional: também gostaria de uma alternância para o modo claro, destinada a usuários que consideram o tema padrão escuro demais. Em vez de começar uma nova conversa, você orientará a sessão existente para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A exibição do Copilot Chat mostra cada etapa do agente — os arquivos que ele explora, os comandos que executa e as edições que propõe. Cada edição aparece no chat com uma diferença e os controles **Keep** e **Undo**.

1. Volte à exibição do Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra o chat para acompanhar cada chamada de ferramenta. Você verá leituras e edições de arquivos e os comandos de terminal cuja execução ele solicitar.
3. Quando o agente propuser uma edição, a diferença aparecerá no chat:
    - Selecione **Keep** para aceitar a alteração.
    - Selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos que o agente modificou nesta sessão.

> [!NOTE]
> Se uma chamada de ferramenta solicitar permissão (por exemplo, para executar um comando de terminal), o agente pausará e mostrará uma solicitação **Allow**. Aprove somente os comandos com os quais você se sente confortável.

5. Aguarde o agente terminar de implementar o modo de alto contraste. Uma mensagem final resumirá as alterações feitas.

## Oriente o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, você ampliará a mesma conversa para adicionar uma alternância para o modo claro. Como continuará na mesma sessão de chat, o agente manterá o contexto do trabalho anterior — incluindo os componentes alterados, a abordagem de estilo e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie o seguinte prompt de acompanhamento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará o trabalho, explorando as alterações já feitas e propondo adições para o modo claro.
3. Revise cada nova edição assim que aparecer, usando **Keep** ou **Undo** em cada diferença.
4. Se o agente seguir uma direção indesejada — por exemplo, se tentar refatorar a interface das alternâncias de um jeito que você não prefere —, você poderá interrompê-lo com o botão **Stop** ao lado do campo de entrada e enviar uma mensagem de esclarecimento.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que recomeçar. Use prompts de acompanhamento para pedir ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione uma terceira alternância; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para as duas alternâncias").

## Revise o conjunto de alterações

Antes de fazer commit, revise rapidamente tudo que o agente alterou.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, estilos e testes relacionados, se houver.
3. Abra alguns dos arquivos alterados e examine as diferenças. Confirme se os padrões de acessibilidade do agente personalizado estão presentes — atributos ARIA, navegação pelo teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações pela exibição Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximas etapas

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento — revisando cada edição, aceitando-a ou rejeitando-a e enviando prompts de acompanhamento para ampliar o trabalho sem perder o contexto.

Nesta lição, você explorou:

- como acompanhar o trabalho de um agente em tempo real;
- como aceitar ou rejeitar edições individuais;
- como orientar uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade como um pull request e revisará as alterações produzidas pelo agente personalizado local durante este laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
