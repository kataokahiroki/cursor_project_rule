# AIエージェントのプロジェクトルール体験シナリオ【Cursor編】

# 目的

プロジェクトルールなし/ありで作ったアプリを並べて比較し、「ルールが効く」体験をする。
体験者はCursorを使ったことがあるプログラマを想定し、手順通りに進めれば同じ成果物になるようにする。

# 体験のゴール

簡単なアプリの作成を通じて、プロジェクトルール有/プロジェクトルール無の場合のそれぞれのUIとコードの差が確認できる。

# 前提

- CursorのSettingsでProject Rulesを作成できる
- プロジェクト直下に`.cursor/rules/`が生成される
- 作業時間は30分を想定
- 作業の大半は「このファイルの内容をコピペしてAIに指示するだけ」で進める

# ルール適用の種類

- 常時適用: すべてのチャットに自動で適用される
- 関連に応じて適用: ルールの説明文と作業内容の関連性で自動適用される
- 特定ファイルに適用: 指定したパターンに一致するファイルで適用される
- 手動適用: チャット内で明示的に指定した場合のみ適用される

本手順では「常時適用」の設定で体験する（`alwaysApply: true` を使用する）。

# 成果物

- `.cursor/rules/general.mdc`（または`.md`）
- `.cursor/rules/frontend.mdc`（または`.md`）
- `webapp-no-rules/`
  - `index.html`
  - `src/main.ts`
  - `src/style.css`
  - `tsconfig.json`
  - `package.json`
- `webapp-with-rules/`
  - `index.html`
  - `src/main.ts`
  - `src/style.css`
  - `src/ui/`
  - `src/logic/`
  - `tsconfig.json`
  - `package.json`
- `webapp-more-rules/`
  - `index.html`
  - `src/main.ts`
  - `src/style.css`
  - `src/ui/`
  - `src/logic/`
  - `tsconfig.json`
  - `package.json`

# 技術スタックとアーキテクチャ

- 技術スタック: TypeScript + HTML + CSS（Vanilla、フレームワーク不使用）
- 実行環境: Viteの開発サーバでローカル起動
- アーキテクチャ: Viteの標準構成（HTML=構造、CSS=見た目、TS=ロジック）
- 状態管理: `src/main.ts`内の単一オブジェクトで状態を保持

# 作成するサンプルアプリ

- 概要: 「簡単なTodo管理アプリ」
- 機能:
  - カレンダーから日付を選択し、テキスト欄にその日のTodoを入力しボタンで登録
  - 登録したTodoが日付の昇順かつ登録の早い順で一覧表示される
  - チェックボックスで未完／完了を管理できる
  - 完了したTodoはボタンで一括削除できる

# 手順

---

## ルールなし:事前準備

1. Cursorでこのプロジェクトを開く
2. `.cursor/rules/` フォルダを確認し、ルールが存在しないことを確認する
  - 無効の状態: `.cursor/rules/` 以下にルールファイルが存在しない
  - 有効になっている場合: `.cursor/rules/` 以下のルールファイルを削除する

## ルールなし:アプリ作成とテスト

1. 新しいチャットを開く
2. チャットに以下をそのまま貼り付けてアプリを作成する

```
以下の作業を実行してください。
詳細は `document/app_instructions.md` を参照してください。
対象アプリ名: webapp-no-rules
アプリのタイトルに「ルールなし」を入れてください。
```

## ルールなし:アプリの操作

ブラウザで動作確認する（Viteが出力するURLを使う）

## ルールあり:ルール作成（ファイル作成）

1. `.cursor/rules/` に `general.mdc` を作成する
2. `general.mdc` に以下を記入（常時適用のため `alwaysApply: true` を入れる）

```
---
description: 全体共通のルール。命名規則、禁止事項、出力形式が必要なときに使う。
alwaysApply: true
---
# Overview
- 変数名と関数名は camelCase
- CSSクラス名は kebab-case
- 1ファイル200行を超えない
- console.log は最小限にする
```

3. `.cursor/rules/` に `frontend.mdc` を作成する
4. `frontend.mdc` に以下を記入（常時適用のため `alwaysApply: true` を入れる）

```
---
description: UI設計、状態管理、アクセシビリティ、CSS方針が必要なときに使う。
alwaysApply: true
---
# Overview
- 主要ボタンに aria-label を付与する
- 色は #ffffff と #1f2937 のみ
- フォントは system-ui のみ
- レイアウトは最大幅640px、中央寄せ、余白は8px単位
- ボタンは角丸8px、枠線1px、背景は #1f2937、文字色は #ffffff
- ディレクトリ構造は `src/ui/` と `src/logic/` を必須
- `main.ts` は state / constants / dom / render / handlers / init の順に並べる
- 各セクションの先頭にコメントを入れる
- `type State` を必須、`any` は使わない
```

## ルールあり:新しいセッションに切り替える

1. 新しいチャットを開く
2. 以降は新しいセッションで進める（前の指示を持ち込まない）

## ルールあり:アプリ作成とテスト

チャットに以下をそのまま貼り付けてアプリを作成する

```
以下の作業を実行してください。
詳細は `document/app_instructions.md` を参照してください。
対象アプリ名: webapp-with-rules
アプリのタイトルに「ルールあり」を入れてください。
```

## ルールあり:アプリの操作

ブラウザで動作確認する（Viteが出力するURLを使う）

## 追加のルール:ルール作成（ファイル操作）

`frontend.mdc` に以下を追加する（貼り付けるだけ）

```
- ページ背景は #fff7ed にする
- 主要ボタンの背景色は #ea580c にする
```

## 追加のルール:新しいセッションに切り替える

1. 新しいチャットを開く
2. 以降は新しいセッションで進める（前の指示を持ち込まない）

## 追加のルール:アプリ作成とテスト

チャットに以下をそのまま貼り付けてアプリを作成する

```
以下の作業を実行してください。
詳細は `document/app_instructions.md` を参照してください。
対象アプリ名: webapp-more-rules
アプリのタイトルに「追加ルール」を入れてください。
```

## 追加のルール:アプリの操作

ブラウザで動作確認する（Viteが出力するURLを使う）

## ルールなし／あり／追加ルールの比較と効果確認

1. `webapp-no-rules/` と `webapp-with-rules/` を別タブで同時に表示
2. `webapp-with-rules/` と `webapp-more-rules/` を別タブで同時に表示
3. 確認ポイント（ここを確認してください）
  - ルール適用による差分: 配色、フォント、余白、ボタン形状
  - ルール適用による差分: `src/ui/` と `src/logic/` の有無
  - ルール適用による差分: `main.ts` の構造順とセクションコメント
  - ルール適用による差分: `type State` の有無と `any` 不使用

## アプリ停止と再起動

1. 開発サーバが起動しているターミナル（AIが `npm run dev` を実行したターミナル）で `Ctrl + C` を押して停止する
2. 再起動する場合は、それぞれのフォルダで `npm run dev` を実行する

