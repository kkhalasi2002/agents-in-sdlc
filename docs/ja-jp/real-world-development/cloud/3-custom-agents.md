---
title: "演習 3 - Custom agent"
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

Custom agent を使って、次のことを確認します。

- custom agent の定義方法。
- custom agent へのタスクの割り当て方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近の利用者のフィードバックによると、文字色と背景色のコントラストが十分でないため、現在のダーク テーマが読みにくいと感じる人がいます。このアクセシビリティ上の課題に対応するため、デザイン チームは利用者が切り替えられるハイ コントラスト モードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。機能を生成するために custom agent を使います。

## アクセシビリティ custom agent を確認する

アクセシビリティ向けの custom agent はすでに用意されています。Copilot をどのように案内するか確認しましょう。

Codespace に戻り、アクセシビリティ custom agent のファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は、custom agent に必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成を担当する内容。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と間違い。

## issue を作成して割り当てる

Mission control は、環境内のエージェントと作業するための中心的な場所です。Copilot cloud agent にタスクを割り当てたり、進捗を監視したり、進行中のセッションに追加のガイダンスを送ったりできます。まず、ハイ コントラスト モードを作成するタスクを Copilot に割り当てましょう。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい issue のダイアログを開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して割り当てダイアログを開きます。
9. custom agent の一覧から **Accessibility agent** を選択します。

   ![Custom agent と Accessibility が選択された cloud agent 割り当て画面のスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクに取り組み始めます。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた専門 AI アシスタントである、GitHub Copilot の [custom agent][custom-agents] を確認しました。Custom agent を使うと、チームの知識や標準を再利用可能なエージェントにまとめ、Copilot が特定の作業をより効果的に行えるように案内できます。

次の概念を確認しました。

- custom agent の定義方法。
- custom agent へのタスクの割り当て方法。

Copilot がハイ コントラスト モードを実装している間に、mission control から[エージェント セッションを監視して調整します][next-lesson]。

## リソース

- [Custom agent について][custom-agents]
- [組織で custom agent を使うための準備][org-custom-agents]
- [Enterprise で custom agent を使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントを監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
