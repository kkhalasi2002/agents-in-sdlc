---
title: "レッスン 7 - QA agent を作成して使う"
description: "issue の要件、quality-checks skill、Playwright MCP を組み合わせる QA custom agent を作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill で自動チェックを実行し、Playwright MCP でブラウザー上のフィルタリング操作を確認しました。今度は、明確な QA プロセスを持つ custom agent にこれらの機能をまとめます。

このレッスンでは、次のことを行います。

- custom agent が instruction、skill、MCP ツールを使う仕組みを確認します。
- 再利用可能な quality assurance profile を作成して確認します。
- QA agent を選択し、フィルタリング issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プル リクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザー上の動作を一貫した方法で確認したいと考えています。custom agent を使うと、この QA プロセスを調整し、再利用できるレポートを作成できます。

## Custom agent とは

Custom agent は、Markdown profile で定義する専門化された Copilot です。profile には、エージェントの目的、instruction、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの instruction はチームの標準を説明します。`quality-checks` skill は繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA profile は、それらを使って要件を評価し、結果を報告するよう Copilot に指示します。profile はほかのカスタマイズの代わりになるものではなく、別の会話も必要ありません。

## QA profile を作成する

機能 PR を作成する前に、Copilot に再利用可能な QA profile を作成してもらいます。profile には、QA が実施するチェックと守るべき境界の両方を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次の custom agent を作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Profile を確認する

新しいエージェントを使う前に、意図した QA ワークフローと境界が profile に反映されていることを確認します。これにより、検証だけを依頼したのに不完全または範囲の広すぎるエージェントが機能を変更する事態を防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須で、`name` は省略できますが、含めるとエージェントの表示名が明確になります。
3. profile の instruction を読み、QA が要件を確認し、リポジトリの instruction に従い、`quality-checks` skill を実行して、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更を commit したりプル リクエストを作成したりしないことを確認します。
5. 必要な責務や境界が profile に反映されていない場合は、続ける前に既定のエージェントに修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトの agent を読み込みます。profile を作成した後、フィルタリングの会話を再開して QA を選択すると、すでに会話内にある issue と計画の決定を利用できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されない場合があります。エージェントは利用できるため、上記のコマンドで有効にしてください。

2. 次のプロンプトで、QA agent に機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent が作業を開始します。
4. 作業が完了したら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに加え、その作業をレビューしました。このレッスンでは、次のことを行いました。

- custom agent が instruction、skill、MCP ツールを使う仕組みを確認しました。
- 再利用可能な quality assurance profile を作成して確認しました。
- QA agent を選択し、フィルタリング issue に照らして結果をレビューしました。

実装、skill の更新、QA profile、テスト、検証レポートが揃いました。次は、これらを [機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用 custom agent を作成する][create-agents]
- [Custom agent の構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
