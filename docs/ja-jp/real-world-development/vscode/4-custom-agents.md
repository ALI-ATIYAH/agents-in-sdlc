---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` にある Markdown ファイルでエージェントを定義し、重点的な指示、ベスト プラクティス、コーディング パターン、専門知識を Copilot に渡します。チームの知識を、アクセシビリティ、セキュリティ、テストなどの再利用可能なエージェントにまとめられます。

各ファイルには少なくとも `name` と `description` を含む YAML frontmatter を記述し、その後に動作や指示を定義する Markdown のプロンプトを続けます。プロジェクト内の `.github/agents` のほか、グローバルな `~/.copilot/agents` にも定義できます。

### カスタムエージェントと agent skill の違い

カスタムエージェントと [agent skill][agent-skills-concept] はどちらも、AI に作業方法を伝える Markdown ファイルで定義できます。簡単に言うと、**カスタムエージェントは作業者、スキルはツール**です。カスタムエージェントは独自のコンテキストを持ち、スキルやほかのエージェントを調整して作業します。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず誰もが利用できるクラウドファンディング プラットフォームを目指しています。現在のダーク テーマは文字と背景のコントラストが不十分で読みにくい、というフィードバックを受けました。チームは、ユーザーが切り替えられる高コントラスト モードを求めています。アクセシビリティ エージェントを使って機能を実装しましょう。

この演習では、既存のアクセシビリティ エージェントを確認し、Copilot Chat で使って高コントラスト モードを実装します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに用意されています。Codespace に戻り、ターミナルを開いて、フィルタリングの PR とは別に、`main` からアクセシビリティ用のブランチを作成します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必要です。

3. アクセシブルな Web サイトのコード生成に関する責務、ベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題を確認します。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にある agent ドロップダウンには、`.github/agents` に定義したカスタムエージェントが表示されます。エージェントを選ぶと、その指示とツールに合わせてチャット セッションが設定されます。

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューで agent ドロップダウンから **Agent** を選択します。

   ![Screenshot showing the agent picker in the Chat view.](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にある agent ドロップダウンを開きます。既定では **default** が選択されています。
3. 一覧から **Accessibility agent** を選択します。
4. 次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調査し、変更案を提示してファイルを編集します。編集内容は、ファイル パスと差分とともにチャット内に表示されます。
6. **Files changed** の表示で、エージェントが変更した作業セットを確認します。

> [!NOTE]
> 実際のリポジトリに変更が加わるため、Astro component、CSS、関連テストなどが編集されます。数分かかることがあります。次の演習では、進行中の作業をレビューし、追加の指示を出します。

## まとめと次のステップ

カスタムエージェントは、特定のタスクや分野に合わせて GitHub Copilot をガイドする専門的な AI アシスタントです。次は、同じセッションで[エージェントの作業を監視してガイドします][next-lesson]。変更を確認しながら、ライト モードの切り替えも追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP server で機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
