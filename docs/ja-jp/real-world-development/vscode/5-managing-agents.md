---
title: "演習 5 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタムエージェント][previous-lesson] |
|:--|

GitHub Copilot を agent mode で使うと、コードベースを調べ、変更案を提示し、ファイルを編集してコマンドを実行します。VS Code の Copilot Chat では、ツールの呼び出しやファイルの編集をリアルタイムで確認できます。差分をインラインでレビューし、変更を個別に採用または取り消し、フォローアップのプロンプトで作業を調整・拡張できます。

この演習では、次のことを行います。

- アクセシビリティ エージェントの作業をリアルタイムで監視します。
- 提案されたファイル編集をレビューして採用します。
- 進行中のセッションに指示を出し、高コントラストに加えてライト モードを追加します。

## シナリオ

[演習 4][previous-lesson]で使ったアクセシビリティ カスタムエージェントは、高コントラスト モードを実装しています。作業中にデザイン チームから、既定のテーマが暗すぎると感じるユーザー向けにライト モードも追加したいという要望がありました。新しい会話を始める代わりに、既存のセッションに追加の作業を依頼します。

## Copilot の進行状況を監視する

Copilot Chat ビューには、エージェントが調べたファイル、実行したコマンド、提案した編集が順に表示されます。各ファイルの差分には **Keep** と **Undo** の操作が表示されます。

1. 演習 4 で使った Copilot Chat ビューに戻ります。
2. エージェントが作業している間、チャットをスクロールしてツールの呼び出しを確認します。ファイルの読み取りや編集、実行を求めるターミナル コマンドが表示されます。
3. ファイル編集の差分が表示されたら、**Keep** で変更を採用し、**Undo** で取り消して再度変更を依頼します。
4. チャット上部の **Files changed** を開き、セッション中にエージェントが変更したファイル全体を確認します。

> [!NOTE]
> ターミナル コマンドの実行などで許可が必要な場合、エージェントは一時停止して **Allow** を表示します。内容を確認し、許可してよいコマンドだけを承認してください。

5. 高コントラスト モードの実装が終わるまで待ちます。最後に変更の概要が表示されます。

## エージェントにライト モードを追加するよう指示する

高コントラスト モードができたら、同じ会話でライト モードの切り替えを追加します。会話を続けると、エージェントは変更済みの component、スタイル、local storage のパターンなど、以前の作業のコンテキストを保持します。

1. Copilot Chat の入力欄に次のプロンプトを送信します。

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. エージェントは既存の変更を確認し、ライト モードの変更案を提示します。
3. 各差分を確認し、**Keep** または **Undo** を選択します。
4. 意図しない方向に進んだ場合は、入力欄の横にある **Stop** で停止してから、追加の説明を送ります。

> [!TIP]
> 作業中のセッションに指示を追加すると、最初からやり直すよりも早く調整できます。たとえば「CSS を簡潔にして、selector の重複ではなく CSS variable を使う」「3 つ目の toggle は追加せず、既存の設定パネルに統合する」「両方の toggle にキーボード ショートカットも追加する」と依頼できます。

## 作業セットをレビューする

変更を commit する前に、エージェントが編集した内容を確認します。

1. VS Code で **Source Control** ビューを開きます。
2. 変更されたファイル一覧を確認します。Astro component、スタイル、関連テストが含まれているはずです。
3. いくつかの差分を開いて確認します。カスタムエージェントのアクセシビリティ パターン、ARIA 属性、キーボード操作、セマンティック HTML、local storage による設定の保持が反映されていることを確認します。
4. 問題がなければ Source Control パネルから変更を stage して commit します。次のレッスンで公開します。

## まとめと次のステップ

Copilot Chat の agent mode でセッションの進行を監視してガイドし、ファイル編集を確認し、変更を採用または取り消し、コンテキストを保ったまま追加の作業を依頼しました。次は、[アクセシビリティの変更をプル リクエストとして公開します][next-lesson]。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [Agent mode を使う][agent-mode]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
