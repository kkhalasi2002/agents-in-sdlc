---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app で Pages workflow を設定し、プル リクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages で Space Quiz をデプロイして、誰でもアクセスできるようにしましょう。Copilot は workflow の準備とデプロイの案内を行いますが、変更をレビューし、サイトが公開されたことを確認するのは自分の役割です。

このレッスンでは、次のことを行います。

- GitHub Pages へのデプロイを Copilot に依頼します。
- マージ前に workflow とプル リクエストをレビューします。
- デプロイを確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、利用者が簡単に試せる場所が必要です。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーや実行時の依存関係を追加せずに公開 URL からアクセスできるようになります。

## Copilot にデプロイの準備を依頼する

> [!WARNING]
> プル リクエストを作成すると、コードの commit と push が行われる場合があります。このプロンプトを送る前に、承認設定と送信先のリポジトリを確認してください。**Approve all** を選択している場合、個別の確認は表示されないことがあります。

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## Workflow とプル リクエストをレビューする

1. README が、リポジトリにまだない場合にのみ追加されることを確認します。
2. workflow の build とデプロイ手順、trigger、権限など、プル リクエストの変更をレビューします。
3. workflow が、マージされていないプル リクエストではなく、既定のブランチからデプロイすることを確認します。
4. 修正を依頼し、更新された差分をレビューします。マージを許可する前に、必須のプル リクエスト チェックが成功するまで待ちます。

## マージとデプロイを許可する

> [!WARNING]
> 次のプロンプトは、レビュー済みのプル リクエストのマージ、リポジトリの Pages 設定の変更、サイトの公開をエージェントに許可します。レビューを完了し、リポジトリとプル リクエストを確認してから送信してください。承認設定によっては、別の確認を求められずに操作が実行されます。

`<PR_NUMBER>` をレビューしたプル リクエストの番号に置き換えてから、別のプロンプトとして送信します。

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![3 段階の図: マージ済みの space-quiz プル リクエスト、Build website・Upload artifact・Deploy to Pages の各手順が成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ で公開された Space Quiz](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを検証する

1. GitHub Actions のデプロイ実行が成功するまで確認します。
2. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化やプル リクエストのマージを実行できない場合は、案内された手順に従います。デプロイが成功し、公開サイトが読み込まれるまで、作業を完了したと判断しないでください。

## まとめと次のステップ

Space Quiz が公開され、エージェントの報告を信頼するだけでなく、デプロイを自分で確認できました。[レッスン 13: レビューと次のステップ][next-lesson]に進んでください。

[next-lesson]: ../13-review/
