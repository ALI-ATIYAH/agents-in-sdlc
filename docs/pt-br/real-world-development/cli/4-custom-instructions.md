---
title: "Lição 4 - Oriente o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica, essas orientações precisam estar disponíveis ao Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *que* código você quer, mas também *como* ele deve ser estruturado. Agora que criou o recurso de filtragem, você explorará as instruções usadas pelo Copilot, adicionará um padrão de documentação e o aplicará ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instruções associados a caminhos chegam ao agente;
- atualizar o arquivo de instruções para que os padrões de código sejam seguidos;
- observar o impacto dos arquivos de instruções no código.

## Cenário

A Tailspin Toys tem diretrizes e requisitos para práticas de desenvolvimento:

- Comentários devem explicar a intenção e decisões não óbvias, sem apenas repetir o código.
- Funções exportadas em `db/` e `src/lib/` devem documentar sua finalidade, os parâmetros e os valores retornados com TSDoc/JSDoc, incluindo um argumento `db` injetável quando houver.
- Componentes Astro reutilizáveis devem documentar seus contratos `Props`, e os comentários devem continuar atualizados quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser preservadas.

Com arquivos de instruções, você fornecerá ao Copilot as informações necessárias para realizar o trabalho de acordo com essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor o estilo de código e os requisitos do projeto. Você pode especificar convenções de código, bibliotecas e os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivo e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter contexto do projeto relevante para a maioria das solicitações.
- Arquivos `.github/instructions/*.instructions.md`, que oferecem orientações para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instrução varia entre ambientes. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explore os arquivos de instruções personalizadas deste projeto

O projeto inicial já inclui arquivos de instruções. Vamos explorá-los antes de fazer alterações para observar o impacto.

1. Volte ao Codespace.
2. No editor do Codespaces (não no terminal), abra `.github/copilot-instructions.md`.
3. Examine a descrição do projeto e as orientações de código. Essas instruções se aplicam a toda interação com o Copilot neste repositório.
4. Abra `.github/instructions` e explore os arquivos. Há instruções para Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md`. O campo `applyTo` no início define um padrão que determina quais arquivos são abrangidos.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e arquivos existentes do projeto. Assim, é possível dividir conjuntos grandes em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir.

## Atualize as instruções para refletir as orientações da equipe

Os arquivos existentes são um bom começo, mas ainda falta uma orientação. Vamos modificar `copilot-instructions.md` para exigir comentários TSDoc no novo código TypeScript.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, próxima da linha 35.
2. Adicione a seguinte linha como último item:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Use as orientações atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa é iniciada. Depois de salvar as instruções, saia e retome a conversa de filtragem para que o Copilot carregue as novas orientações sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit` para encerrá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório, na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está selecionada.
3. Peça ao Copilot que aplique as orientações atualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e examine os arquivos TypeScript alterados. Observe os novos comentários TSDoc e confirme que descrevem o código com precisão.

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções e aplicou um novo padrão ao recurso. Em especial, você:

- explorou como as instruções do repositório e os arquivos associados a caminhos chegam ao agente;
- atualizou o arquivo de instruções para garantir que os padrões de código sejam seguidos;
- observou o impacto dos arquivos de instruções no código.

Em seguida, você [personalizará e executará a habilidade reutilizável `quality-checks`][next-lesson] para executar lint e testes de forma consistente.

## Recursos

- [Adicionar instruções personalizadas para o Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
