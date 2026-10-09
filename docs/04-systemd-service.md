# Gunicorn + systemd サービス構築記録

## 1. 目的

FlaskアプリケーションをGunicornで実行し、
systemdによってサービスとして管理する。

以下の機能を実現する。

- アプリケーションの自動起動
- サービスの起動・停止・再起動
- 異常終了時の自動再起動
- ログの確認
- SSH接続に依存しないアプリケーションの実行

## 2. システム構成

```text
MacBook Air
    |
    | HTTP :8081
    v
Ubuntu Server
    |
    v
Nginx
    |
    | リバースプロキシ
    v
Gunicorn（127.0.0.1:5000）
    |
    v
Flaskアプリ
    ^
    |
systemd（サービス管理）
```

## 3. 環境

| 項目 | 内容 |
|---|---|
| OS | Ubuntu Server 26.04.1 LTS ARM64 |
| 仮想化 | UTM |
| Webサーバー | Nginx |
| アプリケーション | Flask |
| WSGIサーバー | Gunicorn |
| サービス管理 | systemd |
| Python環境 | venv |

## 4. Gunicornのインストール

Flaskアプリケーションのディレクトリに移動する。

```bash
cd ~/homelab/flask-app
```

Python仮想環境を有効化する。

```bash
source .venv/bin/activate
```

Gunicornをインストールする。

```bash
pip install gunicorn
```

## 5. Gunicornの動作確認

以下のコマンドでアプリケーションを起動する。

```bash
gunicorn --bind 127.0.0.1:5000 app:app
```

別のSSHセッションからアクセスする。

```bash
curl http://127.0.0.1:5000
```

以下のレスポンスが返ることを確認。

```text
Hello from Python!
```

動作確認後、Ctrl+CでGunicornを停止する。

## 6. systemdサービスの作成

以下の場所にサービス設定ファイルを作成する。

```bash
sudo nano /etc/systemd/system/flask-app.service
```

設定内容は `systemd/flask-app.service` を参照。

## 7. サービスの登録と起動

systemdに設定ファイルを再読み込みさせる。

```bash
sudo systemctl daemon-reload
```

自動起動を有効化し、サービスを起動する。

```bash
sudo systemctl enable --now flask-app
```

サービスの状態を確認する。

```bash
sudo systemctl status flask-app
```

以下の表示を確認。

```text
Active: active (running)
```

## 8. サービス管理

### 起動

```bash
sudo systemctl start flask-app
```

### 停止

```bash
sudo systemctl stop flask-app
```

### 再起動

```bash
sudo systemctl restart flask-app
```

### 状態確認

```bash
sudo systemctl status flask-app
```

## 9. ログの確認

```bash
sudo journalctl -u flask-app -f
```

systemdのジャーナルからFlaskアプリのログをリアルタイムで確認できる。

終了する場合はCtrl+Cを押す。

## 10. 動作確認

MacBookのブラウザから以下にアクセスする。

```text
http://<server-ip>:8081/
```

Gunicornを手動で起動せずに、
`Hello from Python!` が表示されることを確認した。

### OS再起動後の確認

```bash
sudo reboot
```

Ubuntuの再起動後、再びブラウザからアクセスし、
アプリケーションが正常に動作することを確認する。

※OS再起動後の動作確認は別途実施する。

## 11. 学んだこと

- Flask開発用サーバーとGunicornの違い
- WSGIアプリケーションサーバーの役割
- systemdによるサービス管理
- Unitファイルの構造
- 自動起動の設定
- 異常終了時の再起動設定
- journalctlによるログ確認
- NginxとGunicornの連携

## 12. 今後の課題

- Dockerによるコンテナ化
- Docker Composeによるサービス管理
- GitHub Actionsによるデプロイ自動化
- AWSへの移行