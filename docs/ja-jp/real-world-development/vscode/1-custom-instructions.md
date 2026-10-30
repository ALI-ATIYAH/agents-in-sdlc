---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に把握してほしい背景情報がある場合は、そのコンテキストを利用できるようにします。Copilot に具体的なコンテキストを渡す方法はいくつかあります。中でも重要なのが、コード生成のガイダンスを伝える [instruction file][instruction-files] です。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の instruction file を使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- *現在の*指示で、フィルタリングに使う最初のデータ スライス (publishers helper) を生成します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認します。
- 指示の更新とフィルタリングの基盤を `main` に commit し、次の演習で Copilot が最新のガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は非決定的に動作します。この演習の目的は、指示の更新後に動作がどう改善するかを確認することであり、出力を 1 文字ずつ一致させることではありません。

## Instruction file

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データ レイヤーには必ず unit test を用意します。
- UI はダーク モードにし、現代的な見た目にします。
- コード内のドキュメントには TSDoc の doc comment を使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

instruction file を使うと、こうしたプラクティスに沿って作業するために必要な情報を Copilot に渡せます。

### カスタム指示

カスタム指示は、コンテキストや設定を Copilot に渡し、コーディング スタイルや要件を理解させるために使います。コーディング規約、ライブラリ、コメントの種類などを指定して、より関連性の高い提案やコードを得られます。リポジトリ全体に適用する指示も、特定のファイル用の指示も作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られる単一の指示ファイルです。技術スタック、開発中のものの概要、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に適用されます。TypeScript や Astro などの言語、UI component の作成、unit test の追加などに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、instruction file は Copilot Chat のコード生成に使われます。コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は [ほかの標準に基づく instruction file][custom-instructions-support] もサポートしています。AGENTS.md や CLAUDE.md も利用できます。

### Instruction file を管理するためのベスト プラクティス

instruction file の作成方法を詳しく説明することは、このワークショップの範囲外です。サンプル プロジェクトには、代表的な方法が含まれています。

- `copilot-instructions.md` には、開発中のものの説明、プロジェクト構造、グローバルなコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` は、unit test、Astro component、データ レイヤーなどのファイル種別や、特定のタスクに関する具体的な指示に使います。
- 自然言語で明確に書き、コードの望ましい例と避けるべき例を示します。

instruction file の作成にも AI の利用にも、唯一の正解はありません。試しながら、プロジェクトに合った方法を見つけましょう。

> [!TIP]
> GitHub Copilot を使う各プロジェクトに、十分な instruction file を用意しましょう。このプロジェクトでは、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、多くのタスクに対応するファイルがあります。
>
> Copilot に instruction file を生成させることもできます。方法は利用する環境によって異なります。たとえば VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` があります。必要な場合は、利用中の環境に対応するレッスンで説明します。
>
> テンプレートや出発点を探す場合は、instruction file、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる instruction file を確認しましょう。中核となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の **GitHub Actions Workflows** に関するガイダンスも確認してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro、Drizzle データ レイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。ここでは、repo root からの相対 glob を使って適用対象を決めます。たとえば `**/*.test.ts` に一致する TypeScript の test file が対象です。
5. このプロジェクトで unit test を作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾まで読みます。`unit-tests.instructions.md` などの instruction file や、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用しやすい単位に分け、Copilot に参考例を示せます。ここに記載されたパスは repo root ではなく instruction file からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内ドキュメントはまだ必要とされていません。次の手順で TSDoc doc comment とファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前に* Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

まず、現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新し、同じプロンプトを再送信して違いを確認します。

1. Copilot が必要なコンテキストだけを使うよう、前の演習で開いたエディター タブを閉じます。
2. `src/lib/publishers.ts` を開き、helper の配置場所を Copilot に示します。
3. Chat ビューの agent ドロップダウンで **Agent** を選択し、ファイルを編集できるようにします。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べ、helper とテストなどのコードを更新します。
6. 提案された helper が型付き関数で、最初の引数として `db` client を受け取り、型付きの publisher 配列を返すことを確認します。これは `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約 (`src/lib/*.ts` に適用) に基づいています。
7. 提案されたコードに TSDoc doc comment とファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加することがあります。その場合も問題ありません。この演習の目的は、指示を更新した後の一貫性の改善を確認することです。

## リポジトリ全体に新しい標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コーディング標準を文書化して、コード提案の質を高めましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在はコーディング標準が記載されていますが、コード内ドキュメントのルールはありません。
3. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> instruction file は、グローバルなガイダンス用のリポジトリ レベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。今回の doc comment のようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選んで会話を新しくします。
2. `src/lib/publishers.ts` をもう一度開き、Copilot が適切なファイルに注目できるようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案ファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc comment があることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot に指示し、次の演習で使うコードに適用しました。

## 指示とブランチを commit して push する

instruction file もリポジトリの資産の 1 つです。ほかのファイルと同じようにバージョン管理します。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. 次のコマンドで指示と helper の変更を確認します。

   ```bash
   git status
   ```

3. 指示と helper の基盤として Copilot が変更したファイルを stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

ブランチを push したら、バックログにあるドキュメント標準の Issue に関連付けてプル リクエストを作成します。接続済みの GitHub ツールを使って Copilot に Issue を探し、Issue を解決する PR を作成してマージするよう依頼できます。

> [!NOTE]
> この演習では Copilot が初めて GitHub 上で操作します。プロジェクト テンプレートでは GitHub のツールがあらかじめ workspace に接続されています。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインが必要になる場合があります。画面の案内に従ってください。後の演習で Model Context Protocol について説明します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. 次の内容を依頼し、指示の更新と publishers helper の基盤を説明する PR を作成します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. GitHub の操作を許可するよう求められた場合は、コマンドを確認して適切に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージしてローカル ブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作や shell command の許可を求められた場合は、コマンドを確認して適切に **Allow** を選択します。

GitHub Copilot の支援でプル リクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの instruction file から Copilot がコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- *既存の*指示でコード生成のプロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` に新しい標準を追加しました。
- 同じプロンプトを再実行し、提案コードに標準が反映されることを確認しました。
- 指示の更新と helper の基盤を `main` に commit し、次の演習で使えるようにしました。

次は [agent mode][next-lesson] で指示を使い、Copilot にコードベース全体の機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
