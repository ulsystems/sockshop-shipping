---
source:
  repository: "https://github.com/ulsystems/sockshop-shipping"
  branch: "master"
  commit: "9c0fbfa"
  extracted_at: "2026-03-05T05:16:00Z"
  extractor: "devin"
  external_sources:
    - "https://github.com/ulsystems/sockshop-orders"
    - "https://github.com/ulsystems/sockshop-queue-master"
project:
  name: "sockshop-shipping"
  language: "java"
  framework: "spring-boot"
domains:
  - "shipping"
---

# 用語集

> 最終更新: 2026-03-05T05:16:00Z

## コアドメイン

### 配送（Shipping）

| 用語 | 英語名 | 説明 |
|------|--------|------|
| 配送 | Shipment | 配送タスクを表すエンティティ。UUIDで一意に識別され、注文に紐づく顧客IDを name フィールドとして保持する |
| 配送タスク | Shipping Task | RabbitMQ キューに投入される配送処理の単位。Shipment オブジェクトがJSON形式でシリアライズされてメッセージとして送信される |
| 配送タスクキュー | Shipping Task Queue | RabbitMQ 上のキュー名 "shipping-task"。配送サービスがプロデューサー、Queue Master がコンシューマーとなる |
| 配送タスクエクスチェンジ | Shipping Task Exchange | RabbitMQ 上のトピックエクスチェンジ名 "shipping-task-exchange"。配送タスクキューへのルーティングを担当する |

### ヘルスチェック（Health Check）

| 用語 | 英語名 | 説明 |
|------|--------|------|
| ヘルスチェック | HealthCheck | サービスおよび依存サービスの稼働状態を表す値オブジェクト。service（サービス名）、status（状態）、date（確認日時）で構成される |
| サービスステータス | Service Status | ヘルスチェックの結果を示す文字列値。正常時は "OK"、異常時は "err" |

### マイクロサービスアーキテクチャ

| 用語 | 英語名 | 説明 |
|------|--------|------|
| 注文サービス | Orders Service | Sock Shop の注文処理を担当するマイクロサービス（sockshop-orders）。配送サービスへ HTTP POST で配送リクエストを送信する呼び出し元 |
| キューマスター | Queue Master | RabbitMQ キューから配送タスクを消費し、実際の配送処理（Dockerコンテナのスポーン等）を実行するマイクロサービス（sockshop-queue-master） |
| フロントエンド | Front-end | Sock Shop のWebフロントエンド（sockshop-front-end）。ユーザーが商品を購入する際のUIを提供する |
| Sock Shop | Sock Shop | Weaveworks が開発したマイクロサービスデモアプリケーション。靴下のオンラインショップを模したアプリケーションで、マイクロサービスアーキテクチャのリファレンス実装として使用される |

### メッセージング

| 用語 | 英語名 | 説明 |
|------|--------|------|
| RabbitMQ | RabbitMQ | AMQP プロトコルに基づくオープンソースのメッセージブローカー。配送サービスと Queue Master の間の非同期通信を仲介する |
| AMQP | Advanced Message Queuing Protocol | メッセージ指向ミドルウェアのためのオープン標準プロトコル。RabbitMQ が実装するメッセージングプロトコル |
| トピックエクスチェンジ | Topic Exchange | RabbitMQ のエクスチェンジタイプの一つ。ルーティングキーに基づいてメッセージを適切なキューに振り分ける |
| メッセージコンバーター | Message Converter | RabbitMQ メッセージの送受信時にオブジェクトとJSON間の変換を行うコンポーネント。Jackson2JsonMessageConverter が使用されている |

### 監視・運用

| 用語 | 英語名 | 説明 |
|------|--------|------|
| 分散トレーシング | Distributed Tracing | マイクロサービス間のリクエストの流れを追跡する技術。Spring Cloud Sleuth + Zipkin で実現される |
| Zipkin | Zipkin | 分散トレーシングシステム。サービス間のリクエストのレイテンシや依存関係を可視化する |
| Prometheus | Prometheus | オープンソースの監視・アラートツールキット。HTTPリクエストのレイテンシ等のメトリクスを収集する |
| リクエストレイテンシ | Request Latency | HTTPリクエストの処理にかかった時間（秒単位）。Prometheus Histogram として記録され、service / method / route / status_code のラベルで分類される |
