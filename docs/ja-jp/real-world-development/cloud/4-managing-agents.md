---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、Copilot cloud agent にユーザー体験の改善や機能追加を行う 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクの進行状況を監視することも大切です。

GitHub.com の[agents ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがあります。この mission control では、オープンなプル リクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を与えたりできます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

高コントラスト モードの作成をエージェントに割り当てた後、チームはライト モードも追加することにしました。サイトのスタイルと切り替え機能を更新しているため、同時に追加するのが合理的です。高コントラストに加えてライト モードも実装されるよう、エージェントの作業に指示を追加します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状況を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン領域と左側の一覧にあるタスクを確認します。コードベースのドキュメント更新、ゲームを変更する API の生成、Web サイトへの高コントラスト モードの追加など、割り当てたタスクが表示されます。
3. 実行中のタスクを選択し、Copilot が実行した操作を確認します。たとえば、リポジトリのチェックアウト、作業環境の作成、MCP server の設定、タスクを完了するための各種手順などです。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプル リクエスト (PR) のパネルで、PR と変更されたファイルを確認します。

## cloud agent に追加の指示を出す

実行中のタスクを確認したので、高コントラスト モードを作成している Copilot にライト モードも追加するよう依頼します。

1. 高コントラスト モードを追加するセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状況によって異なります。

   ![Accessibility session in mission control](../../../_images/ex6-accessibility-session.png)

2. セットアップが終わり、作業が始まるまで数分待ちます。
3. **Steer active session while Copilot is working** ダイアログで、次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Screenshot of the cloud agent task in the agents page with the steer active session while copilot is working dialog highlighted](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が指示を受け取り、作業に組み込むことを確認します。

## Copilot の作業を続ける

Copilot は更新されたタスクに取り組みます。メッセージを送信したときに実行中だった手順が終わると、新しい依頼が作業に組み込まれます。

処理には数分かかります。その間に、これまでに学んだ内容を振り返りましょう。

## まとめと次のステップ

Copilot agents ページは、Copilot cloud agent のタスクを監視してガイドするための中心的な場所です。進行中および完了したタスクを追跡し、作業内容を確認し、進行中のタスクの範囲を変更したりガイダンスを追加したりできます。

次は、Copilot が作成したプル リクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
