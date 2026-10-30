---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能追加を目的に、GitHub Copilot と複数の作業に取り組みました。コードへのドキュメント追加、デザイン チームが試せる関連ゲーム機能、高コントラストとライト モードの切り替えを含むアクセシビリティ機能を依頼しました。変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot の提案を確認しましょう。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監督なしでタスクを実行します。安全性を確保するため、次のようなセキュリティ上の制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みはコード用ブランチに限られます。
- cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions workflow の実行には、人間による承認が必要です。
- MCP server などの[外部リソースへのアクセスは既定で制限][agent-firewall]されています。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent がコードにドキュメントを追加した最初のプル リクエスト (PR) を確認します。github.com の標準的な PR 画面を使います。

> [!NOTE]
> PR を確認するときに、ファイアウォールによって GitHub Copilot がブロックされたという警告が表示される場合があります。既定で MCP server への接続など外部リソースへのアクセスが制限されているため、これは**想定内**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com でリポジトリを開きます。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は完了まで待ちます。数分かかることがあるので、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc doc comment などのドキュメントを確認します。変更内容は状況によって異なります。TSDoc doc comment、TypeScript 規約、コメント ヘッダーを確認してください。これらは演習 2 の最初に確認したカスタム instruction file に基づいています。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. workflow が承認待ちであることを示す表示を探します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows を選択する画面](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションに workflow が追加されます。問題がなければ、単一 Astro app のプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チーム メンバーと同じように、PR やコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確定的ではないため、必ず使うべき文面を指定することはできません。たとえば、次のような更新を依頼できます。

- 各コード ファイルの先頭に、役割を説明するコメント ヘッダーを追加します。
- TypeScript と Astro のファイルに TSDoc doc comment を追加します。
- Astro app の構成を説明する README を作成します。

1. **@copilot** をメンションして、生成されたドキュメントの変更を依頼するコメントを追加します。上記の例や、コードベースのドキュメントに関する別の提案を使えます。
2. **View Session** を選択して Copilot の作業を確認します。更新のために新しいセッションが始まります。
3. **Back to pull request** を選択して PR に戻れます。

   ![プル リクエストに戻る操作](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わると、PR に新しい commit が追加されます。
5. **Files changed** タブで変更をレビューします。

満足するまで改善を繰り返せます。準備ができたら、PR を draft からレビュー可能な状態にして `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR を確認します。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. workflow が承認待ちであることを示す表示を探します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows を選択する画面](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションに workflow が追加されます。単一 Astro app のプロジェクト チェックが成功するまで待ちます。数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace でターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。cloud agent には、表示件数やコメント ヘッダー、TSDoc doc comment の追加など、変更を繰り返し依頼できます。この作業はカスタム指示を更新する**前**に割り当てたため、その点も確認しましょう。前と同じように **Conversation** タブにコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタムエージェントが実装した機能をレビューします。この PR には、演習 3 で依頼した高コントラスト モードと、演習 4 の mission control で追加を依頼したライト モードの両方が含まれているはずです。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は完了まで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 次の点に注意して実装を確認します。
   - モード切り替え用の UI component
   - ユーザー設定を保持する local storage の利用
   - 高コントラスト モードとライト モードの CSS またはスタイル変更
   - アクセシビリティ属性 (ARIA label、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. workflow が承認待ちであることを示す表示を探します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows を選択する画面](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションに workflow が追加されます。単一 Astro app のプロジェクト チェックが成功するまで待ちます。数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。ターミナルを開き、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリケーションを起動して、ブラウザーで高コントラストとライト モードの切り替えを確認します。期待どおりに動作し、ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ カスタムエージェントは、アクセシビリティのベスト プラクティスに沿って機能を実装するよう Copilot をガイドしました。懸念点や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

実際のバックログ項目を cloud agent に任せ、その間に別の作業へ取り組むと効果的です。リポジトリに Issue を追加して Copilot に割り当て、この習慣を身につけましょう。たとえば次の作業があります。

- ゲーム詳細ページに支援者向け問い合わせフォームを作成します。
- ゲーム一覧ページにページネーションを実装します。
- データ アクセス helper に入力検証とエラー処理を追加します。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- リポジトリに含まれるカスタム instruction file を確認し、cloud agent の出力に与える影響を確認しました。
- Issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタムエージェントを作成して使い、高コントラストとライト モードを追加しました。
- Copilot agents ページを mission control として使い、進行中のアクセシビリティ セッションを監視してガイドしました。
- cloud agent のプル リクエストをレビューして改善し、`@copilot` に変更を依頼して workflow を承認しました。

## レビューと次のステップ

Cloud agent ハーネスは完了です。さらに学びたい場合は、ほかのハーネスで関連するシナリオを別の環境から確認できます。

- **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP 統合を試します。
- **[CLI ハーネス](../../cli/)** — Plan、Autopilot、agent skill、QA カスタムエージェント、Playwright MCP、Agent Merge、任意の `/delegate` による cloud agent への連携を使って、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリでは、次の作業も試せます。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てます。
- 別の分野向けのカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
