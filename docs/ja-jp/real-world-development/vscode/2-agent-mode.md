---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。開発者は通常、必要なファイルを探し、変更を加え、テストを実行し、問題を修正し、見落としたファイルに気付いてもう一度変更する、といった作業を繰り返します。

ここで役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、IDE 内でより自律的に作業するよう設計されています。開発者のように既存のプロジェクト構造を調査し、必要な変更を加え、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに作られています。Agent mode は IDE 内で作業し、すばやいフィードバックと対話を可能にします。Cloud agent はチームの一員のように非同期で作業し、issue とプル リクエストを通じてやり取りするペア プログラマーとして設計されています。

この演習では、次の方法を学びます。

- Copilot Agent Mode でプロジェクトを調査し、関連ファイルを特定して、複数のファイルにまたがる変更を行う。
- GitHub Copilot Agent Mode で UI とデータ レイヤーに新機能を実装する。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする。

## シナリオ

ゲームの一覧が増えてきたので、利用者がカテゴリや発行元で絞り込めるようにします。前の演習で publishers helper を追加しました。次は、Copilot Agent Mode を使って残りのデータ レイヤー、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の機能を理解するため Tailspin Toys の Web サイトを確認しましょう。

この Web サイトは、開発者向けテーマのボード ゲームを扱うクラウドファンディング プラットフォームです。ゲームを一覧表示し、それぞれの詳細を確認できます。単一の Astro アプリであり、build 時にページを静的 HTML として生成します。ページは frontmatter 内で Drizzle ORM を通じてローカル SQLite データベースを直接参照します。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス helper は `src/lib/` に置き、操作性が必要な場合は標準 DOM API を使う小規模な Astro `<script>` を追加します。

### Web サイトを起動する

Web サイトを簡単に起動できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習を終えていれば、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くかどうかを尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは、Web サーバーを実行している Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **Home Page:** タイトル、画像、説明を含むボード ゲームの一覧を表示します。
- **Game Details Page:** ゲームを選択すると、タイトル、説明、発行元、カテゴリなど、詳しい情報が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

Web サイトの初期実装は動作しますが、さらに機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを作成すると、issue のバックログも自動的に作成されます。GitHub Copilot に内容を表示してもらいましょう。

1. Chat ビューの agent dropdown で **Agent** を選択します。**Agent** はファイルにまたがる変更を自律的に計画・実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨しているわけではありません。ワークショップを作成した際に Claude Sonnet 4.5 を使用したため、手順にも記載しています。安定した学習体験のため、受け取るコード提案が比較的一貫することを期待しています。ただし、大規模言語モデル (LLM) は確率的に動作するため、ワークショップに示された内容と異なる提案が出る場合があります。これは通常の想定された動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP command を使う場合がありますが、タスクは同様に完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が、利用者にとって役立つと考えた順に項目を優先付けしたことを確認してください。

## Instruction file を確認する

Copilot にコードを生成してもらう前に、作業のコンテキストを伝える instruction file を確認しましょう。Web サイトへの機能追加方法を説明する [user interface (UI) instruction file](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - component 設計、テスト可能性、アクセシビリティに関する原則。
   - ファイル種別に応じた instruction へのリンク:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Instruction file から、ほかの instruction file やプロジェクト内のファイルを参照できます。パスは instruction file の場所からの相対パスです。これにより、再利用や複雑な内容の分割が容易になり、生成時に Copilot が参考にできる例やテンプレートも示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) に残りのフィルタリング ロジックを追加または調整します。
- フィルタリングの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

さらに、コードベースにマージする前にテストを実行し、すべて成功することを確認します。Copilot Agent Mode にこれらの作業を任せましょう。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、instruction file を追加します。

   ![UI instruction file を選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューの agent dropdown で **Agent** が選択されていることを確認します。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. 次のプロンプトで、バックログの関連 issue に基づいて機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、目的の機能に関連するファイルを探す様子を確認します。データ レイヤー helper、UI、テストを見つけてファイルの変更を始め、テストを実行します。

   ![Copilot がプロジェクトのファイルを調査する様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調査し、ファイルを変更し、テストを実行します。タスクやコードベースの複雑さによっては、数分かかる場合があります。作業中、コード エディターに **Keep** と **Undo** のボタンが表示されることがあります。Copilot が完了すると、変更ごとに **Keep** または **Undo** を選択できるようになるため、作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![テストの実行を確認するダイアログを Copilot Chat ペインに表示したスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はタスクを完了し、エラーがないと判断するまでコード生成とテストを繰り返します。

   ![Copilot Agent Mode による Chat セッションの完了を示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いたブラウザーに戻り、新しい機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。具体的には、次のことを学びました。

- GitHub Copilot Agent Mode で UI とデータ レイヤーに新機能を実装する。
- Copilot Agent Mode でプロジェクトを調査し、関連ファイルを特定して、複数のファイルにまたがる変更を行う。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プル リクエストを作成します。

### ボーナスの探索演習 — ページ分割を実装する

ゲームの一覧が増えるとページ分割が必要になります。この演習で学んだことを使い、Copilot にサイトのページ分割機能を実装するよう依頼します。次の点を検討してください。

- 既存の instruction file を使うなど、現在のベスト プラクティスに従います。
- ページ分割の方法を検討します。利用者が 1 ページあたりの表示件数を選べるようにするか、固定値にするかを決めます。
- 希望する方法で実装できるよう、プロンプトで Copilot に必要なガイダンスを伝えます。
- GitHub Copilot に変更を依頼し、コンテキストを追加しながら反復する必要がある場合があります。Copilot を使う際の通常の流れです。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
