# Ubuntu Server構築記録

## 1. 構築環境

| 項目 | 設定 |
|---|---|
| ホストPC | M5 MacBook Air |
| 仮想化ソフト | UTM |
| OS | Ubuntu Server 26.04.1 LTS ARM64 |
| CPU | 2コア |
| メモリ | 4GB |
| ストレージ | 30GB |
| ネットワーク | Shared Network（NAT） |

## 2. サーバー情報

| 項目 | 設定 |
|---|---|
| ホスト名 | homelab-01 |
| ユーザー名 | miyabi |
| ネットワーク | DHCP |
| IPアドレス | 192.168.64.2 |
| SSHサーバー | OpenSSH Server |

※IPアドレスはDHCPで割り当てられるため、変更される可能性がある。

## 3. Ubuntu Serverの構築手順

1. UTMをインストールする。
2. Ubuntu Server ARM64のISOファイルをダウンロードする。
3. UTMでLinuxの仮想マシンを作成する。
4. CPU・メモリ・ストレージを設定する。
5. Ubuntu Serverをインストールする。
6. ホスト名とユーザーを設定する。
7. OpenSSH Serverを有効化する。
8. インストール完了後、Ubuntuを再起動する。

## 4. SSH接続

Macのターミナルから以下を実行する。

```bash
ssh miyabi@192.168.64.2
```

SSH接続に成功すると、次のように表示される。

```bash
miyabi@homelab-01:~$
```

### SSHサーバーの確認

Ubuntu側で以下のコマンドを実行する。

```bash
sudo systemctl status ssh
```

`active (running)` と表示されれば、SSHサーバーが正常に動作している。

## 5. 基本コマンド

| コマンド | 説明 |
|---|---|
| `whoami` | 現在のユーザー名を表示 |
| `hostname` | ホスト名を表示 |
| `pwd` | 現在のディレクトリを表示 |
| `ls -la` | ファイル一覧を詳細表示 |
| `ip a` | IPアドレスなどを確認 |
| `sudo apt update` | パッケージ情報を更新 |
| `sudo apt upgrade` | インストール済みパッケージを更新 |

## 6. 今後の予定

- [x] Ubuntu Serverのインストール
- [x] SSHサーバーの設定
- [x] MacからSSH接続
- [ ] Linuxの基本操作
- [ ] NginxによるWebサーバー構築
- [ ] Dockerの導入
- [ ] Webアプリケーションのデプロイ