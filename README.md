# Homelab Infrastructure

Linux・ネットワーク・クラウド技術の学習と、インフラ構築・運用の実践を記録するリポジトリです。

## 📖 概要

クラウド・インフラエンジニアを目指し、サーバー構築から運用・自動化までの技術を段階的に学習しています。

単にサービスを動かすだけでなく、仕組みを理解し、構築手順を再現できる状態にすることを目標としています。

### 学習目標

- Linuxサーバーの構築・管理
- TCP/IP・DNS・HTTPなどのネットワーク技術の理解
- Dockerを利用したコンテナ環境の構築
- AWSを利用したクラウドインフラの構築
- GitHub ActionsによるCI/CDの実装
- TerraformによるInfrastructure as Code（IaC）の実践
- サーバーの監視・セキュリティ対策

## 🛠 技術スタック

| カテゴリ | 技術 |
|---|---|
| OS | Ubuntu Server |
| 仮想化 | UTM |
| ネットワーク | TCP/IP, DNS, SSH, HTTP/HTTPS |
| Webサーバー | Nginx |
| コンテナ | Docker, Docker Compose |
| クラウド | AWS |
| IaC | Terraform |
| CI/CD | GitHub Actions |
| バージョン管理 | Git, GitHub |

※今後学習・導入する予定の技術を含みます。

## 🗺 学習ロードマップ

### Phase 1：Linux・サーバー構築

- [ ] UTMでUbuntu Serverを構築
- [ ] Linuxの基本コマンドを習得
- [ ] ユーザー・グループ・権限管理
- [ ] SSHによるリモート接続
- [ ] NginxによるWebサーバー構築

### Phase 2：Docker・コンテナ

- [ ] Dockerのインストール
- [ ] Dockerイメージ・コンテナの理解
- [ ] Dockerfileの作成
- [ ] Docker Composeによる環境構築
- [ ] Webアプリケーションのコンテナ化

### Phase 3：AWS・クラウド

- [ ] AWSの基本サービスを理解
- [ ] EC2インスタンスの構築
- [ ] VPC・サブネット・セキュリティグループの設定
- [ ] EC2へのWebアプリケーションのデプロイ
- [ ] CloudWatchによる監視

### Phase 4：自動化・IaC

- [ ] Shell Scriptによる作業の自動化
- [ ] GitHub ActionsによるCI/CD構築
- [ ] Terraformの基本操作
- [ ] TerraformによるAWS環境の構築
- [ ] デプロイの自動化

## 📁 ディレクトリ構成

学習の進行に応じて、以下の構成を目指します。

```text
homelab-infrastructure/
├── README.md
├── docs/           # 構築手順・学習記録
├── scripts/        # 自動化スクリプト
├── docker/         # Docker関連ファイル
├── nginx/          # Nginx設定
└── terraform/      # Terraform構成ファイル
```

## 🎯 最終目標

自身で開発したWebアプリケーションをAWS上にデプロイし、構築・運用・監視・自動化までを実践することを目標としています。

最終的には、以下の構成を実現します。

```text
GitHub
   │
   ▼
GitHub Actions
   │
   ├── テスト
   ├── ビルド
   └── デプロイ
          │
          ▼
       AWS EC2
          │
          ▼
       Docker
          │
          ▼
       Next.js
          │
          ▼
     PostgreSQL
```

## 📝 学習記録

構築手順・発生したエラー・解決方法などは `docs/` に記録していきます。

設定内容だけでなく、技術の仕組みや採用理由も説明できるようにすることを意識しています。

---

このリポジトリは個人の学習・検証を目的としており、継続的に更新していきます。