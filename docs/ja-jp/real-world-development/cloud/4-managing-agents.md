---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの 2 つの演習では、Copilot cloud agent にユーザー エクスペリエンスの改善と機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の [agents page][agents-page] など、さまざまなツールで管理できます。この mission control では、未完了のプル リクエスト (PR) があるエージェント タスクをすべて確認できます。実行された操作を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次のことを行います。

- agents page で cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイ コントラスト モードの作成をエージェントに依頼した後、チームはライト モードも追加するよい機会だと考えました。サイトのスタイルを更新し、切り替え機能を追加する作業をすでに進めているため、ライト モードも含めるのが自然です。エージェントの作業を調整して、ハイ コントラストに加えてライト モードも追加します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents page](https://github.com/copilot/agents) を開きます。
2. メイン ペインと左側のペインにあるタスク一覧を確認します。Copilot に割り当てた次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを変更するための API を生成する。
   - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを選択します。Copilot が実行した次のような作業を確認します。
   - リポジトリからコードをチェックアウトする。
   - Copilot の作業環境を作成する。
   - MCP サーバーをセットアップする。
   - 割り当てられたタスクを完了するためにさまざまな手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプル リクエスト (PR) ペインも確認します。ここから PR と変更されたファイルを確認し、作業を監視できます。

## Cloud agent に追加の指示を出す

実行中のタスクを確認できたので、エージェントがハイ コントラスト モードを作成している間に、ライト モードの切り替えも追加するよう依頼します。

1. ハイ コントラスト モードを追加しているセッションを選択します。Copilot が付けた名前や作業状況によって、タイトルは異なります。

   ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まるまで数分間セッションを確認します。次のようなメッセージが表示されたら、作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Agents page の cloud agent タスクで、作業中のセッションへの追加指示ダイアログを強調表示したスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に取り込むことを確認します。

## Copilot に作業を任せる

先ほどと同じように、更新されたタスクに Copilot が取り組みます。メッセージを送信した時点で実行中だった手順が終わると、新しい依頼が作業に反映されます。

今回も完了まで数分かかります。ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視して案内する中心的な場所である Copilot agents page を確認しました。この mission control では、進行中および完了したタスクを追跡し、実行中の作業をレビューし、必要に応じて進行中のタスクに別の指示を出して範囲を調整できます。

次の概念を確認しました。

- mission control と agents page で cloud agent のタスクを監視する。
- 進行中のセッションに追加機能を依頼する。

Copilot がアクセシビリティ機能を作成している間に、Copilot が作成したプル リクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents page][agents-page]
- [Custom agent について][custom-agents]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
