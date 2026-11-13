---
title: "演習 4 - Custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept] を使うと、開発ワークフローにおける特定のタスクや分野に合わせて、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置いてエージェントを定義すると、特定の作業を効果的に実行するための詳細な instruction、ベスト プラクティス、コーディング パターン、専門知識を Copilot に伝えられます。チームの知見を再利用可能なエージェントにできます。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを適用するテスト エージェントなどです。

Custom agent は、プロジェクト内の `.github/agents` フォルダー、または `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter を記述し、その後にエージェントの振る舞い、専門知識、instruction を定義する Markdown prompt を続けます。

### Custom agent と agent skill の違い

Custom agent と [agent skill][agent-skills-concept] には、機能上の重なりがあります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**custom agent は作業者、skill はツール**です。

Custom agent は独自のコンテキスト ウィンドウを持ち、作業の一部として skill や別のエージェントを調整するように設計されています。このワークショップでは、アクセシビリティ custom agent がアクセシビリティ ガイドラインに照らしてサイトをレビューし、更新します。その作業の一環として、プル リクエストのワークフローを扱う skill や、テストを実行・管理する skill などを呼び出すこともできます。

> [!NOTE]
> Custom agent の作成に唯一の正解はありません。AI のほかの活用方法と同じように、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近の利用者のフィードバックによると、文字色と背景色のコントラストが十分でないため、現在のダーク テーマが読みにくいと感じる人がいます。このアクセシビリティ上の課題に対応するため、デザイン チームは利用者が切り替えられるハイ コントラスト モードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。機能を生成するために custom agent を使います。

この演習では、次のことを行います。

- 既存のアクセシビリティ custom agent を確認します。
- Copilot Chat でアクセシビリティ agent を使い、ハイ コントラスト モードを実装します。

## アクセシビリティ custom agent を確認する

アクセシビリティ向けの custom agent はすでに用意されています。Copilot をどのように案内するか確認しましょう。

Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用の新しいブランチを作成します (演習 3 のフィルタリング PR とは分けて作業します)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は、custom agent に必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成を担当する内容。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と間違い。

## Copilot Chat で custom agent を使う

VS Code の Copilot Chat ビュー下部にある agent dropdown には、`.github/agents` で定義したすべての custom agent が表示されます。Custom agent を選択すると、その agent の instruction とツールを使う会話を始められます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

1. まだ選択されていない場合は、Chat ビューの agent dropdown で **Agent** を選択します。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部にある agent dropdown を選択します。既定では、現在の agent として **default** が表示されています。
3. 利用可能な agent の一覧から **Accessibility agent** を選択します。
4. 次のプロンプトを accessibility agent に送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat が作業を開始します。コードベースを調査して変更を提案し、プロジェクト ファイルを編集します。編集はファイル パスと差分とともにチャット内に表示されます。
6. 編集が適用されると、**Files changed** の表示が更新され、agent が変更したファイルを確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリに実際の変更を加えます。作業中に Astro component、CSS、関連テストなどの既存ファイルを編集します。

進行中の作業のレビューと調整は、次の演習で行います。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた専門 AI アシスタントである、GitHub Copilot の [custom agent][custom-agents] を確認しました。Custom agent を使うと、チームの知識や標準を再利用可能なエージェントにまとめ、Copilot が特定の作業をより効果的に行えるように案内できます。

次の概念を確認しました。

- custom agent の定義方法。
- Copilot Chat の agent mode で custom agent を使う方法。

次は[エージェントの作業を監視して調整し][next-lesson]、変更を確認しながら同じセッションにライト モードの切り替えも追加します。

## リソース

- [Custom agent について][custom-agents]
- [IDE で custom agent を作成する][creating-custom-agents-ide]
- [VS Code の custom agent][custom-agents-vscode]
- [Custom agent の構成][custom-agents-config]
- [awesome-copilot の custom agent][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントを監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
