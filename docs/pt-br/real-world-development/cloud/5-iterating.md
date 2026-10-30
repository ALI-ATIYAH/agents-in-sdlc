---
title: "Exercício 5: Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciamento de agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design iterar e implementasse recursos de acessibilidade, incluindo alternâncias para os modos de alto contraste e claro. Vamos explorar as alterações de código e, se necessário, enviar comentários ao Copilot para melhorar o resultado.

### Cenário

Como já destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a chegada da IA generativa. Sempre precisamos revisar o código gerado e seguir nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar a documentação, o recurso de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, foram implementadas algumas restrições de segurança para manter tudo protegido:

- O Copilot tem apenas acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions, que cria um ambiente separado e temporário para o trabalho.
- Todos os fluxos de trabalho do GitHub Actions precisam de aprovação humana antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revise a documentação gerada

Vamos começar pelo primeiro pull request (PR) gerado pelo agente de nuvem do GitHub Copilot: a adição de documentação ao código. Você fará isso na interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao explorar o PR, você talvez veja um aviso de que o GitHub Copilot está bloqueado por um firewall. Isso é **esperado**, pois o Copilot tem acesso limitado a recursos externos por padrão, incluindo chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com título semelhante a **Add missing documentation** ou outro título mais completo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a marca **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos, então aproveite para fazer uma pausa ou refletir sobre o que aprendeu até aqui.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os comentários TSDoc recém-criados e outra documentação. As alterações exatas variam.

    Ao examinar as alterações, procure comentários TSDoc, convenções TypeScript e cabeçalhos de comentários. Eles foram definidos nos arquivos de instruções personalizadas que você revisou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
7. Você verá um indicador de que alguns fluxos de trabalho aguardam aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.

## Solicite alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você também pode mencioná-lo em comentários — assim como faria com outra pessoa da equipe — no pull request ou em comentários de linha do código. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não podemos indicar um texto específico. Algumas ideias de solicitações:

- adicionar cabeçalhos de comentários no início de cada arquivo de código, descrevendo brevemente sua finalidade;
- adicionar comentários TSDoc a arquivos TypeScript e Astro;
- criar um README com uma descrição da estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer usuário. Use uma das ideias acima ou outra sugestão de documentação que gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe como ele inicia uma nova sessão para fazer as atualizações.
3. Você pode selecionar **Back to pull request** para voltar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot terminar as alterações, haverá um novo commit no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você poderá converter o PR de rascunho em pronto para revisão e mesclá-lo à branch principal.

![Converter PR em pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Vamos voltar ao PR que o Copilot gerou para resolver o issue sobre a exibição de jogos relacionados na página de detalhes de cada jogo.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page** ou outro título mais completo.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
6. Você verá um indicador de que alguns fluxos de trabalho aguardam aprovação.
7. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Os fluxos de trabalho deverão entrar na fila na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** Você também pode alternar para esta branch no Codespace para testar manualmente o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode iterar com o agente de nuvem do Copilot e pedir atualizações. Por exemplo, solicite um ajuste na quantidade de jogos relacionados ou cabeçalhos de comentários e comentários TSDoc (lembre-se: a tarefa foi atribuída **antes** da atualização das instruções personalizadas). Assim como nos PRs anteriores, basta adicionar um novo comentário na guia **Conversation** para que o Copilot o veja e inicie uma sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Este PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no centro de controle no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com título semelhante a **Add high contrast mode to website** ou outro título mais completo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a marca **[WIP]**. Nesse caso, aguarde a conclusão do trabalho, que pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes da interface para alternar entre os modos;
   - uso do armazenamento local para manter as preferências;
   - alterações de CSS ou estilo para os modos de alto contraste e claro;
   - atributos de acessibilidade (rótulos ARIA, navegação pelo teclado etc.);
   - código JavaScript/TypeScript que gerencia a alternância entre modos.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
7. Você verá um indicador de que alguns fluxos de trabalho aguardam aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** Você pode alternar para esta branch no Codespace para testar os recursos de acessibilidade manualmente. Acesse o Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os modos de alto contraste e claro no navegador para confirmar se funcionam como esperado e se a preferência continua ativa após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot na implementação desses recursos de acordo com as práticas recomendadas de acessibilidade. Se notar algum problema ou oportunidade de melhoria, mencione **@copilot** em um comentário para solicitar alterações, como nos PRs anteriores.

## Exercício opcional — continue delegando

O agente de nuvem funciona melhor quando você pode delegar itens reais do backlog e se dedicar a outras atividades. Para desenvolver esse hábito, crie mais alguns issues no seu repositório e atribua-os ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo;
- implementar paginação na lista de jogos;
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu a trilha do agente de nuvem. Ao longo destas lições, você:

- **inspecionou os arquivos de instruções personalizadas incluídos no repositório** para ver seus efeitos no resultado do agente de nuvem;
- **atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona;
- **criou e usou um agente personalizado** para acessibilidade, adicionando alternâncias para os modos de alto contraste e claro;
- **usou a página de agentes do Copilot como centro de controle** para monitorar e orientar a sessão de acessibilidade;
- **revisou os pull requests do agente de nuvem e iterou sobre eles**, mencionando `@copilot` para solicitar alterações e aprovando fluxos de trabalho.

## Revisão e próximas etapas

Você concluiu a trilha do agente de nuvem. Se quiser continuar aprendendo, as outras trilhas complementam o que praticou:

- 🖥️ **[Trilha do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Trilha do CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, habilidades de agente, um agente de QA personalizado, Playwright MCP, Agent Merge e uma ponte `/delegate` opcional de volta ao agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- atribua ao agente de nuvem um issue de refatoração ou de cobertura de testes;
- crie um agente personalizado para outro domínio;
- configure `copilot-setup-steps.yml` para outra pilha de tecnologia.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciamento de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
