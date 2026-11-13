---
title: "レッスン 4 - custom instruction で Copilot を案内する"
description: "リポジトリの instruction を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法で実行する必要があるタスクでは、そのガイダンスを Copilot が参照できるようにします。[Instruction file][instruction-files] は、求めるコードだけでなく、その構成方法も説明します。フィルタリング機能を作成したので、Copilot が使った instruction を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの instruction とパス単位の instruction file が、エージェントにどのように渡るかを確認します。
- コーディング標準に従うよう instruction file を更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

優れた開発チームと同じように、Tailspin Toys にも開発プラクティスに関するガイドラインと要件があります。例を挙げます。

- コメントにはコードを言い換えるだけでなく、意図と自明でない判断を説明します。
- `db/` と `src/lib/` の export 関数には、`db` 引数を注入できる場合はそれも含めて、目的、パラメーター、戻り値を TSDoc/JSDoc で記述します。
- 再利用可能な Astro component には `Props` の契約を記述し、関連コードが変わったらコメントも最新の状態にします。
- 既存のフォーマットと lint のガイダンスを維持します。

Instruction file を使うと、ここで示したプラクティスに沿ってタスクを実行するために必要な情報を Copilot に確実に渡せます。

## Instruction file

Custom instruction を使うと、コンテキストや好みを Copilot に伝え、コーディング スタイルや要件を理解させられます。これは、より適切な提案やコード スニペットを得るための強力な機能です。好みのコーディング規約、ライブラリ、コードに含めるコメントの種類などを指定できます。リポジトリ全体、または特定のファイル種別向けの instruction を作成できます。

Instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべて**のリクエストで Copilot に送られる単一の instruction file です。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを提供します。

> [!NOTE]
> ほかの形式の instruction とそのサポート状況は、環境によって異なります。特定の形式に依存する前に、[custom instruction のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトの custom instruction file を確認する

スターター プロジェクトには、すでにいくつかの instruction file が含まれています。変更を加える前に、何が用意されているか確認しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡潔な説明とコーディング ガイダンスを確認します。この instruction は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの instruction が含まれています。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、instruction の適用対象を決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file や既存のプロジェクト ファイルへのリンクを確認します。大きな instruction を小さく再利用しやすい単位に分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて instruction file を更新する

既存のファイルはよい出発点ですが、追加したい内容があります。中核となる `copilot-instructions.md` を変更し、新しく生成する TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** という見出しを探します。35 行目付近にあります。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの instruction を読み込みます。instruction を保存したら、機能のコンテキストを保ったまま instruction を読み直すため、フィルタリングの会話を終了して再開します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ root から、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択画面が表示されたらフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot が instruction file からコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの instruction とパス単位の instruction file がエージェントに届く仕組みを確認しました。
- コーディング標準に従うよう instruction file を更新しました。
- instruction file がコードに与える影響を確認しました。

次は、[再利用できる quality-checks skill をカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI に custom instruction を追加する][instruction-files]
- [custom instruction のサポート][custom-instructions-support]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
