---
title: "レッスン 1 - GitHub Copilot app のインストール"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

[**GitHub Copilot app**][about-copilot-app] は、エージェント主導の開発に使うデスクトップアプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。自分ですべての作業を行うのではなく、それぞれ分離されたワークスペースで複数のエージェントに並列作業を指示し、繰り返し作業を自動化するワークフロー向けに設計されています。Node.js のインストールとプロジェクトのコピーが完了したので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートによって用意されたバックログを含め、ワークスペースを確認します。
- クイックチャットを試して、アプリ自体について学びます。

## シナリオ

チームは、増え続けるバックログに対応するために AI エージェントを導入しています。Copilot app では、Issue の選択、エージェントの実行、変更のレビュー、pull request のマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象の Copilot プランが必要です。Copilot Free、Copilot Student、またはいずれかの有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise を使用している場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使うには、まずアプリをインストールします。Windows、macOS、Linux 向けのバージョンが用意されています。アプリをインストールして認証し、Tailspin Toys リポジトリを追加しましょう。

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ランディングページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って GitHub.com または GHE.com のアカウントでサインインします。データ所在地に対応した GitHub Enterprise Cloud を使用している場合は **GitHub Enterprise Cloud** を選択し、求められたら `*.ghe.com` のホスト名を入力します。

   GitHub Enterprise Server を使用している場合は、まずクラウドアカウントでオンボーディングを完了します。その後、**Settings** を開き、**Accounts**、**Add account**、**GitHub Enterprise Server** の順に選択します。[Enterprise Server のサインインガイド][ghes-sign-in]に従って、ブラウザーでアプリを承認します。

5. オンボーディングでリポジトリを選択する画面が表示されたら、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリの選択をスキップし、オンボーディング後に追加することもできます。
6. テーマを選ぶよう求められたら選択し、オンボーディングを完了します。
7. サイドバーの **Sessions** の下にリポジトリが表示されていることを確認します。すでに表示されていれば、再度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた所有者名とリポジトリ名を指定します。
9. リポジトリがサイドバーに表示されていることを確認します。このレイアウトでは、`tailspin-toys` などのリポジトリフォルダーが **Sessions** の下にまとめられ、その下に個々のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、アプリの使い方を確認しましょう。サイドバーはいくつかの領域で構成されています。

- **New** - ここから Copilot との新しいチャットセッションを開始できます。
- **Pull requests** - アプリの GitHub ネイティブ統合を通じて pull request が表示されます。アプリを離れずに pull request の参照や絞り込み、CI ステータスの確認、レビューを行えます。
- **Issues** - 同じように Issue が表示されます。Issue の参照や絞り込み、Issue からのセッション開始を行えます。
- **Automations** - スケジュールまたはオンデマンドで実行する、保存済みのエージェントタスクです。やることリストの管理、定期的なプロジェクト保守、その他の面倒な作業を任せるのに便利です。まとめでは次のステップとして紹介しますが、ワークショップの演習には含まれません。
- **Customize** - MCP server、プラグイン、スキルなどを追加して Copilot app の機能を拡張します。Playwright MCP の設定に使用します。
- **Chats** - 独自のブランチやワークスペースを必要としない、質問やブレインストーミング向けの簡易的な会話です。最初のチャットを開始すると表示されます。表示されていない場合は、**Sessions** の横の **+** を選択して最初のチャットを開始します。
- **Sessions** - リポジトリフォルダーと、そのエージェントセッションをまとめます。`tailspin-toys` などのリポジトリ名を選択するとリポジトリビューが開き、その下のセッションを選択すると会話に戻ります。セッションごとに独立したワークスペースで実行されるため、変更を競合させずに複数のセッションを同時に実行できます。最初のセッションは、星評価を追加するレッスンで開始します。

ワークショップを進めながら、ワークスペースを確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問しましょう。操作方法がわからない場合や、何かが可能かどうか知りたい場合は、Copilot が案内してくれます。

### 用意されたバックログを確認する

バックログのないプロジェクトはほとんどなく、Tailspin Toys も例外ではありません。テンプレートからリポジトリを作成したときに用意されたバックログを確認しましょう。

1. サイドバーの **Sessions** で Tailspin Toys のリポジトリフォルダーを選択します。フォルダーの下にある個別のセッションではありません。次に、リポジトリの上部ナビゲーションで **Issues** を選択します。
2. Issue 番号を決めつけず、タイトルで次の Issue を探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して詳細を読みます。各 Issue はエージェントセッションの開始点にもなります。最初の小さな変更を終えた後、フィルター機能の Issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧は自動的に絞り込まれ、Copilot app に追加したリポジトリの項目だけが表示されます。ほかのリポジトリの作業項目を表示するには、そのリポジトリをアプリに追加してください。

## クイックチャットを試す

アプリに慣れるには、アプリ自体について質問するのが効果的です。その用途には **クイックチャット**が適しています。クイックチャットでは、ブランチや worktree を作成せずに質問やブレインストーミングができます。セッションを必要としない、その場限りの簡単な質問に最適です。

1. サイドバーで **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。その後 **Chats** が表示されます。
2. アプリのセッションがどのように動作するかを尋ねます。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが独立した git worktree で実行されるため、変更を競合させずに複数のエージェントを並列実行できることがわかります。会話はいつでも続けられ、新しいチャットも開始できます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次のことを学びました。

- アプリをインストールして GitHub にサインインする。
- GitHub リポジトリからプロジェクトを追加する。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つける。
- クイックチャットで、その場限りの簡単な質問をする。

次は、[最初のエージェントセッションを開始][next-lesson]し、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の概要][getting-started]
- [GitHub Copilot app でのエージェントセッションの操作][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
