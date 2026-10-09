# Nginx リバースプロキシ構築記録

## 1. 目的

Nginxをリバースプロキシとして使用し、
Python（Flask）アプリケーションにHTTPリクエストを転送する。

## 2. システム構成

```text
MacBook（ブラウザ）
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
Flask（127.0.0.1:5000）
        |
        v
Hello from Python!
```

## 3. 環境

- OS：Ubuntu Server 26.04.1 LTS ARM64
- Webサーバー：Nginx
- アプリケーション：Python / Flask
- ネットワーク：UTM Shared Network（NAT）

## 4. Flaskアプリの構築

Pythonの実行環境をインストールする。

```bash
sudo apt update
sudo apt install python3 python3-venv -y
```

作業ディレクトリを作成する。

```bash
mkdir -p ~/homelab/flask-app
cd ~/homelab/flask-app
```

Pythonの仮想環境を作成する。

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Flaskをインストールする。

```bash
pip install flask
```

`app.py` を作成する。

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello from Python!"

if __name__ == "__main__":
    app.run(host="127.0.0.1", port=5000)
```

アプリケーションを起動する。

```bash
python app.py
```

別のSSHセッションから動作確認する。

```bash
curl http://127.0.0.1:5000
```

`Hello from Python!` が表示されれば成功。

## 5. Nginxリバースプロキシ設定

設定ファイルを作成する。

```bash
sudo nano /etc/nginx/sites-available/flask-proxy
```

以下の設定を記述する。

```nginx
server {
    listen 8081;
    server_name _;

    location / {
        proxy_pass http://127.0.0.1:5000;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## 6. 設定の有効化

```bash
sudo ln -s /etc/nginx/sites-available/flask-proxy /etc/nginx/sites-enabled/
```

設定ファイルを検証する。

```bash
sudo nginx -t
```

問題がなければ設定を反映する。

```bash
sudo systemctl reload nginx
```

## 7. 動作確認

MacBookのブラウザから以下にアクセスする。

```text
http://<server-ip>:8081/
```

`Hello from Python!` が表示されることを確認した。

## 8. 学んだこと

- FlaskによるWebアプリケーションの作成
- Python仮想環境の利用
- HTTPリクエストとレスポンス
- Nginxによるリバースプロキシ
- `proxy_pass` によるリクエスト転送
- localhost（127.0.0.1）の役割
- Webサーバーとアプリケーションサーバーの違い

## 9. 今後の課題

現在はFlaskの開発用サーバーを使用している。

SSH接続を終了するとアプリケーションが停止するため、
Gunicornとsystemdを導入してサービスとして管理する予定。