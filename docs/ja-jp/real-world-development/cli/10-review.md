---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用できるカスタマイズ、参考リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys で一連の作業を進めながら、GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されているフィルタリング issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージしました。
- フィルタリング issue を起点に、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- custom instruction でエージェントを案内し、既存の `quality-checks` skill をカスタマイズして、unit test、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- quality assurance (QA) custom agent を作成して選択し、要件、テスト範囲、skill の結果、ブラウザーでの証拠を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリング PR の Agent Merge を承認しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使う slash command を確認しました。

## 提出した成果

このワークショップには 2 つの PR の節目があります。それぞれ、更新済みの `main` から作成した別のブランチで作業します。

1. **星評価:** ゲーム カードに既存の `starRating` と、評価がないことを明示する表示を追加します。
2. **フィルタリングと品質確認のワークフロー:** フィルタリングを実装し、instruction を更新して機能に適用し、`quality-checks` のレポートをカスタマイズして QA profile を作成し、関連するテストを追加します。

フィルタリングの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔に進めるため、これらの作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザーでの確認、MCP を通じた Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` skill は unit test、lint、型チェックを実行し、設定した新しい形式で結果を報告しました。QA agent は PR 作成前にそれらの結果と要件、テスト範囲のレビューをまとめました。

追加するテストは実際の不足を補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを承認する前にコードと証拠を確認し、変更後は影響を受ける証拠を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、instruction を更新し、skill をカスタマイズし、QA profile を作成して、MCP サーバーを設定しました。こうしたカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。instruction は標準を設定し、skill は繰り返し実行する作業を記述し、custom agent は専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて **モードとモデル** を選びます。作成前に方針を検討するときは **Plan**、範囲を絞った変更を一緒に進めるときは **Interactive**、範囲が明確なタスクを任せるときは **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

環境と同じくらい、コンテキストも重要です。何を、なぜ、どのように作りたいかを明確に伝えると、出力は大きく変わります。

## さらに試す

基本的なワークフローを学びました。Copilot CLI には、ほかにも次のような機能があります。

- `/review` でコード レビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを委任します。

## 次のステップ

どのツールも、使い続けることで上達します。本番コード、趣味のコード、何年も作りたいと思いながら手をつけていなかった小さなアプリなどで活用してください。学んだことをチームで共有し、チームの知見も取り入れましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境での進め方を確認するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
