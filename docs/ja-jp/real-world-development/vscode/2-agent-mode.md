---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストなどの操作を実行する必要があります。開発者は、関連ファイルを探し、変更し、テストし、デバッグし、見落としたファイルを見つけて再度変更します。作業は際限なく続くことがあります。

こうした作業に役立つのが Copilot Agent Mode です。

Copilot Agent Mode は IDE 内でより自律的に作業します。開発者と同様に、既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる利用体験を提供します。agent mode は IDE 内で作業し、すばやいフィードバックと対話を可能にします。cloud agent はチーム メンバーのように非同期で作業し、Issue とプル リクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを見つけて、連携した変更を加える方法
- Copilot Agent Mode で UI とデータ レイヤーをまたぐ機能を実装する方法
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前にレビューする方法

## シナリオ

ゲームの一覧が増えてきたため、カテゴリや発行元で絞り込めるようにします。前の演習では publishers helper を作成しました。次は Copilot Agent Mode を使って、残りのデータ レイヤー、UI、テストを仕上げます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲーム一覧と詳細を表示します。単一の Astro app で、ビルド時に静的 HTML としてページを生成します。ページは Drizzle ORM を使って frontmatter からローカル SQLite データベースを直接参照します。独立した backend API や client-side UI framework はありません。再利用可能なデータ アクセス helper は `src/lib/` にあり、標準 DOM API を使う小さな範囲限定の Astro `<script>` で操作性を追加します。

### Web サイトを起動する

単一の Astro dev server を起動する `npm run dev` task が用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習を終えていれば `main` にいます。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. フィルタリング用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro dev server が起動すると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro server のアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost URL のリンクを選ぶと、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは Web server をホストしている Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主なページは次のとおりです。

- **Home Page**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加します。まずバックログを確認しましょう。テンプレートからプロジェクトを作成したときに Issue のバックログも自動作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューの agent dropdown で **Agent** を選択します。**Agent** はファイルをまたぐ変更を自律的に計画・実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップは特定のモデルを推奨するものではありません。作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。体験をそろえるため、提案内容ができるだけ一致することを期待していますが、LLM は確率的に動作するため、ワークショップの記載と異なる提案が出る場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる MCP command を使うことがありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して Issue 一覧を取得するコマンドを実行します。
5. 生成された Issue 一覧を確認します。

Copilot はユーザーに最も役立つと判断した項目に基づいて、優先順位も付けています。

## Instruction file を確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に伝える instruction file を確認しましょう。Web サイトに機能を追加する方法を説明する [user interface (UI) instruction file](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。アーキテクチャの概要、component 設計・テスト容易性・アクセシビリティの原則、Astro、Tailwind CSS、Drizzle などのファイル種別に特化した instruction file へのリンクが含まれます。

> [!TIP]
> instruction file から、別の instruction file やプロジェクト内のファイルを参照できます。パスは instruction file の場所からの相対パスです。指示を再利用しやすい小さな単位に分け、例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングを完成するには、少なくとも次の 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) に残りのフィルタリング ロジックを追加または調整します。
- フィルタリングの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

さらに、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選んで新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選び、instruction file を指定します。

   ![UI instruction file を選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューの agent dropdown で **Agent** が選択されていることを確認します。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データ レイヤー helper、UI、テストを見つけた後、ファイルの編集とテストを始めます。

   ![Copilot がプロジェクト ファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによって数分かかることがあります。作業中、コード エディターに **Keep** と **Undo** が表示される場合があります。完了すると、すべての変更に **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選び、テストを実行します。

   ![Copilot Chat ペインでテスト実行の確認を求めるダイアログを示すスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 途中で一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はタスクが完了し、エラーがないと判断するまでコード生成とテストを繰り返します。

   ![Copilot Agent Mode の Chat セッション全体を示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode で Tailspin Toys の Web サイトに機能を追加しました。Agent Mode が UI とデータ レイヤーをまたぐ機能を実装し、関連ファイルを調べて連携した変更を行うこと、そしてコードベースにマージする前に変更とテストをレビューすることを確認しました。

次は [Playwright MCP server で機能をテストし][next-lesson]、プル リクエストを作成します。

### 追加の探索演習 — ページネーションを実装する

ゲーム一覧の増加に合わせて、ページネーションが必要になります。この演習で学んだ方法を使い、Copilot にページネーションを依頼します。次の点を検討してください。

- 既存の instruction file を使うなど、既存のベスト プラクティスに従います。
- ページ サイズをユーザーが選べるようにするか、固定値にするかなど、ページネーションの動作を決めます。
- 希望する実装に必要なガイダンスを、プロンプトで Copilot に伝えます。
- Copilot に追加のコンテキストを伝え、変更を依頼しながら反復することもあります。これは Copilot を使う通常の進め方です。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
