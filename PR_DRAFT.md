# PR タイトル

chore: 依存パッケージを最新安定版へアップグレード (2026-03)

---

# PR 本文

## 概要

`chore/dependency-upgrade-2026-03` ブランチで、全依存パッケージを最新安定版へアップグレードしました。
すべての既存テスト (323件) が通過しています。

## 変更内容

### patch / minor アップグレード（後方互換）

| パッケージ | 旧バージョン | 新バージョン | 種別 |
|---|---|---|---|
| `@slack/socket-mode` | 2.0.5 | 2.0.6 | patch |
| `@slack/web-api` | 7.10.0 | 7.15.0 | minor |

### major アップグレード

#### `@biomejs/biome` 1.9.4 → 2.4.9

- `biome migrate --write` により `biome.json` を v2 形式へ自動移行
  - `organizeImports` セクション → `assist.actions.source.organizeImports`
  - `files.include` / `files.ignore` → `files.includes`（否定プレフィックス形式）
- v2 で新たに有効になった lint ルールへ対応（ソースコード修正）:
  - `noUselessEscapeInRegex`: 正規表現内の不要なエスケープを除去
  - `noUnusedFunctionParameters`: 未使用引数に `_` プレフィックスを付与
  - `noUnusedVariables`: 未使用変数（catch 節の `error` 等）に `_` プレフィックスを付与
  - インポート順序 (`organizeImports`) の自動整列

#### `vitest` 1.6.1 → 4.1.2

- **セキュリティ脆弱性を解消**: vitest 1.x が依存していた `esbuild ≤0.24.2`（moderate）、`rollup 4.0.0-4.58.0`（high）の脆弱性が 3件 → 0件に
- **破壊的変更への対応**: vitest v4 では `vi.fn().mockImplementation()` でクラス（コンストラクタ）をモックする場合、アロー関数が `new` できないため `function` キーワードが必要
  - `test/team/SlackClient.test.js`: `SocketModeClient` / `WebClient` のモック実装を修正
  - `test/team/TeamManager.test.js`: `SlackClient` のモック実装を修正

### 見送り項目

| パッケージ | 現在 | 最新 | 理由 |
|---|---|---|---|
| `chalk` | 5.6.2 | 5.6.2 | すでに最新。`^5.3.0` の範囲内で最新版が使用済み |

## 検証結果

```
> npm test

> slagg@1.0.0 check
> biome check src/

Checked 10 files in 17ms. No fixes applied.

 RUN  v4.1.2 /home/zishida/dev/slagg

 ✓ test/team/TeamManager.test.js        (35 tests)
 ✓ test/team/SlackClient.test.js        (69 tests)
 ✓ test/integration/error-scenarios.test.js (11 tests)
 ✓ test/config/ConfigurationManager.test.js (59 tests)
 ✓ test/message/MessageProcessor.test.js (23 tests)
 ✓ test/main.test.js                    (18 tests)
 ✓ test/message/handlers/ConsoleOutputHandler.test.js (20 tests)
 ✓ test/integration/basic-flow.test.js  (11 tests)
 ✓ test/message/handlers/SpeechHandler.test.js (24 tests)
 ✓ test/utils/Logger.test.js            (12 tests)
 ✓ test/message/handlers/NotificationHandler.test.js (15 tests)
 ✓ test/shutdown.test.js                (19 tests)
 ✓ test/config/example-validation.test.js (3 tests)
 ✓ test/message/MessageHandler.test.js  (4 tests)

 Test Files  14 passed (14)
      Tests  323 passed (323)
   Duration  996ms
```

`npm audit` 結果: **脆弱性 0件**（アップグレード前は 3件：moderate×2、high×1）

## 影響範囲

- 本番コードの動作ロジックに変更なし（lint/format 修正のみ）
- テストの mock 実装のみ変更（テスト対象コードに変更なし）
