---
title: "演習 5 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom agent][previous-lesson] |
|:--|

GitHub Copilot を agent mode で使うと、コードベースの調査、変更の提案、ファイルの編集、コマンドの実行などを自律的に行います。Copilot は自分のマシンでリアルタイムに作業するため、VS Code の Copilot Chat ではツールの呼び出しやファイルの編集を逐一確認できます。差分をその場でレビューし、個々の変更を適用または取り消し、エディターを離れずに追加のプロンプトを送って作業を調整・拡張できます。

この演習では、次のことを行います。

- アクセシビリティ agent の作業をリアルタイムで監視します。
- 提案されたファイルの編集をレビューして適用します。
- セッションの途中で agent に指示し、ハイ コントラスト モードにライト モードの切り替えを追加します。

## シナリオ

[演習 4][previous-lesson] で使ったアクセシビリティ custom agent が、ハイ コントラスト モードを実装しています。作業中に、デザイン チームから追加の依頼がありました。既定のテーマが暗すぎると感じる利用者向けに、ライト モードの切り替えも追加してほしいとのことです。新しい会話を始める代わりに、既存のセッションに追加の指示を出して作業を拡張します。

## Copilot の進捗を監視する

Copilot Chat ビューでは、agent が調査するファイル、実行するコマンド、提案する編集など、各ステップを確認できます。ファイルの編集は差分と **Keep** / **Undo** の操作とともにチャット内に表示されます。

1. 演習 4 で使った Copilot Chat ビューに戻ります。
2. agent が作業している間、チャットをスクロールして各ツールの操作を確認します。ファイルの読み取りや編集、実行を求めるターミナル コマンドなどが表示されます。
3. agent がファイル編集を提案すると、差分がチャット内に表示されます。
   - **Keep** を選択して変更を適用します。
   - **Undo** を選択して変更を取り消し、agent にもう一度試してもらいます。
4. チャット上部の **Files changed** を開くと、このセッション中に agent が変更したファイルをまとめて確認できます。

> [!NOTE]
> ツールの操作に許可が必要な場合 (ターミナル コマンドの実行など)、agent は一時停止して **Allow** を表示します。実行してもよいコマンドだけを承認してください。

5. agent がハイ コントラスト モードの実装を終えるまで待ちます。最後に、変更内容をまとめたメッセージが表示されます。

## Agent にライト モードを追加するよう指示する

ハイ コントラスト モードができたので、同じ会話でライト モードの切り替えも追加します。同じチャット セッションを続けるため、agent は変更した component、採用したスタイル、設定した local storage のパターンなど、前の作業のコンテキストを保持しています。

1. Copilot Chat の入力欄に、次の追加プロンプトを送信します。

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. Agent は既存の変更を確認し、ライト モードを追加する作業を提案します。
3. ハイ コントラスト モードのときと同じように、新しい編集を確認します。差分ごとに **Keep** または **Undo** を選択します。
4. 望まない方向に進んだ場合、たとえばトグル UI を好ましくない方法でリファクタリングしようとした場合は、チャット入力欄の横にある **Stop** ボタンで停止して、意図を明確にする追加メッセージを送信します。

> [!TIP]
> 進行中のセッションに指示を追加するほうが、最初からやり直すより早く済みます。追加プロンプトを使って、調整を依頼します (「CSS を簡素化して、selector の重複ではなく CSS 変数を使ってください」)。方向を修正します (「トグルを 3 つにせず、ライト モードを既存の設定パネルに追加してください」)。作業範囲を広げます (「2 つのトグルにキーボード ショートカットも追加してください」)。

## 変更されたファイルをレビューする

作業を commit する前に、agent が変更したすべての内容を確認します。

1. VS Code で **Source Control** ビューを開きます。
2. 変更されたファイルをすべて確認します。Astro component、スタイル、関連テストなどが更新されているはずです。
3. 変更されたファイルをいくつか開き、差分を確認します。custom agent が指示したアクセシビリティ パターン、ARIA 属性、キーボード操作、semantic HTML、local storage による設定の保持などが反映されていることを確認します。
4. 問題がなければ、Source Control パネルで変更を stage して commit します。変更は[次のレッスン][next-lesson]で公開します。

## まとめと次のステップ

Copilot Chat の agent mode を使い、進行中のセッションを監視して調整しました。ファイルの編集を都度レビューして適用または却下し、コンテキストを保ったまま追加プロンプトで作業を拡張しました。

このレッスンでは、次のことを確認しました。

- agent の作業をリアルタイムで監視する。
- 個々のファイル編集を適用または却下する。
- 追加プロンプトで進行中のセッションを調整する。

[次のレッスン][next-lesson]では、アクセシビリティの作業をプル リクエストとして公開し、このラボを通じてローカルの custom agent が行った変更をレビューします。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [Agent mode の使用][agent-mode]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
