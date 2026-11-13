---
title: "演習 1 - custom instruction (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法で実行する必要があるタスクや、Copilot が知っておくべき背景情報がある場合は、必要なコンテキストにアクセスできるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。中でも重要なのは、コード生成のガイダンスを伝える [instruction file][instruction-files] です。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位の instruction file を通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に届く仕組みを確認します。
- *現在の* instruction を使って、フィルタリングの最初のデータ部分となる publishers helper を生成します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する新しい標準を追加します。
- 同じプロンプトをもう一度実行し、生成コードに新しい標準が反映される様子を確認します。
- instruction の更新とフィルタリングの作業を `main` に commit し、次の演習で Copilot が更新済みのガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習の目的は、instruction を更新した後の振る舞いの*傾向*を確認することであり、出力を 1 文字単位で一致させることではありません。

## Instruction file

### シナリオ

優れた開発チームと同じように、Tailspin Toys にも開発プラクティスに関するガイドラインと要件があります。例を挙げます。

- データ レイヤーには、必ず unit test を用意します。
- UI はダーク モードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc の doc comment を使います。
- 各ファイルの冒頭に、ファイルの役割を説明するコメント ブロックを追加します。

Instruction file を使うと、ここで示したプラクティスに沿ってタスクを実行するために必要な情報を Copilot に確実に渡せます。

### Custom instruction

Custom instruction を使うと、コンテキストや好みを Copilot に伝え、コーディング スタイルや要件を理解させられます。より適切な提案やコード スニペットを得るために、好みのコーディング規約、ライブラリ、コードに含めるコメントの種類などを指定できます。リポジトリ全体、または特定のファイル種別向けの instruction を作成できます。

Instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべて**のリクエストで Copilot に送られる単一の instruction file です。使用する技術スタック、作成中のものの概要、ベスト プラクティス、その他の全体的なガイダンスなど、ほとんどの chat や CLI リクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成できます。TypeScript や Astro などの言語別ガイドラインや、UI component の作成、unit test の追加などのタスクに利用できます。

> [!NOTE]
> IDE では、instruction file は Copilot Chat のコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction file と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は [ほかの形式の instruction file][custom-instructions-support] もサポートしており、AGENTS.md や CLAUDE.md ファイルも利用できます。

### Instruction file を管理するためのベスト プラクティス

Instruction file の作成について詳しく説明することは、このワークショップの範囲外です。ただし、サンプル プロジェクトには代表的な方法を示す例が含まれています。大まかな指針は次のとおりです。

- `copilot-instructions.md` には、作成中のものの説明、プロジェクト構造、全体に適用するコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` は、ファイル種別 (unit test、Astro component、データ レイヤー) や特定のタスク向けの詳細な instruction に使います。
- 自然な言葉で、明確にガイダンスを記述します。望ましいコードと望ましくないコードの例も示します。

Instruction file の作成に唯一の正解がないのと同じように、AI の使い方にも唯一の正解はありません。試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した instruction file のセットを用意しましょう。このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に instruction file を生成してもらうこともできます。各 surface での操作は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。現在使っている surface のレッスンで、該当する場合に説明します。
>
> テンプレートや出発点を探していますか。[awesome-copilot][awesome-copilot] リポジトリには instruction file、custom agent、その他のリソースが集まっています。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

このリポジトリに含まれる instruction file を読みましょう。中核となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスにも注目してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの instruction があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、instruction の適用対象を決める glob が設定されています。この例では、`**/*.test.ts` に一致するものなど、任意の TypeScript test ファイルが対象です。
5. このプロジェクトで unit test を作成するための instruction を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開いて一番下までスクロールします。`unit-tests.instructions.md` などの別の instruction file や、プロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな instruction を小さく再利用しやすい単位に分け、コード生成時に Copilot が参考にできる例を示せます。(ここに記載されたパスは repo root ではなく、instruction file からの相対パスです。)

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc comment とファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## instruction を更新する*前に* Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

Custom instruction の影響を確認するため、まず現在の instruction file を使ってプロンプトを送信します。その後、instruction を更新して同じプロンプトをもう一度実行し、違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot に必要なコンテキストだけを渡します。
2. `src/lib/publishers.ts` を開き、helper の配置場所を Copilot に示します。
3. Chat ビューの agent dropdown で **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調査してコードを更新します。helper とそのテストなど、複数のファイルに変更が及ぶ場合もあります。
6. 提案された helper が型付き関数であり、最初の引数として `db` client を受け取り、型付きの publisher 配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約によるものです。
7. 提案されたコードには、TSDoc の doc comment とファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示しなくても doc comment を追加する場合があります。その場合も問題ありません。重要なのは、instruction 更新後に振る舞いの一貫性が向上することです。

## 新しいリポジトリ標準を追加する

先ほど説明したように、`.github/copilot-instructions.md` はプロジェクト レベルの情報を Copilot に伝えるファイルです。コーディング標準を記述して、コード提案の質を高めましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールはありません。そのため、先ほど生成された helper に doc comment がありませんでした。
3. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc の doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、instruction file は全体的なガイダンス用のリポジトリ レベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。今回の doc comment ルールのようにプロジェクト全体に適用する標準は、リポジトリ レベルのファイルに記載します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` を再度開き、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの冒頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

これで、Copilot が新しいプロジェクト標準に従い、次の演習で利用する実際のコードに適用するよう案内できました。

## instruction を commit してブランチを push する

Instruction file もリポジトリのほかのファイルと同じように、ソース管理で管理します。変更を commit して、リポジトリに push しましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次を実行し、instruction と helper の変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. ターミナルで instruction の更新と、helper の基盤として Copilot が変更したすべてのファイルを stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

ブランチを push したので、バックログにあるドキュメント標準の issue に関連付けてプル リクエストを作成します。手動で行う代わりに、すでにプロジェクトに接続されている GitHub ツールを使って Copilot に任せましょう。issue を探し、issue をクローズする PR を作成してから、マージを依頼します。

> [!NOTE]
> この演習では、Copilot が初めて GitHub 上の操作を実行します。プロジェクト テンプレートでは、GitHub ツールがすでに workspace に接続されています。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインが求められることがあります。案内に従って許可してください。この接続 (Model Context Protocol) の仕組みは、後の演習で説明します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. instruction file の更新と、今後のフィルタリング作業に使う publishers helper の基盤の両方が説明された PR を現在のブランチから作成するよう、Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub 操作を実行するための **Allow** を求めたら、コマンドを確認して、適切であれば **Allow** を選択します。
5. PR が作成されたら、次のプロンプトで PR のマージと、ブランチを main に戻すよう Copilot に依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Copilot が GitHub 操作と shell command を実行するための **Allow** を求めたら、コマンドを確認して、適切であれば **Allow** を選択します。

GitHub Copilot の助けを借りて、プル リクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトの instruction file から Copilot がコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- *既存の* instruction を使ってコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映される様子を確認しました。
- instruction の更新と helper の基盤を `main` に commit し、次の演習で使えるようにしました。

次は、Copilot がコードベース全体に新機能を追加するときに [agent mode で instruction を活用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot の custom instruction をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用 custom instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
