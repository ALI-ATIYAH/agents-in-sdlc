---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、作業用の [Codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が動作することを確認します。

## ラボ用リポジトリを設定する

コードを作成するためのリポジトリを用意します。[テンプレート][template-repository]から自分用のリポジトリを作ると、ラボに必要なファイルがそろいます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボ リポジトリで **Use this template** を選択し、続けて **Create a new repository** を選択します。

   ![Use this template button](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントで参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成できます。

   ![Input the repository template settings](../../../_images/ex0-repository-settings.png)

4. ラボの後半で使うため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

> [!NOTE]
> **バックログは準備済みです**
>
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップではこの Issue を使うため、自分で登録する必要はありません。

## Codespace を作成する

次に、Codespace を使って演習を進めます。[GitHub Codespaces][codespaces] は、ブラウザーからコードを記述、実行、デバッグできるクラウド開発環境です。複数の言語、拡張機能、ツールをサポートする IDE を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Select the Code button](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンで新しい Codespace を作成します。

   ![Create a new codespace](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手作業でインストールするよりも早く準備できます。その間に GitHub Copilot の機能を確認しましょう。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。今はブラウザーのタブを閉じずに開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行できます。どちらも必要な前提条件がそろった環境です。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、表示されたら **Reopen in Container** を選択します。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat と agent mode を使う

GitHub Copilot Chat の agent mode を使うには、IDE に GitHub Copilot Chat 拡張機能が必要です。GitHub Codespace を使っている場合は、通常すでにインストールされています。

> [!TIP]
> GitHub Copilot Chat 拡張機能がない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または VS Code の Extensions ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

拡張機能をインストールした後、GitHub アカウントでの認証が必要になる場合があります。

1. Codespace に戻ります。
2. エディター右側に Copilot Chat が表示されていない場合は、Codespace 上部の **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに「Hello world」のようなメッセージを入力して送信します。
4. 認証されていない場合は GitHub アカウントへのサインインが求められます。画面の案内に従って認証します。
5. 認証後、Copilot Chat ウィンドウが表示されます。

![Example of Copilot Chat authentication prompt](../../../_images/ex1-copilot-authentication.png)

## まとめ

ラボ リポジトリのコピーを作成し、コードを書くための Codespace の作成を始めました。

## 次のステップ

Copilot を使い始めましょう。[演習 1 - カスタム指示][next-lesson]に進み、Copilot にプロジェクトの規約を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
