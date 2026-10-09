# Nginx Webサーバー構築記録

## 1. 目的

Ubuntu Server上にNginxを導入し、
HTTP通信によってWebページを配信する。

また、ポート80と8080で異なるWebページを公開する。

## 2. 環境

- OS：Ubuntu Server 26.04.1 LTS ARM64
- Webサーバー：Nginx
- 仮想化：UTM
- ネットワーク：Shared Network（NAT）

## 3. Nginxのインストール

```bash
sudo apt update
sudo apt install nginx -y
```

## 4. Nginxの状態確認

```bash
sudo systemctl status nginx
```

`active (running)` が表示されることを確認。

## 5. ポート8080の設定

Webページ用ディレクトリを作成する。

```bash
sudo mkdir -p /var/www/homelab8080
```

`/var/www/homelab8080/index.html` にHTMLを作成する。

Nginxの設定ファイルを作成する。

```bash
sudo nano /etc/nginx/sites-available/homelab8080
```

設定内容は `nginx/homelab8080.conf` を参照。

シンボリックリンクを作成して設定を有効化する。

```bash
sudo ln -s /etc/nginx/sites-available/homelab8080 /etc/nginx/sites-enabled/
```

## 6. 設定の確認と反映

```bash
sudo nginx -t
sudo systemctl reload nginx
```

## 7. 動作確認

ブラウザから以下にアクセスする。

- `http://<server-ip>/`
- `http://<server-ip>:8080/`

それぞれ異なるWebページが表示されることを確認。

## 8. 学んだこと

- Nginxの基本的な役割
- HTTP通信の仕組み
- ポート番号の役割
- Nginx設定ファイルの構造
- systemctlによるサービス管理
- シンボリックリンクによる設定の有効化