# ☁️ KumoCMS
### エンタープライズグレードのマルチリージョンサーバーレスコンテンツ管理システム

KumoCMSは、グローバルスケールを目的として構築されたオープンソースの高性能ヘッドレスCMSです。AWSサーバーレスプリミティブを活用することで、マルチリージョンの耐障害性を組み込んだ「ゼロオペレーション」エクスペリエンスを提供し、高可用性環境でのドキュメント管理とコンテンツ配信に最適です。

---

## ✨ 主な機能

- 🌍 **マルチリージョンアクティブ-アクティブ**: DynamoDB Global TablesとS3クロスリージョンレプリケーションの組み込みサポートにより、サブ秒のデータ同期を実現。
- ⚡ **超低レイテンシー**: S3マルチリージョンアクセスポイント（MRAP）とRoute 53レイテンシーベースレコードによるグローバルトラフィックルーティング。
- 📦 **無制限のファイルサイズ**: S3署名付きURLを使用した直接的で安全なクライアント側アップロードにより、API Gatewayの10MB制限を回避。
- 🛡️ **セキュリティファースト**: AWS WAFによるレート制限とAmazon Cognitoによる堅牢なJWTベース認証とのネイティブ統合。
- 💰 **従量課金制**: 100%サーバーレス。パッチを適用するサーバーなし、アイドルコストなし。

---

## 🏗️ アーキテクチャ

KumoCMSは完全にAWSマネージドサービス上で動作します：

- **フロントエンド**: S3 + CloudFront上のReact（オプション）。
- **コンピューティング**: PythonベースのAWS Lambda + API Gateway（リージョナル/エッジ）。
- **データベース**: DynamoDB Global Tables（NoSQL）。
- **ストレージ**: Simple Storage Service（S3）。

---

## 📋 ユースケース

### 内部CMS

![KumoCMS Internal Use Case](diagrams/KumoCMS_Internal.png)

KumoCMSを組織の安全な内部ドキュメント管理システムとして展開します。企業文書、ポリシー、内部ナレッジベースを管理し、アクセスを制御するのに最適です。

### アクティブ-アクティブマルチリージョンコンテンツ管理

![KumoCMS Active-Active Multi-Region](diagrams/KumoCMS_Active-Active.png)

KumoCMSのマルチリージョンアーキテクチャを活用して、自動フェイルオーバー機能を備えたグローバルコンテンツ配信を実現します。地理的領域にわたる高可用性と災害復旧を必要とするミッションクリティカルなアプリケーションに最適です。

---

## 🧩 コンポーネント

KumoCMSは、モジュール化された再利用可能なコンポーネントから構築されています：

### kumocms-lambda-python

**リポジトリ**: [kumocms-lambda-python](https://github.com/kumocms/kumocms-lambda-python)

KumoCMS APIを支えるコアとなるPython Lambda関数コード。このリポジトリには、アップロード、取得、アーカイブ、復元、削除などのドキュメント管理操作のためのすべてのAPIハンドラーが含まれています。

**主な機能**:
- ドキュメントライフサイクル管理のためのRESTful APIハンドラー
- APIキー認証のためのAWS Secrets Manager統合
- クライアント直接アップロード用のS3署名付きURL生成
- イベント駆動型メタデータ抽出
- モックを使用した包括的なユニットテスト

**APIドキュメント**: [Swagger UI](./swagger.html) | [OpenAPI仕様](https://raw.githubusercontent.com/kumocms/kumocms-lambda-python/main/src/handlers/api/swagger.json)

### kumocms-vault-internal

**リポジトリ**: [kumocms-vault-internal](https://github.com/kumocms/kumocms-vault-internal) *（プライベート）*

KumoCMSを内部ドキュメント管理システムとして展開するための完全なTerraformインフラストラクチャコード。これは、本番環境でKumoCMSを展開する方法を示すリファレンス実装です。

**主な機能**:
- `kumocms-lambda-python` Lambda展開パッケージの利用
- リージョナルリソース用の`terraform-aws-kumocms-regional`モジュールの使用
- マルチリージョンデータレプリケーション用のDynamoDB Global Tablesの作成
- カスタムオーソライザーを使用したAPI Gatewayの構成
- 暗号化とライフサイクルポリシーを備えたS3バケットの設定
- APIキー用のAWS Secrets Managerの管理

### terraform-aws-kumocms-regional

**リポジトリ**: [terraform-aws-kumocms-regional](https://github.com/kumocms/terraform-aws-kumocms-regional)

リージョナルKumoCMSリソースをプロビジョニングするための再利用可能なTerraformモジュール。このモジュールは、単一リージョンで必要なすべてのAWSリソースをカプセル化します。

**主なリソース**:
- **API Gateway**: Lambda統合とカスタムオーソライザーを備えたRESTful API
- **Lambda関数**: ドキュメントハンドラーとイベントプロセッサー
- **S3バケット**: バージョニングとライフサイクルポリシーを備えたドキュメントストレージ
- **EventBridgeルール**: 自動化されたメタデータ抽出とDLQ再試行ロジック
- **SQSキュー**: 失敗した操作のためのDead Letter Queue
- **IAMロールとポリシー**: Lambda関数のための最小権限アクセス

---

## 🤝 コントリビューション

私たちはコントリビューションを歓迎します！バグレポート、新機能、より良いドキュメントなど、お気軽にissueを開くかプルリクエストを送信してください。

詳細については、[**コントリビューションガイドライン**](./CONTRIBUTING.md)をご覧ください。

1. プロジェクトをフォーク
2. フィーチャーブランチを作成（`git checkout -b feature/AmazingFeature`）
3. 変更をコミット（`git commit -m 'Add some AmazingFeature'`）
4. ブランチにプッシュ（`git push origin feature/AmazingFeature`）
5. プルリクエストを開く

---

## ⚖️ ライセンス

MITライセンスの下で配布されています。詳細は[**LICENSE**](./LICENSE)をご覧ください。

---

### 📺 リソース
- **[S3バケットのクロスリージョンレプリケーションの設定](https://www.youtube.com/watch?v=dQw4w9WgXcQ)**: KumoCMSアーキテクチャのコアコンポーネントであるS3レプリケーションルールを構成するための実践的なガイド。
