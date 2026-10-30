---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を加速し、より重要な作業に集中できます。

この演習では、Copilot cloud agent を使って次のことを確認します。

- コード生成用の環境をカスタマイズする方法
- 安全に操作を実行する方法
- 範囲を明確にした Issue の重要性
- Copilot に Issue を割り当てる方法

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを開発した委託業者はドキュメントを十分に作成していません。まず、アプリケーション内のすべての export 関数に TSDoc の doc comment を追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、閲覧を続けられるようにします。現時点では、受け入れテストで UX を試せる程度の実装でかまいません。この作業はブロッカーですが、優先度がより高い Issue もあります。

こうした作業は優先順位が下がりやすく、Copilot cloud agent に適しています。エージェントに非同期で作業させている間に別の作業を進め、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者のようにバックグラウンドで作業できます。たとえば [GitHub Issue を Copilot に割り当て][assign-issue]ます。割り当て後、Copilot は進捗を追跡するドラフトのプル リクエストを作成し、環境を準備してタスクを開始します。作業中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、プル リクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な作業にも複雑な面があるため、[cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階的に進め、学び、試し、調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と Copilot のガイド方法を確認しました。まだ `.github/copilot-instructions.md` を見ていない場合は、ここで読み取り専用で確認しておきましょう。後で結果をレビューするときに、指示の影響を確認できます。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript 規約と TSDoc doc comment に関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` に適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てるとき、作成されたプル リクエストに TSDoc doc comment、TypeScript 規約、コメント ヘッダーが反映されているか確認してください。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent 用の開発環境を設定する

コードを作るときは、通常、必要な環境とセットアップ用スクリプトを準備します。Copilot にタスクを割り当てる場合も同様で、cloud agent はソフトウェア エンジニアのように作業します。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールと依存関係を準備できます。ラボの進行と学習のため、必要な設定は事前に用意されています。Node.js、プロジェクトの依存関係、end-to-end test 用のブラウザー バイナリ、単一 Astro app 用にマイグレーションと seed が済んだ SQLite データベースを準備します。

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

ほかの GitHub workflow と同様ですが、次の点に注目してください。

- `copilot-setup-steps` という単一の job があります。この job は、Copilot がプル リクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` trigger を使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するのを待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、情報が古かったり、そもそもなかったりするプロジェクトは少なくありません。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成を得意とするため、cloud agent に依頼するのに適した Issue です。バックグラウンドで必要なドキュメントを作成します。次の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細を示す画面](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプル リクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。まだ表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後に Copilot の todo list が作成されます。

> [!NOTE]
> todo list が PR に表示されるまで数分かかることがあります。Copilot は環境を準備し (先ほどのワークフローを実行します)、プロジェクトを分析して、問題への最適な取り組み方を検討しています。

13. todo list と、実行予定のタスクを確認します。
14. プル リクエストのタイムラインを下に進み、Copilot が Issue の作業を開始したことを確認します。
15. **View session** を選択します。

   ![Copilot セッションの表示画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するには、ブラウザーを更新する必要がある場合があります。

16. ライブ セッションをスクロールし、Copilot がコードを調査して状態を把握し、計画を考え、コードを作成する様子を確認します。

作業には数分かかります。Copilot cloud agent の主な目的は、作業を非同期で行い、開発者が別のタスクに集中できるようにすることです。この機能を活用するため、もう 1 つのタスクを割り当ててから、アプリケーションの機能を追加するコード作成に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザイン チーム向けに関連ゲームの表示を追加する作業は短時間でも、別のタスクに使える時間です。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択します。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![Copilot が Issue に取り組んでいることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。cloud agent はソフトウェア エンジニアのように動作するため、積極的に監視する必要はありません。完了後にレビューできます。次はカスタムエージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、Issue を割り当て、非同期に作業させることができます。技術的負債への対応、新機能の作成、フレームワーク間のコード移行などを依頼できます。

この演習では、コード生成環境のカスタマイズ、安全な操作、範囲が明確な Issue の重要性、Copilot への Issue 割り当てを確認しました。cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent は MCP server も利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
