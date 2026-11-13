---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で issue からフィルタリングを実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

まずは小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存のワークフローに沿って動作し、適切なものを適切な方法で構築できるように設計されています。このレッスンから、典型的なエージェント主導の開発プロセスをいくつかのレッスンにわたって進めます。issue をもとに新機能を作り、コードの妥当性と機能の動作を確認したうえで、プロジェクトにマージします。

> [!NOTE]
> 機能開発を続ける間は、同じ会話とブランチを使います。通常は、扱うファイルの種類ごとにブランチや PR を分けますが、ここでは主な概念に集中できるように手順を簡略化しています。

このレッスンでは、次のことを行います。

- GitHub issue から Copilot との新しい会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この後も、リポジトリ instruction の更新、既存の `quality-checks` skill のカスタマイズ、MCP による検証、QA agent の作成、機能 PR の作成を続けます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリや発行元でゲームを絞り込む必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意しておく詳細があります。Plan モードでそれらを決めてから、範囲を限定した実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発ワークフローに導入しても、基本は変わりません。むしろ、基本がさらに重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を説明する issue から始めます。
2. 構築する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で確認します。
6. プル リクエスト (PR) を作成します。
7. コードレビューが完了し、継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、一般的には上記の流れを基本にしています。

この標準的な進め方に沿うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じ確認プロセスを通るようにできます。

## 会話モード

**会話モード**は、エージェントにどこまで自律的に作業させるかを決めます。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive:** エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan:** エージェントが最初に計画を作成します。計画中はプロジェクト ファイルを編集できません。
- **Autopilot:** エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

最初に Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から始める

フィルタリングに取り組む前に、Codespace に戻り、リポジトリとターミナルが準備できていることを確認します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) から始まっていることを確認します。`#` を使うと、GitHub の issue やプル リクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てることで、機能の実装やタスクを Copilot に任せる前に、その進め方を決められます。複雑な作業では、少し時間をかけて計画を立てることをお勧めします。Plan モードに切り替え、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、最適な方法を検討して計画を作成します。

3. 作業中に、フィルタリングの動作について質問されることがあります。希望する動作に基づいて回答してください。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 上下にスクロールして計画を確認します。
6. 自分の判断と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたので、実装に進みましょう。Autopilot を使って Copilot に作業を任せます。

Autopilot では、Copilot が完了したと判断するまで、問題への対応を反復します。

1. **Accept plan and build on autopilot (recommended)**、またはインストールされているバージョンで同様の名前が付いた選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って作業を反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画を確認してください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同様にマージ前にレビューします。コードを確認し、サイトを起動して問題なく動作することを確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** も **Autopilot** も表示されていないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を issue と計画時の決定に照らし合わせます。
4. コードのレビューが終わったら <kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を試す

コードがよさそうでも、実際に動くでしょうか。前と同じようにアプリを起動し、Codespaces で転送されたポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. 計画で決めたとおりにカテゴリ、発行元、およびその組み合わせで絞り込みます。
4. リセットと結果が空の場合の動作が、issue と計画で決めた内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を作成し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から Copilot との新しい会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、コードが文書化されたプラクティスに従うよう、[custom instruction を使って][next-lesson]詳しく確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
