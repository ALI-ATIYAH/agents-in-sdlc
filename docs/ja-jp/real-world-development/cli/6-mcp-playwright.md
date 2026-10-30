---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書く以外にも、データや外部サービスを扱い、Copilot が利用できる自動化機能を追加する必要があります。MCP server を使うと、CLI に組み込まれていないツールやサービスも Copilot から利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での使い方を確認します。
- まだ利用できない場合は Playwright MCP server を追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認します。

## シナリオ

単体テストや end-to-end テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じように開発中の Web サイトを使えるようにすると、変更が期待どおりに動作するかを確かめる作業を自動化でき、信頼性が高まります。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは最新の情報にアクセスし、ユーザーに代わって操作できます。

ツールやリソースには、AI エージェントと外部サービスをつなぐ MCP server を通じてアクセスします。各 MCP server は、エージェントが利用できるツールとリソースのセットを提供します。

よく使われる MCP server の例を次に示します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プル リクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化を利用できます。

ほかにも多くの MCP server があります。GitHub は、エコシステム内の MCP server を見つけやすくし、貢献を促すために [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP server も、ほかの依存関係と同様に扱ってください。利用前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP server を追加する

Playwright MCP server がまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、この手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP server の追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押して server の種類が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して新しい MCP server を保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは機能が期待どおりに動くことを手動で確認しました。次は、Copilot が利用できる Playwright MCP server を使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP server で機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP server を見つけるため、明示的に指定しなくてもかまいません。使うツールが分かっている場合は、指定すると結果の一貫性を高め、トークンを節約できます。

2. Copilot がブラウザーで機能を確認する手順を表示する様子を確認します。
3. レポートを読み、すべてが期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作し、作業後にサーバーを停止して結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP server を使い、実際のブラウザーで機能を確認しました。次は、スキルとブラウザー ツールを組み合わせる [QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP server を追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
