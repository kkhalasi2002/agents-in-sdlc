---
title: "演習 1 - custom instruction (cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法で実行する必要があるタスクや、Copilot が知っておくべき背景情報がある場合は、必要なコンテキストにアクセスできるようにします。[Instruction file][instruction-files] は、Copilot にガイダンスを伝え、何をしてほしいかだけでなく、どのように実行してほしいかも理解させます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位の instruction file を通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に届く仕組みを確認します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue を非同期で処理するため、影響をリアルタイムで並べて示すのは難しいためです。このハーネスの後半で cloud agent が作成したプル リクエストをレビューするときに、instruction file の影響を確認します。

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

## 新しいリポジトリ標準を追加する

この演習で編集するのは、プロジェクト全体に適用する、コード内ドキュメントとファイル レベルのコメント ヘッダーのルールです。後の演習で issue を割り当てると、cloud agent がこのルールに従います。

編集する前に、作業ブランチを作成します。cloud agent は issue の対象ブランチから instruction を読み込みますが、ここでは変更をきちんと commit します。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

先ほど説明したように、`.github/copilot-instructions.md` はプロジェクト レベルの情報を Copilot に伝えるファイルです。コーディング標準を記述し、コード提案の質を高めましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールはありません。そのため、先ほど生成した helper に doc comment がありませんでした。
3. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc の doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、instruction file は全体的なガイダンス用のリポジトリ レベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。今回の doc comment ルールのようにプロジェクト全体に適用する標準は、リポジトリ レベルのファイルに記載します。

## instruction の更新を commit、push、マージする

cloud agent は issue の対象ブランチから instruction file を読み込みます。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が instruction を読み込めるよう、変更を `main` にマージする必要があります。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプル リクエストを開き、マージします。次の演習で issue を割り当てると、cloud agent はこの instruction を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチ作成手順を省略して `main` に直接 commit できます。ブランチを使う手順は、実際のプロジェクトでの進め方に近づけるために含まれています。

## まとめと次のステップ

このプロジェクトの instruction file から Copilot がコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。このハーネスの後半で、cloud agent が作成するプル リクエストに標準が反映されていることを確認します。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot の custom instruction をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
