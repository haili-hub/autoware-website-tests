# auto-website-tests

[English](README.md) | [日本語](README.ja.md)

![CI](https://github.com/haili-hub/autoware-website-tests/actions/workflows/ci-e2e-tests.yml/badge.svg)

Autoware ウェブサイトのナビゲーションワークフローを対象とした Playwright E2E テストです。ホームページから GitHub リポジトリの README までの一連の操作をカバーしています。

## テスト一覧

| ファイル | テスト | 対象 |
| -------- | ------ | ---- |
| `tests/website-github-navigation/TC001_initial-access.ts` | 初期アクセス | autoware.org |
| `tests/website-github-navigation/TC002_find-github-link.ts` | GitHub リンクの検索とアクセス | autoware.org → github.com |
| `tests/website-github-navigation/TC003_repository-access.ts` | GitHub リポジトリへのアクセス | github.com/autowarefoundation |
| `tests/website-github-navigation/TC004_readme-translation.ts` | README の確認 | github.com/autowarefoundation/autoware |

## 動作要件

- Node.js（LTS）
- Chromium（Playwright 経由でインストール）
- `autoware.org` および `github.com` へのインターネット接続

## セットアップ

```bash
npm install
npx playwright install chromium
```

## テストの実行

```bash
# 全テストを実行（HTML レポーター）
npm test

# UI モードで実行
npm run test:ui

# デバッグモードで実行
npm run test:debug

# 最新の HTML レポートを開く
npm run test:report
```

## Allure レポート

```bash
# Allure 結果を収集しながらテストを実行
npm run test:allure

# Allure レポートを生成して開く
npm run allure:report
```

結果は `allure-results/` に出力され、生成されたレポートは `allure-report/` に保存されます。

## 設定

`playwright.config.ts` — Chromium のみ使用。`autoware.org` の読み込みに対応するため、以下のタイムアウトを設定しています：

| 設定項目 | 値 |
| -------- | -- |
| テストタイムアウト | 60秒 |
| ナビゲーションタイムアウト | 60秒 |
| アクションタイムアウト | 30秒 |
| リトライ（CI） | 2回 |
| ワーカー数（CI） | 1 |

デフォルトでは HTML レポーターを使用します。`npm run test:allure`（`ALLURE=true`）で Allure レポーターに切り替わります。初回リトライ時にトレースを収集します。

## テスト計画

テスト計画の詳細は `specs/autoware-website-navigation.plan.md` を参照してください。

## 備考

- **ブラウザ翻訳**（右クリック → 日本語に翻訳）はブラウザのネイティブ機能であり、Playwright では自動化できません。翻訳品質の確認は手動で行う必要があります。
- `autoware.org` はバックグラウンドポーリングを使用しているため `networkidle` が解決されません。テストではデフォルトの `load` イベントを使用しています。
