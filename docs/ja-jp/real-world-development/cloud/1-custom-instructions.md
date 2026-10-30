---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に把握してほしい背景情報がある場合は、その情報にアクセスできるようにします。[instruction file][instruction-files] でガイダンスを伝えると、Copilot は何をするかだけでなく、どのように進めるかも理解できます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の instruction file を使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期に作業するため、影響をリアルタイムで並べて示すのが困難です。後続の演習で cloud agent が作成するプル リクエストをレビューするときに、指示ファイルの影響を確認します。

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
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は [ほかの標準に基づく instruction file][custom-instructions-support] もサポートしています。AGENTS.md や CLAUDE.md も利用できます。

### Instruction file を管理するためのベスト プラクティス

instruction file の作成方法を詳しく説明することは、このワークショップの範囲外です。サンプル プロジェクトには、代表的な方法が含まれています。

- `copilot-instructions.md` には、開発中のものの説明、プロジェクト構造、グローバルなコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` は、unit test、Astro component、データ レイヤーなどのファイル種別や、特定のタスクに関する具体的な指示に使います。
- 自然言語で明確に書き、コードの望ましい例と避けるべき例を示します。

instruction file の作成にも AI の利用にも、唯一の正解はありません。試しながら、プロジェクトに合った方法を見つけましょう。

> [!TIP]
> GitHub Copilot を使う各プロジェクトに、十分な instruction file を用意しましょう。このプロジェクトでは、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、多くのタスクに対応するファイルを確認できます。
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
6. `.github/instructions/drizzle.instructions.md` を開いて末尾まで読みます。`unit-tests.instructions.md` などの instruction file や、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用しやすい単位に分け、コード生成時に参考例を示せます。ここに記載されたパスは repo root ではなく instruction file からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内ドキュメントはまだ必要とされていません。次の手順で TSDoc の doc comment とファイル コメント ヘッダーのルールを追加します。

## リポジトリ全体に新しい標準を追加する

この演習で編集するのは、プロジェクト全体の標準として、コード内に TSDoc の doc comment とファイル レベルのコメント ヘッダーを追加するルールです。後続の演習で cloud agent が Issue に取り組むときに、この標準を適用します。

編集の前にブランチを作成します。cloud agent は Issue が対象とするブランチの指示を読み込みますが、変更を整理して commit しておきます。

1. Codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コーディング標準を文書化して、コード提案の質を高めましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在はコーディング標準が記載されていますが、コード内ドキュメントのルールはありません。
3. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc の doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> instruction file は、グローバルなガイダンス用のリポジトリ レベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。今回の doc comment のルールのようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## 指示の変更を commit、push、マージする

cloud agent は、Issue が対象とするブランチの instruction file を読みます。次の演習で Copilot に Issue を割り当てると、cloud agent は `main` からブランチを作成します。そのため、指示を適用するには変更を `main` にマージする必要があります。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプル リクエストを開き、マージします。次の演習で Issue に取り組むとき、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチを作成せず `main` に commit してもかまいません。実際のプロジェクトでの進め方に近づけるため、ここではブランチを使っています。

## まとめと次のステップ

このプロジェクトの instruction file から Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りのハーネスで cloud agent が作成するプル リクエストを確認すると、この標準の適用状況が分かります。

次は、[Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
