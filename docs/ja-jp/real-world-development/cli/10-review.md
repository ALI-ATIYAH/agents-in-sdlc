---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、フィルタリングの Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージしました。
- フィルタリングの Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証の根拠を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリングの PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

ワークショップには、更新済みの `main` からそれぞれブランチを作成する、2 つの PR の節目があります。

1. **星評価**: 既存の `starRating` と未評価の場合の表示をゲーム カードに追加します。
2. **フィルタリングと品質ワークフロー**: フィルタリング機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルと関連テストを追加します。

フィルタリングの計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を 1 つの PR にまとめています。

## 検証方法の違い

自動テスト、自分で行うブラウザーの確認、MCP を通じた Copilot のブラウザー確認という複数の方法でコードを検証しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果に要件とテスト カバレッジのレビューを加えました。

追加するテストは、実際に不足しているカバレッジを補う必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前にコードと根拠を確認し、変更後は関連する検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業結果を左右します。このワークショップでは、指示の更新、スキルのカスタマイズ、QA プロファイルの作成、MCP server の設定を行いました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行できるタスクをまとめ、カスタムエージェントは専門的な役割を定義し、MCP server は外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った **モードとモデル** を選びます。実装前の検討には **Plan**、変更を確認しながら進めるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

ツールや環境と同様に、コンテキストも重要です。*何を*作るのか、*なぜ*必要なのか、*どのように*作るのかを明確に説明すると、出力が大きく変わります。

## さらに確認する

主なワークフローは完了しました。次の CLI 機能も確認できます。

- `/review` でコードレビュー エージェントに変更を分析させます。
- `/rubber-duck` で問題を相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して処理します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールの使い方を上達させるには、実際に使い続けることが一番です。本番コード、個人のプロジェクト、作りたいと思いながら手を付けられなかった小さなアプリなどで活用しましょう。チームと学びを共有し、互いの知見から学んでください。ドキュメントも引き続き確認しましょう。

ターミナルから Tailspin Toys を拡張するには、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境で試す場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
