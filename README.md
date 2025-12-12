# WordPress + MariaDB Docker Compose Project

このプロジェクトは、Docker Composeを使用して、WordPress（Webサーバ）とMariaDB（データベース）が連携した環境を構築するためのものです。

## 1. プロジェクトの概要
本プロジェクトでは、インフラ構成をコードで管理（IaC）し、誰でも同一の環境を再現できるように構築されています。
外部（Host）のポート **8888** を使用し、既存のWebサービスとの競合を避ける設定を行っています。

## 2. 前提条件（実行環境）
- **対象OS**: Windows 11 (WSL2環境)
- **CPU**: x86_64 (12th Gen Intel Core i5-1235U)
- **ツール**: Docker Desktop, Visual Studio Code

## 3. ファイル構成
```text
server-kadai/
├── .env                 # 環境変数（機密情報の分離管理）
├── docker-compose.yml   # サービス構成定義ファイル
├── README.md            # 本ドキュメント（構築手順書）
└── result.png           # 動作確認スクリーンショット
```

## 4. セットアップ手順

### ① 環境変数の設定
プロジェクトのルートディレクトリに `.env` ファイルを作成し、以下の内容を記述します。
```env
MYSQL_ROOT_PASSWORD=admin_pass
MYSQL_DATABASE=wp_db
MYSQL_USER=wp_user
MYSQL_PASSWORD=wp_pass
```

### ② コンテナの起動
VSCodeのターミナル（PowerShell）で以下のコマンドを実行します。
```powershell
docker compose up -d
```

### ③ 動作確認
ブラウザを起動し、以下のURLにアクセスします。
- **URL**: `http://localhost:8888`

---

## 5. 実行結果（スクリーンショット）
以下に、コンテナが正常に起動している状態および、WordPressのセットアップ画面が表示されている証拠写真を提示します。

![実行結果の確認](images/result.png)

---

## 6. トラブルシューティング
構築の過程で以下の対応を行いました。

- **ポート競合の回避**: 
  当初使用した `8080` ポートが既存のnginxと競合したため、`docker-compose.yml` を編集し、ホスト側ポートを `8888` へ変更。
- **Docker認証問題**: 
  イメージ取得時の認証エラーに対し、Docker Desktopからのサインアウト、および環境の再起動を行うことで対応。
- **CPUアーキテクチャの選択**: 
  Intel Core i5搭載機であるため、DockerインストーラーはAMD64用を選択。

## 7. セキュリティへの配慮
- **.envファイルの活用**: データベースのパスワード情報を設計図（YAML）から切り離し、安全性を向上。
- **内部ネットワーク化**: データベースコンテナ（3306番ポート）は外部公開せず、WordPressコンテナとの通信のみを許可。

## 8. 参考資料
- [WordPress Official Image - Docker Hub](https://hub.docker.com/_/wordpress)
- [MariaDB Official Image - Docker Hub](https://hub.docker.com/_/mariadb)