# web-service-gin

GinとMySQLを使ったREST APIアプリケーションです。Docker多段ビルドを使用して開発用と本番用を最適化しています。

## 🚀 クイックスタート

### 前提条件

- Docker & Docker Compose
- `.env` ファイル（環境変数設定用）

### 環境変数設定

`.env` ファイルを作成し、以下を設定：

```env
DB_HOST=db
DB_PORT=3306
DB_USER=
DB_PASSWORD=
DB_NAME=
```

## 🔧 開発環境

### ホットリロード開発（推奨）

```bash
# 開発環境起動（Air + MySQL）
docker-compose up -d

# ログ確認
docker-compose logs -f web

# 停止
docker-compose down
```

**特徴:**
- ✅ ファイル変更で自動再起動（ホットリロード）
- ✅ ボリュームマウントでコード同期
- ✅ Air設定でビルド最適化
- 📦 イメージサイズ: 約642MB

### APIテスト

```bash
# ホームページ
curl http://localhost:8080/

# アルバム一覧
curl http://localhost:8080/albums

# アルバム作成
curl -X POST http://localhost:8080/albums \
  -H "Content-Type: application/json" \
  -d '{"title":"My Album","artist":"Artist Name","price":29.99}'
```

## 🏭 本番環境

### 本番用イメージ作成

```bash
# 最適化された軽量イメージをビルド
docker build --target production -t myapp:prod .

# イメージサイズ確認
docker images myapp:prod
```

**本番用イメージの特徴:**
- 📦 イメージサイズ: **約21MB** (30倍軽量化！)
- ⚡ Alpine Linux + バイナリのみ
- 🔒 SSL/TLS証明書対応
- 🚀 高速起動

### 本番用起動例

```bash
# 本番用コンテナ起動
docker run -d \
  --name myapp-prod \
  -p 8080:8080 \
  -e DB_HOST=your-db-host \
  -e DB_PORT=3306 \
  -e DB_USER=your-user \
  -e DB_PASSWORD=your-password \
  -e DB_NAME=your-database \
  myapp:prod
```

## 📊 多段ビルド構成

| ステージ | 用途 | サイズ | 特徴 |
|---------|------|--------|------|
| **development** | 開発環境 | 642MB | Air + ホットリロード |
| **builder** | ビルド専用 | - | Go バイナリ作成 |
| **production** | 本番環境 | 21MB | 軽量 + セキュア |

## 🗄️ データベース

### 自動初期化

- `sql/init.sql`: テーブル作成とサンプルデータ
- 初回起動時に自動実行
- アルバムサンプルデータ（John Coltrane等）を含む

### MySQL接続

```bash
# MySQLコンテナに接続
docker-compose exec db mysql -u root -p

# データ確認
mysql> USE go_docker;
mysql> SELECT * FROM album;
```

## 🌐 API エンドポイント

| メソッド | パス | 説明 |
|---------|------|------|
| `GET` | `/` | ホーム |
| `GET` | `/albums` | アルバム一覧 |
| `POST` | `/albums` | アルバム作成 |
| `GET` | `/albums/:id` | アルバム詳細 |
| `PATCH` | `/albums/:id` | アルバム更新 |
| `DELETE` | `/albums/:id` | アルバム削除 |

## 🛠️ 開発ツール

- **Air**: Go ホットリロードツール
- **Gin**: 高速Webフレームワーク  
- **MySQL**: リレーショナルデータベース
- **Docker**: コンテナ化
- **多段ビルド**: 最適化されたイメージ作成
