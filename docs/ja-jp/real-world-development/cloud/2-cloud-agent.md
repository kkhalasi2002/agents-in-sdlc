---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom instructions][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ issue、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能の追加といった作業を自律的に実行できます。作業が完了すると、人間がレビューできるドラフト PR を作成します。こうした面倒な作業を任せることで、開発プロセスを加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent では、次のことを確認します。

- コード生成のための環境をカスタマイズする。
- 操作を安全に実行する。
- issue の範囲を明確にすることの重要性を理解する。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。サイトの最初のバージョンを開発した委託先は、ドキュメントを適切に整備していませんでした。実際、ドキュメントはまったくありません。最初の作業として、アプリケーションのすべての export 関数に TSDoc の doc comment を追加します。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する別のタイトルを「関連するゲーム」として表示し、利用者が引き続きゲームを探せるようにします。まだ完成度の高い実装は必要ありませんが、UX の受け入れテストに使えるものを求めています。この作業は現在ブロックされていますが、ほかに優先度の高い issue があります。

このような作業は優先度が下がりやすく、Copilot cloud agent に適しています。Copilot cloud agent に非同期で作業させれば、開発者はほかの作業に集中し、後から戻って成果をレビューし、期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。人間の開発者に仕事を依頼するときと同じく、[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法などがあります。割り当てると、Copilot は進捗を追跡するドラフト プル リクエストを作成し、環境をセットアップして、タスクに取りかかります。作業中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策をレビューできる状態になると、プル リクエストで通知されます。

## 範囲を明確にした instruction の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な作業にも、掘り下げると複雑な面があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして、段階を踏み、学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリの custom instruction

これまでの演習では、custom instruction と Copilot への指示方法を確認しました。custom instruction の演習をすでに行った場合は `.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、ここで簡単に見ておきましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる instruction file を読み取り専用で確認しておくと、後でその影響を見つけやすくなります。

GitHub Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc の doc comment に関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter を確認します。この instruction は `**/*.test.ts` に一致するファイルに適用されます。

次のセクションで **Code lacks documentation** issue を cloud agent に割り当てるとき、作成されたプル リクエストに TSDoc の doc comment、TypeScript の規約、コメント ヘッダーが反映されているか確認してください。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境をセットアップする

誰がコードを書く場合でも、通常は適切な状態で作業を始めるための環境とセットアップ スクリプトが必要です。ソフトウェア エンジニアのように作業する Copilot にタスクを割り当てる場合も同じです。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用の setup workflow][setup-workflow]を作成すると、作業開始前に実行する環境をカスタマイズできます。これにより、必要な開発ツールや依存関係を利用できるようになります。ワークショップをスムーズに進めるため、事前に設定済みです。この workflow は Node.js、プロジェクトの依存関係、end-to-end test 用のブラウザー バイナリ、単一の Astro アプリ用に migration と seed を済ませた SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要な手順に対し、可能な限り最小の権限を設定します。Copilot の操作には専用の token が与えられます。
    permissions:
      # セットアップ手順の一部としてリポジトリを clone する場合 (依存関係のインストールなど) は、`contents: read` 権限が必要です。セットアップ手順で clone しない場合は、完了後に Copilot が自動的に clone します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 単一の Astro アプリの build と test でデータを使えるよう、ローカル SQLite データベースを migration して seed します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub workflow と似ていますが、いくつか重要な点があります。

- `copilot-setup-steps` という 1 つの job があります。この job は、Copilot がプル リクエストで作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` trigger を使うと、リポジトリの **Actions** タブから workflow を手動で実行できます。Copilot が実行するのを待たずに、workflow が正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントが重要だと誰もが理解していても、多くのプロジェクトでは情報が古かったり、まったくなかったりします。この種の技術的負債は放置されやすく、生産性を低下させ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメント作成を得意とするため、この作業は Copilot cloud agent に割り当てるのに適しています。Copilot はバックグラウンドで必要なドキュメントを作成します。後の演習で作業をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい issue のダイアログを開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプル リクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような内容です。一覧に PR が表示されない場合は、しばらく待ってからブラウザーを更新してください。
12. 数分後に、Copilot が todo list を作成したことを確認します。

> [!NOTE]
> Copilot の todo list が PR に表示されるまで数分かかることがあります。Copilot が前述の workflow を使って環境を作成し、プロジェクトを分析して、課題に取り組む方法を決めています。

13. todo list と、実行予定のタスクを確認します。
14. プル リクエストのタイムラインを下にスクロールし、Copilot が issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ブラウザーを更新する必要がある場合があります。

16. ライブ セッションをスクロールして Copilot の作業を確認します。コードを調査して状態を把握し、計画を考えて判断した後にコードを作成する様子を確認できます。

作業には数分かかる場合があります。Copilot cloud agent の主な目的の 1 つは、非同期にタスクを実行し、その間にほかの作業を進められるようにすることです。2 つ目のタスクを cloud agent に割り当て、その間にアプリケーションのコードを書いて機能を追加しましょう。

## ゲームの詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分担できることです。デザイン チーム向けに関連ゲーム セクションを追加する作業はそれほど時間がかからないとしても、その間に別の作業を進められます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい issue のダイアログを開きます。
4. **Blank issue** を選択して空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことを示します。

![目の絵文字で issue の作業中であることを示す Copilot](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように作業するので、常に監視する必要はありません。完了後にレビューします。次は custom agent の作成と使用を確認しましょう。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に issue を割り当てると、非同期に作業を進められます。技術的負債の解消、新しい機能の作成、別のフレームワークからのコード移行などに活用できます。

次の概念を確認しました。

- コード生成のための環境をカスタマイズする。
- 操作を安全に実行する。
- issue の範囲を明確にすることの重要性を理解する。
- issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、custom agent の作成と使用に進みます。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認した custom instruction も使えます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent の setup workflow に関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: Custom agents →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
