---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューし、プル リクエストを作成しましょう。星評価のプル リクエスト (PR) は手動でマージしました。今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、フィルタリングと品質に関する一連の作業を 1 つの会話とブランチにまとめ、この PR に含めています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルの自動化方法を確認します。
- 機能全体の変更と検証の根拠を確認します。
- フィルタリングの PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能の開発では、Copilot を使って計画、実装、検証を進めてきました。Tailspin Toys では、マージの承認権限を開発者が保ったまま、残りの PR 作業を自動化します。

## Agent Merge とは

**Agent Merge** は、プル リクエストを取り込むために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題 (失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じた rebase など) を処理します。リポジトリで許可されている場合は、GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択しました。Agent Merge にその作業を任せることもできますが、有効にする前に変更をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードがそろったら、全体をレビューして PR を作成し、残りの作業を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージが実行されます。
7. 「PR #14 was squash-merged successfully.」のような Copilot のメッセージを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は理由を確認し、リポジトリで許可される範囲で、レビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プル リクエストの処理を自動化しました。続いて、[Copilot CLI の便利なスラッシュ コマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプル リクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
