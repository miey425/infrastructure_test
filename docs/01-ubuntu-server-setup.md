# Ubuntu Server構築記録

## 環境

- ホストPC：M5 MacBook Air
- 仮想化ソフト：UTM
- OS：Ubuntu Server 26.04.1 LTS ARM64
- CPU：2コア
- メモリ：4GB
- ストレージ：30GB
- ネットワーク：Shared Network（NAT）

## サーバー情報

- ホスト名：homelab-01
- ネットワーク設定：DHCP
- SSH：OpenSSH Server

## SSH接続

Macのターミナルから以下を実行する。

```bash
ssh miyabi@192.168.64.2