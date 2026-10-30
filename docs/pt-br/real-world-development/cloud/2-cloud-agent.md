---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não enfrentem problemas com dívida técnica. Pode ser a existência de problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de recursos que ficaram paradas no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele gera um PR de rascunho para que um desenvolvedor revise. Assim, é possível delegar tarefas repetitivas, acelerar o desenvolvimento e deixar os desenvolvedores livres para se concentrar em objetivos maiores.

Neste exercício, você vai explorar:

- como personalizar o ambiente para a geração de código;
- como garantir que as operações sejam realizadas com segurança;
- a importância de definir o escopo dos issues com clareza;
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas tarefas de dívida técnica para resolver. Os prestadores de serviço inicialmente contratados para criar o site deixaram a documentação em um estado inadequado — na verdade, você verá que ela está completamente ausente. Como primeira etapa, querem adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Eles gostariam que a página de detalhes de cada jogo mostrasse outros jogos relacionados — títulos da mesma categoria — para que as pessoas continuem explorando. Por enquanto, não precisam de uma implementação refinada; querem algo que possam usar para testes de aceitação da experiência. Isso é um bloqueio no momento, mas há issues de prioridade mais alta.

Esses são exemplos de tarefas que podem ser facilmente adiadas e são ótimas candidatas para atribuição ao agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto você se dedica a outras atividades, permitindo que depois você volte para revisar o trabalho e confirmar se está correto.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de forma semelhante a um desenvolvedor humano. Assim como acontece ao trabalhar com uma pessoa, há várias formas de atribuir tarefas, incluindo [atribuir um issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o andamento, configura um ambiente e começa a trabalhar. Você pode examinar a sessão enquanto ela está em andamento ou depois de concluída. Quando a solução proposta estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo bem definido

O GitHub Copilot não faz mágica: você não pode pular a etapa de pensar. Mesmo operações aparentemente simples podem se tornar complexas quando analisadas em detalhes. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já concluiu o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para dar uma olhada rápida. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções incluídos neste repositório, sem editá-los, para depois identificar seus efeitos.

Abra os arquivos a seguir na interface Web do GitHub ou em um codespace que já esteja em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, principalmente as expectativas sobre convenções TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Ao atribuir o issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe os comentários TSDoc, as convenções TypeScript e os cabeçalhos de comentários no pull request resultante — eles vêm desses arquivos de instruções. Voltaremos a esse ponto ao revisar os PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem do Copilot

A criação de código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a um engenheiro de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo com um [fluxo de trabalho de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, para instalar ferramentas de desenvolvimento e dependências necessárias antes que o agente comece. Esse fluxo já foi configurado para facilitar o laboratório e o aprendizado. Ele dá acesso ao Node.js, às dependências do projeto, aos navegadores para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido para o aplicativo Astro único:

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo parece qualquer outro fluxo de trabalho do GitHub, mas tem alguns pontos importantes:

- Ele contém um único trabalho chamado `copilot-setup-steps`. Esse trabalho é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o fluxo manualmente pela guia **Actions** do repositório. Isso é útil para testar se ele funciona, sem esperar o Copilot executá-lo.

## Adicione documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novos desenvolvedores. Felizmente, o Copilot é muito bom em criar documentação, e este é um issue perfeito para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de novo issue.
4. Selecione **Blank issue** para criar um issue em branco.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar o issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição do Copilot a um issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-gerado, com um título semelhante a `[WIP]: Code lacks documentation`. Se o novo PR ainda não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar alguns minutos para aparecer no PR. O Copilot está criando o ambiente (executando o fluxo de trabalho mencionado anteriormente), analisando o projeto e definindo a melhor abordagem para o problema.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do pull request para baixo. Você verá uma atualização informando que o Copilot começou a trabalhar no issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema. Ele explora o código e entende o estado, pausa para pensar e decidir sobre a abordagem e cria código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem é executar tarefas de forma assíncrona, deixando você livre para outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, escrever código para adicionar recursos ao aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Como já destacamos, uma das grandes vantagens do agente de nuvem do GitHub Copilot é dividir o trabalho para que você possa cuidar de uma tarefa enquanto ele trabalha em outra. A seção de jogos relacionados para a equipe de design talvez não leve muito tempo, mas ainda assim você pode usar esse tempo em outras atividades. Vamos atribuí-la ao agente de nuvem.

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de novo issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina o **Title** como `Show related games on the game details page`.
6. Defina a **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar o issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição do Copilot a um issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá alguns emojis 👀 no primeiro comentário do issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando no issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando diligentemente na segunda solicitação. O agente de nuvem trabalha de maneira semelhante a um engenheiro de software, então você não precisa acompanhá-lo ativamente: basta revisar o resultado depois de concluído. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximas etapas

Nesta lição, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com o agente de nuvem, você pode atribuir issues ao Copilot para execução assíncrona. Use-o para resolver dívida técnica, criar recursos ou ajudar a migrar código entre frameworks.

Você explorou estes conceitos:

- personalizar o ambiente para geração de código;
- garantir que as operações sejam realizadas com segurança;
- definir claramente o escopo dos issues;
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso a instruções personalizadas, que exploramos nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
