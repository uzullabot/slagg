# PR Title

chore: 依存パッケージを最新安定版へアップグレード

# PR Body

## 概要

依存パッケージを見直し、可能な範囲で最新安定版へアップグレードしました。
主に Slack SDK / Biome / Vitest を更新し、メジャーアップデートに伴うテスト互換修正を加えています。

## 変更内容

- `@slack/socket-mode` を `2.0.6` へ更新
- `@slack/web-api` を `7.15.0` へ更新
- `@biomejs/biome` を `2.4.9` へ更新
- `vitest` を `4.1.2` へ更新
- Biome v2 向けに `biome.json` を更新
- Vitest v4 に合わせて `test/team/TeamManager.test.js` のクラスモック実装を修正
- 軽微な lint 対応（未使用変数名など）

## 背景・判断

- まず patch/minor 相当の安全な更新を優先しました
- `Biome` と `Vitest` はメジャー更新ですが、追随コストが限定的だったため今回まとめて更新しました
- `Vitest` 更新後に `TeamManager` テストが失敗したため、Vitest v4 のクラスモック仕様差分に合わせて修正しています

## 検証結果

実行コマンド:

```bash
npm install
npm test
```

結果:

- `npm audit`: 0 vulnerabilities
- `npm test`: 323 tests passed

## 影響範囲

- CLI の主要機能には変更なし
- 開発環境の依存関係とテスト基盤を最新化
- テストコードのモック実装は Vitest v4 前提になりました

## 補足

README / MANUAL_QA の内容変更は今回必須ではなかったため、依存更新と互換修正に絞っています。
