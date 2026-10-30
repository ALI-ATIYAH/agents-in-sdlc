---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合は、そのガイダンスを Copilot に渡します。[instruction file][instruction-files] では、必要なコードだけでなく、その構成方法も説明できます。フィルタリング機能を実装したので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、コードに適用しましょう。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス単位の instruction file がエージェントにどう渡されるかを確認します。
- コーディング標準に従うよう instruction file を更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントでは、コードの言い換えではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` の export 関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を付けます。該当する場合は、注入可能な `db` 引数も記述します。
- 再利用可能な Astro component では `Props` の契約を文書化し、関連コードが変わったらコメントも更新します。
- 既存のフォーマットと lint のガイダンスを維持します。

instruction file を使うと、Copilot に適切な情報を渡して、こうしたプラクティスに沿って作業できます。

## Instruction file

カスタム指示は、コンテキストや設定を Copilot に渡し、コーディング スタイルや要件を理解させるために使います。好みの規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定のファイル用の指示も作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られる単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、言語、ファイル種別、タスクごとのガイドラインを提供します。

> [!NOTE]
> その他の指示形式とサポート状況は環境によって異なります。特定の形式を利用する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すでに複数の instruction file が含まれています。変更を加える前に、現在の内容を確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。ここでは、指示を適用するファイルを決める glob が設定されています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file やプロジェクト ファイルへのリンクを確認します。大きな指示を小さく再利用しやすいファイルに分け、Copilot に参考例を示せます。

## チームのガイダンスに合わせて指示を更新する

現在のファイルは良い出発点ですが、改善の余地があります。`copilot-instructions.md` を変更し、新しく生成する TypeScript に TSDoc コメントを追加するよう指示しましょう。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します。35 行目付近にあります。
2. 次の行を最後の箇条書きとして追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を読み込ませつつ機能のコンテキストを保つため、フィルタリングの会話を終了して再開します。

1. フィルタリングの会話が開いている場合は `/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   求められた場合はフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction file からコンテキストを得る方法を確認し、新しい標準を機能に適用しました。次は、[再利用できる quality-checks スキルをカスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[awesome-copilot]: https://github.com/github/awesome-copilot
