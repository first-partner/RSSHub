# Ads Analytics Agent（設計段階）

GA4 と Google 広告のデータを自動で集計・分析し、分析結果を根拠に改善の示唆を出す AI エージェント。

> 現在は **設計・要件定義フェーズ** です。実装は `docs/01_requirements.md` の「未決事項」が確定してから着手します。

## ドキュメント

| ファイル                                                             | 内容                                                                             |
| -------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| [docs/00_existing-tools-survey.md](docs/00_existing-tools-survey.md) | 既存スキル・プラグイン・コネクタの調査結果と、自作するかの判断                   |
| [docs/01_requirements.md](docs/01_requirements.md)                   | 要件定義（目的、スコープ、機能要件、非機能要件、KPI、ロードマップ、未決事項）    |
| [docs/02_architecture.md](docs/02_architecture.md)                   | システム設計（構成図、処理フロー、技術スタック、データモデル、インターフェース） |
| [docs/03_analysis-and-insights.md](docs/03_analysis-and-insights.md) | 分析ロジックと示唆出しの詳細設計（異常検知、寄与度分解、プロンプト、検証）       |

## 概要

```text
GA4 / Google 広告 ──► 取得 ──► 保存 ──► 集計 ──► 分析（コード） ──► 示唆（Claude） ──► 検証 ──► Slack / Google ドキュメント
```

- 数値の計算はすべてコードで行い、Claude は解釈と示唆の文章化だけを担当する
- Phase 1 は読み取り専用（広告の変更はしない）
