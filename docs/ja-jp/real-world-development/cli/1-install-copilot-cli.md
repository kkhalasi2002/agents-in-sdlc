---
title: "レッスン 1 - GitHub Copilot CLI のインストール"
description: "Codespace に Copilot CLI をインストールして認証し、基本を確認して、用意されたフィルタリング issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディング アシスタントです。コードベースを調査し、コードを生成し、コマンドを実行し、外部ツールと連携できます。すべてコマンド ラインから行えるため、タスクを任せたり、変更を依頼したりしながら集中して作業できます。最初のステップはツールのインストールです。すでに使い慣れたツールで実行できます。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使ってフィルタリング issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに取り組むため、AI agent を使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルからその機能を利用できるようにします。このレッスンでは、インストールと認証を済ませ、ワークショップの後半で使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js が事前にインストールされているので、このレッスンでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. メッセージが表示されたら、device code の手順に従って認証し、Copilot CLI を許可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選んで「はい」と回答します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI の起動時に認証を求められなければ、そのまま進めてください。

## 基本を確認する

通常の shell prompt で入力したコマンドは Codespace で直接実行されます。Copilot CLI の起動後は、自然言語を agent に送信し、slash command で会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選び、<kbd>Enter</kbd> を押します。もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストールしたバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot が正しく動作していることを確認するため、簡単な質問をします。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot が回答前にリポジトリを調査することを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
6. 次のように依頼して、フィルタリング issue を探します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて issue を読みます。最初の小さな変更の後で、この issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルでチェックアウトしているブランチを使います。自動的に worktree が作成されるわけではありません。各変更の前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は許可された範囲外のツールを使う前に確認を求めます。このワークショップでは、`--yolo` を付けて再起動します。これは Codespace 内で確認プロンプトを省略する、ユーザーが許可したショートカットです。演習に集中して進められます。

> [!CAUTION]
> `--yolo` は完全な自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM のような分離された環境でのみ使い、日常的な開発の既定値として alias に設定しないでください。詳しくは [Allowing and denying tool use][allow-all-warning] を参照してください。

このワークショップでは `--enable-all-github-mcp-tools` を付けると、後のレッスンで issue やプル リクエストを扱うための GitHub MCP 読み取り / 書き込みツールが有効になります。Codespace はローカル コンピューターへのアクセスを制限しますが、認証された GitHub リソースは実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリ root から、もう一度 Copilot CLI を起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問を送り、会話が動作することを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後で instruction を変更したり agent を追加したりした後、`copilot --resume` を実行すると同じ会話とブランチに戻れます。

## まとめと次のステップ

おめでとうございます。このレッスンでは、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーを使ってフィルタリング issue を見つけました。

次は、[最初の対象を絞った変更][next-lesson]に取り組み、Copilot CLI でゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI のインストール][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
