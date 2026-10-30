---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para combinar uma abordagem, Autopilot para implementar a filtragem a partir de um issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. Alterações maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para funcionar com o fluxo de trabalho existente da organização e ajudar a criar os recursos certos do jeito certo. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento orientado por agente: começará por um issue para criar um novo recurso, verificará se o código é válido e se o recurso funciona como esperado e, por fim, fará a mesclagem ao projeto.

> [!NOTE]
> Você usará a mesma conversa e branch para continuar o fluxo de trabalho do recurso. Normalmente, haveria branches ou PRs separados para os diferentes tipos de arquivo, mas neste workshop faremos um atalho para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma nova conversa do Copilot a partir de um issue do GitHub;
- definir requisitos no modo Plan;
- implementar o recurso no modo Autopilot;
- revisar o código;
- validar o recurso manualmente em um navegador encaminhado.

À medida que avançar, você atualizará as instruções do repositório, personalizará a habilidade `quality-checks`, adicionará validação MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. O issue do backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser definidos antes de começar a programar. Você usará o modo Plan para decidir esses pontos e, em seguida, autorizará uma implementação com escopo definido pelo Autopilot.

## Contexto

A introdução de agentes de programação de IA ao fluxo de desenvolvimento não muda os fundamentos — na verdade, torna-os ainda mais importantes. Muitos desenvolvedores seguem um processo semelhante a este:

1. Começar com um issue registrado que descreve o trabalho.
2. Criar um plano do que precisa ser implementado.
3. Criar e revisar o código.
4. Executar testes para validá-lo.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, mesclar o código.

> [!NOTE]
> Os detalhes exatos variam entre equipes e organizações, mas a maioria segue uma variação desse processo.

Seguir essa abordagem ajuda a garantir que o código gerado por IA cumpra os requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham juntos. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria primeiro um plano e não pode editar arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e depois use Autopilot para implementá-lo.

## Comece pelo issue

Antes de começar a filtragem, volte ao Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se estiver parado, reinicie-o.
2. Confirme que o PR de avaliação por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todos os issues.
8. Use as setas para destacar o issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar o issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número semelhante). O caractere `#` permite trazer um issue ou pull request (PR) do GitHub para o contexto.

## Planeje o recurso de filtragem

O planejamento permite definir a abordagem para implementar um recurso ou executar uma tarefa antes de delegá-la ao Copilot. É recomendável planejar um pouco qualquer trabalho complexo. Vamos alternar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar para o modo Plan. Confirme que o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência ao issue adicionada na etapa anterior, digite:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a criar o plano, explorando o projeto e determinando a melhor abordagem.

3. Durante o processo, o Copilot poderá fazer perguntas sobre o funcionamento da filtragem. Responda de acordo com suas preferências. Não há respostas certas ou erradas.
4. Depois que o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano e revise-o.
6. Peça ao Copilot para revisar qualquer parte que não corresponda às suas decisões.

## Aprove o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo. Vamos deixar o Copilot trabalhar usando o modo Autopilot.

O Autopilot permite que o Copilot itere no problema até considerar a tarefa concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou uma opção com nome semelhante na versão instalada.
2. Confirme que o indicador abaixo do prompt mostra **Autopilot**.
3. Acompanhe o Copilot seguindo o plano, gerando código e executando testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Assim que o código for gerado, revise-o antes de mesclá-lo, como faria com qualquer outro código. Vamos revisar o código e executar o site para confirmar que tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com o issue e as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da exibição de diferenças.
5. Revise a saída das verificações do projeto e peça ao Copilot que resolva as falhas.

## Explore a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros de categoria e editora e as combinações acordadas no plano.
4. Confirme que a redefinição dos filtros e os resultados vazios funcionam de acordo com o issue e suas decisões.
5. Volte ao Codespace e peça ao Copilot que pare o servidor de desenvolvimento iniciado.

## Resumo e próximas etapas

Você usou diferentes modos de conversa para criar e revisar um recurso. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de um issue do GitHub;
- definiu requisitos no modo Plan;
- implementou o recurso no modo Autopilot;
- revisou o código;
- validou manualmente o recurso em um navegador encaminhado.

Em seguida, vamos explorar melhor como o código é gerado e garantir que siga os padrões documentados, [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
