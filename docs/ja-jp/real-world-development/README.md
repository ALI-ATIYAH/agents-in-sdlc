---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用しましょう。変更の計画、コードの作成、動作のテスト、プル リクエストのレビュー、エージェントを使った作業管理を繰り返し実践します。

## シナリオ

架空の企業 Tailspin Toys で開発者として働きます。この会社は、開発者をテーマにしたボードゲームのクラウドファンディングを提供しています。チームのバックログは GitHub Issue として登録済みなので、すぐに着手できます。アクセシビリティやコーディング標準の改善に加え、フィルタリングやページネーションなどの機能開発も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、どの環境でも利用できます。開発方法に合った環境を選んでください。各ワークショップは個別のセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュ コマンドを扱います。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、キャンバスで共同作業を行い、GitHub Issue とプル リクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を案内し、進行状況を確認して、エージェントが作成したプル リクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat の agent mode、MCP server、カスタムエージェントを利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
