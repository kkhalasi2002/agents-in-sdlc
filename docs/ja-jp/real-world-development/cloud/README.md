---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** は、GitHub Copilot がクラウドで非同期に作業できる機能です。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調査し、変更を加え、プル リクエストを作成します。その間、別の作業を進められます。

演習では、まず cloud agent が従う custom instruction を追加し、GitHub issue を割り当てて実装を任せます。custom agent を確認して使い、エージェント ダッシュボードからセッションの進捗を監視して作業を調整します。最後に、作成されたプル リクエストをレビューし、結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. custom instruction][ex1] | コンテキスト | cloud agent が従う custom instruction を追加します |
| [2. Cloud Agent][ex2] | 非同期エージェント | issue を GitHub Copilot cloud agent に割り当てます |
| [3. Custom Agents][ex3] | 専門エージェント | custom agent を確認して使います |
| [4. エージェントの管理][ex4] | 監視 | エージェント セッションを監視して調整します |
| [5. 反復して改善する][ex5] | レビュー | PR をレビューし、Copilot の作業を反復して改善して、次の手順を選びます |

## 前提条件

このワークショップを始める前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランがある GitHub アカウント
- [ ] アカウントまたは組織で GitHub Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランを利用していませんか。学生であることが確認された方は、[GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。このワークショップで使うエージェント、MCP、コード レビュー、Copilot CLI の機能は **Copilot Student** プランに含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free は対象外です。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス](../cli/) または [VS Code ハーネス](../vscode/) を参照してください。

## 始める

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
