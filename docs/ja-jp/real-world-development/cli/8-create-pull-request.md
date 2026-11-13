---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルタリング、instruction、skill の更新、QA profile、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、instruction の更新、skill の更新、quality assurance (QA) profile、テストはすべて 1 つのブランチに保存されています。まとめてレビューして、プル リクエストを作成しましょう。星評価のプル リクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は機能、instruction の更新、skill の更新、QA agent をいくつかの PR に分けます。ワークショップを簡潔に進めるため、フィルタリングと品質確認の一連の作業を 1 つの会話とブランチで進め、この PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージまでの作業を自動化する仕組みを確認します。
- 機能全体の変更と検証の証拠を調べます。
- フィルタリング PR を作成します。
- レビューの後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリングのワークフローを通じて、Copilot を使って機能を計画、実装、検証してきました。Tailspin Toys では、残りの PR 作業を自動化しつつ、マージの承認は開発者が管理したいと考えています。

## Agent Merge について

**Agent Merge** は、プル リクエストをマージ可能な状態にするための残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている項目、たとえば失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要な場合の rebase に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、作業をレビューして問題なくマージできることを確認してください。

## Agent Merge で PR を管理する

コードが揃ったので、まとめてレビューし、PR を作成して、残りの作業を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. コマンド `/pr create` を使って新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべて成功すると、マージが実行されます。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されていればレビュー後に手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プル リクエストの処理など、開発プロセスの複数の作業を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージまでの作業を自動化する仕組みを確認しました。
- 機能全体の変更と検証の証拠を調べました。
- フィルタリング PR を作成しました。
- レビューの後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウド委任に使える [Copilot CLI の便利な slash command を確認します][next-lesson]。

## リソース

- [Copilot CLI でプル リクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
