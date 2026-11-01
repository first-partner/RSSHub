# 00. 既存スキル・プラグイン・コネクタ調査

調査日: 2026-11-01
調査範囲: claude.ai のスキル、プラグインカタログ（Anthropic Directory）、MCP コネクタレジストリ

## 1. 調査結果サマリ

| 区分                             | 結果                                                                       |
| -------------------------------- | -------------------------------------------------------------------------- |
| ユーザーの claude.ai スキル      | GA4 / Google Ads 関連は **該当なし**                                       |
| 接続済みコネクタ                 | Google Drive / Gmail / Google Calendar のみ。GA4・Google Ads は **未接続** |
| プラグイン（未導入・導入可能）   | GA4 + Google Ads を扱うものが複数あり（下表）                              |
| MCP コネクタ（未導入・導入可能） | Supermetrics、Windsor.ai、Adspirer、BigQuery、Polar Analytics など         |

## 2. 候補になる既存プラグイン / コネクタ

| 名称                              | 提供元                     | 対応データ                                     | 主な機能                                                                                                     | 書き込み（入札・予算変更） |
| --------------------------------- | -------------------------- | ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------ | -------------------------- |
| Trackian Marketing Analytics      | Trackian（community）      | GA4, Google Ads, Meta, TikTok, GSC, Shopify 等 | 週次レビュー、異常検知（毎晩）、無駄な検索語句、ファネル離脱、示唆出し（`/recommendations`, `/investigate`） | KPI 目標・アラートのみ     |
| AdCopilot                         | Atromx（community）        | Google Ads, GA4, GSC, GTM                      | 日次/週次レポート、監査、除外キーワード提案                                                                  | あり（承認制）             |
| Windsor.ai                        | Windsor.ai（partner）      | 320+ ソース（GA4, Google Ads 含む）            | キャンペーンレポート、無駄な費用検出                                                                         | あり                       |
| Supermetrics（コネクタ）          | Supermetrics               | 200+ ソース                                    | データ取得（クエリ）中心                                                                                     | なし                       |
| AdvisorPPC                        | Advisor Media（community） | Google Ads, GA4, GSC, GTM, YouTube             | 監査、PPC レポート                                                                                           | 有料プランであり           |
| Adspirer                          | Adspirer（community）      | Google Ads, Meta, 他多数 + GA4                 | パフォーマンスレビュー、最適化、運用                                                                         | あり（承認制）             |
| Marketing                         | Anthropic                  | Supermetrics / Amplitude 等経由                | `performance-report` スキル（レポート雛形）                                                                  | なし                       |
| Google Cloud BigQuery（コネクタ） | Google                     | BigQuery                                       | SQL 実行（GA4 / Ads の BigQuery エクスポートを分析）                                                         | なし                       |

## 3. 評価と方針

**既製品だけで足りるケース**: 「日次/週次で数値を見て、一般的な改善提案が欲しい」だけなら、Trackian や AdCopilot などを入れれば数日で動きます。

**自作が向くケース**（本設計の前提）:

1. 自社固有の KPI 定義（例: 「MQL = GA4 の `generate_lead` かつ特定 LP」）や目標値で評価したい
2. 示唆のロジック・優先度付けを自社の運用ルールに合わせたい
3. データを自社の GCP / BigQuery に残し、外部 SaaS に広告データを預けたくない
4. 出力先（Slack、Google ドキュメント、スプレッドシート）や頻度を自由に決めたい
5. SaaS 費用を抑えたい

**採用方針（推奨）: ハイブリッド**

- データ取得は **Google 公式 API（GA4 Data API / Google Ads API）を自前で叩く**。もしくは BigQuery エクスポートを使い、BigQuery コネクタで読む。
- 集計・分析は **決定的なコード（Python）** で実行し、Claude には数値を計算させない。
- 示唆出し・文章化だけを **Claude（Claude Agent SDK / Messages API）** に任せる。
- 既製プラグインは PoC 期間の比較対象・セカンドオピニオンとして試用する（任意）。

> 補足: 上記の既製プラグインを試す場合は、claude.ai のプラグイン / コネクタ設定から導入・認証が必要です（このセッションからは接続できません）。
