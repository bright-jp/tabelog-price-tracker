# Tabelog 価格トラッカー

[![Bright Data](https://img.shields.io/badge/Powered%20by-Bright%20Data-blue?style=flat-square)](https://brightdata.jp)
[![Tabelog Price Tracker](https://img.shields.io/badge/Tabelog%20Price%20Tracker-Managed%20Solution-orange?style=flat-square)](https://brightdata.jp/products/insights/price-tracker/tabelog)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow?style=flat-square)](https://python.org)

[![Bright Insights Price Tracker](https://raw.githubusercontent.com/danielshashko/bright-insights-assets/main/price-tracker-hero-v2.png)](https://brightdata.jp/products/insights/price-tracker/tabelog)

リアルタイムの Tabelog 価格トラッキング - eコマースプラットフォーム。開始方法は 2 通りあります: **フルマネージド**のインテリジェンスプラットフォーム、または Bright Data の AI Scraper Builder で構築する**カスタム scraper**です。

---

## オプション 1: Bright Insights - AI 搭載価格トラッキング（推奨）

**[Bright Insights](https://brightdata.jp/products/insights/price-tracker/tabelog)** は、Bright Data のフルマネージドなリテールインテリジェンスプラットフォームです。scraper を構築する必要も、インフラを維持する必要もありません。構造化され、分析にそのまま使える価格データを、ダッシュボード、data feed、または BI ツールに届けるだけです。

**チームが Bright Insights を選ぶ理由:**
- 🚀 **セットアップ不要** - すぐに使えるダッシュボードと data feed で数分で本番稼働
- 🤖 **AI による推奨** - 対話型 AI アシスタントが数百万件のデータポイントを即座に実用的なインサイトへ変換
- ⚡ **リアルタイム監視** - 1 時間ごとから日次までの更新頻度と即時アラート（email、Slack、webhook）
- 🌍 **無制限のスケール** - あらゆる website、あらゆる地域、あらゆる更新頻度に対応
- 🔗 **プラグアンドプレイ統合** - AWS、GCP、Databricks、Snowflake など
- 🛡️ **フルマネージド** - Bright Data が schema 変更、site 更新、データ品質を自動で処理

**主なユースケース:**
- ✅ 数百万の SKU にわたる **Tabelog の価格をリアルタイムで監視**
- ✅ **競合価格を追跡**し、値引きパターンを特定
- ✅ **価格改定を自動化**して Tabelog 上で競争力を維持
- ✅ MAP ポリシー準拠を監視し、価格違反を検出
- ✅ 競合のプロモーションと販促動向を追跡
- ✅ クリーンで正規化されたデータを dynamic pricing algorithm や AI model に直接投入

> **月額 $250 から - [最適なお見積もりはこちら →](https://brightdata.jp/products/insights/price-tracker/tabelog)**

---

## オプション 2: 独自の Tabelog scraper を構築する

事前構築済みの Tabelog scraper API がない？問題ありません。Bright Data の **AI Scraper Builder** なら、数クリックでカスタム Tabelog scraper を生成できます — コーディングは不要です。

### 数分で Tabelog scraper を構築

**[Tabelog AI Scraper Builder を開く →](https://brightdata.jp/products/web-scraper/tabelog)**

ドメインを選び、必要なデータ要件を記述するだけで、AI scraper builder が自動的に API を作成します。

1. **必要なデータを平易な英語で記述**
2. **AI が即座に scraper API を生成**
3. **API リクエストを実行してすぐに結果を取得**
4. 必要に応じて **組み込み IDE でコードを編集**

構築が完了すると、scraper に **Web Scraper ID** (`gd_xxxxxxxxxxxx`) が付与されます — 以下の Setup 手順で使うためにコピーしてください。

### 前提条件

- Python 3.9 以上
- [Bright Data account](https://brightdata.jp)（無料トライアルあり）
- Bright Data の **API token**（[取得方法](https://docs.brightdata.jp/general/account/account-settings#api-token)）
- Tabelog 用の **Web Scraper ID**（上記の構築手順で取得）

### Setup

1. **この repository を clone**

   ```bash
   git clone https://github.com/bright-jp/tabelog-price-tracker.git
   cd tabelog-price-tracker
   ```

2. **依存関係をインストール**

   ```bash
   pip install -r requirements.txt
   ```

3. **認証情報を設定**

   `.env.example` を `.env` にコピーし、値を入力します:

   ```bash
   cp .env.example .env
   ```

   ```env
   BRIGHTDATA_API_TOKEN=your_api_token_here
   BRIGHTDATA_DATASET_ID=your_dataset_id_here
   ```

   > **あなたの Web Scraper ID**
   > [AI Scraper Builder dashboard](https://brightdata.jp/products/web-scraper/tabelog) から Web Scraper ID をコピーし、
   > `BRIGHTDATA_DATASET_ID` に貼り付けてください（形式: `gd_xxxxxxxxxxxx`）。

---

## 使用方法

Tabelog scraper の構築が完了し、Web Scraper ID を `.env` に設定すると、Python interface は同じ方法で利用できます。

### 1. URL で特定の商品を追跡

Tabelog の商品 URL のリストを渡して、構造化された価格データを取得します:

```python
from price_tracker import track_prices

urls = [
    "https://www.tabelog.com/product/sample-item-123456",
    # Add more product URLs here
]

results = track_prices(urls)
for item in results:
    print(f"{item.get('title')} - {item.get('final_price', item.get('price'))} {item.get('currency', '')}")
```

または直接実行:

```bash
python price_tracker.py
```

### 2. キーワードで商品を検索

キーワード検索に一致する商品を見つけます:

```python
from price_tracker import discover_by_keyword

results = discover_by_keyword("laptop", limit=50)
```

### 3. カテゴリ URL で商品を閲覧

Tabelog のカテゴリページからすべての商品を収集します:

```python
from price_tracker import discover_by_category

results = discover_by_category(
    "https://tabelog.com/category/example",
    limit=100,
)
```

---

## 出力フィールド

各結果レコードには次のフィールドが含まれます:

| Field | Description |
|-------|-------------|
| `url` | 商品ページ URL |
| `title` | 商品名 / タイトル |
| `brand` | ブランドまたはメーカー |
| `initial_price` | 元の価格 / 定価 |
| `final_price` | 現在の販売価格 |
| `currency` | 通貨コード（例: USD、EUR） |
| `discount` | 値引き額または割引率 |
| `in_stock` | 商品が購入可能かどうか |
| `rating` | 平均星評価 |
| `reviews_count` | レビュー総数 |
| `seller_name` | 販売者名 |
| `images` | 商品画像 URL の配列 |
| `description` | 商品説明テキスト |
| `timestamp` | データ収集タイムスタンプ |

### 出力例

```json
[
  {
    "url": "https://www.tabelog.com/product/sample-item-123456",
    "title": "Example Product Name",
    "brand": "Example Brand",
    "initial_price": 59.99,
    "final_price": 44.99,
    "currency": "USD",
    "discount": "25%",
    "in_stock": true,
    "rating": 4.5,
    "reviews_count": 1234,
    "images": ["https://tabelog.com/images/product1.jpg"],
    "description": "Product description text...",
    "timestamp": "2025-01-15T10:30:00Z"
  }
]
```

---

## 高度なオプション

`trigger_collection()` 関数は、データ収集を制御するためのオプションパラメータを受け付けます:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | - | 返却するレコードの最大数 |
| `include_errors` | boolean | `true` | 結果に error report を含める |
| `notify` | string (URL) | - | snapshot の準備完了時に呼び出す webhook URL |
| `format` | string | `json` | 出力形式: `json`、`csv`、または `ndjson` |

オプション付きの例:

```python
from price_tracker import trigger_collection, get_results

inputs = [{"url": "https://www.tabelog.com/product/sample-item-123456"}]
snapshot_id = trigger_collection(inputs, limit=200, notify="https://your-webhook.com/hook")
results = get_results(snapshot_id)
```

---

## リソース

- 🌟 [Tabelog Price Tracker - Bright Insights (Managed)](https://brightdata.jp/products/insights/price-tracker/tabelog)
- 🏗️ [Tabelog scraper を構築](https://brightdata.jp/products/web-scraper/tabelog)
- 📖 [Bright Data Web Scraper API Documentation](https://docs.brightdata.jp/scraping-automation/web-scraper-api/overview)
- 🗄️ [Web Scrapers Control Panel](https://brightdata.jp/cp/scrapers)
- 🔑 [API token の取得方法](https://docs.brightdata.jp/general/account/account-settings#api-token)
- 🌐 [Bright Data ホームページ](https://brightdata.jp)

---

*[Bright Data](https://brightdata.jp) で構築 - 業界をリードする web data platform。*