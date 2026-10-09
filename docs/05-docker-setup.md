# DockerによるFlaskアプリのコンテナ化

## 1. 目的

FlaskアプリケーションをDockerコンテナで実行し、
Nginxのリバースプロキシ経由でアクセスできる環境を構築する。

## 2. 環境

| 項目 | 内容 |
|---|---|
| OS | Ubuntu Server 26.04.1 LTS ARM64 |
| 仮想化 | UTM |
| コンテナ | Docker Engine |
| Webサーバー | Nginx |
| アプリケーション | Python / Flask |
| WSGIサーバー | Gunicorn |

## 3. システム構成

```text
MacBook Air
    |
    | HTTP :8081
    v
Ubuntu Server
    |
    v
Nginx（8081）
    |
    | proxy_pass
    v
127.0.0.1:5001
    |
    v
Dockerコンテナ
    |
    v
Gunicorn（5000）
    |
    v
Flask
```

## 4. Dockerのインストール

Docker公式のAPTリポジトリを登録し、
Docker EngineとDocker Composeプラグインをインストールする。

公式手順：
https://docs.docker.com/engine/install/ubuntu/

インストール後、以下を実行する。

```bash
sudo docker run hello-world
```

`Hello from Docker!` が表示されることを確認した。

## 5. Dockerイメージの作成

`docker/flask-app/` にDockerfileとアプリを配置する。

Dockerイメージをビルドする。

```bash
sudo docker build -t homelab-flask:1.0 .
```

イメージの一覧を確認する。

```bash
sudo docker images
```

## 6. Dockerコンテナの起動

```bash
sudo docker run -d \
  --name homelab-flask \
  -p 127.0.0.1:5001:5000 \
  homelab-flask:1.0
```

### オプション

- `-d`：バックグラウンドで起動
- `--name`：コンテナ名を指定
- `-p`：ホストとコンテナのポートを対応付ける
- `127.0.0.1:5001:5000`：ホスト側5001番からコンテナ側5000番へ転送

## 7. 動作確認

コンテナの状態を確認する。

```bash
sudo docker ps
```

HTTPリクエストを送信する。

```bash
curl http://127.0.0.1:5001
```

`Hello from Python!` が返ることを確認した。

## 8. Nginxとの連携

以下のNginx設定ファイルを編集する。

```text
/etc/nginx/sites-available/flask-proxy
```

`proxy_pass` を次のように変更する。

```nginx
proxy_pass http://127.0.0.1:5001;
```

設定を確認して反映する。

```bash
sudo nginx -t
sudo systemctl reload nginx
```

ブラウザで以下にアクセスする。

```text
http://<server-ip>:8081/
```

## 9. 学んだこと

- Dockerイメージとコンテナの違い
- Dockerfileの基本構文
- イメージのビルド
- コンテナの起動・停止
- Dockerのポートマッピング
- NginxとDockerの連携
- ARM64環境でのコンテナ実行

## 10. 今後の課題

- Docker Composeによる管理
- コンテナの自動再起動
- Dockerイメージの軽量化
- GitHub Actionsによる自動ビルド
- AWSへのデプロイ