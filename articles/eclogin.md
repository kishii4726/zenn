---
title: "【eclogin】ECS/EC2/Local docker containerにログインするためのツール"
emoji: "🦉"
type: "tech"
topics: ["aws", "ecs", "ec2", "docker", "cli"]
published: false
---

# eclogin
https://github.com/kishii4726/eclogin

ECS タスク/EC2 インスタンス/ローカル環境で動かしているdockerコンテナに簡単にログインするためのツールを作りました。
ECS Execでコンテナにログインする際は`クラスター名`、`タスクid`、`コンテナ名`、CLIでEC2インスタンスにセッションマネージャーでログインする際は`インスタンスID`、ローカル環境のコンテナにログインする際は`コンテナ名`もしくは`コンテナID`が必要ですがこれらを都度確認する必要がなくなります。

![eclogin_v0 0 19](https://github.com/user-attachments/assets/a10ad39c-4d33-4e81-8b92-da4fe19936ee)

[以前](https://github.com/kishii4726/ecsh)も同じようなツールを作成しましたがそちらの改良版になります。

# 概要
対話形式とオプションを指定して実行する2パターンがあります。
対話形式の場合は`eclogin ecs|ec2|local`を実行するだけで事前準備は不要です。

オプションを使用する場合は以下のように必要な情報を指定して実行します。
一部オプションのみ指定して残りは対話形式で選択するということも可能です。
```
# ECSの場合
Flags:
  -c, --cluster string     ECS cluster name
  -C, --container string   ECS container name
  -h, --help               help for ecs
  -p, --profile string     AWS profile name
  -r, --region string      AWS region name
  -s, --service string     ECS service name
  -S, --shell string       Shell to use for the session
  -t, --task-id string     ECS task ID
```

対話形式ですべての必要な選択肢を選んだ後に以下のようにオプション形式で実行した場合のコマンド例が出力されます。複数回同じコンテナやEC2インスタンスにログインしたい場合にはこちらをコピーして使うと次回実行時が楽になります。
```
eclogin equivalent command:
eclogin ecs --cluster test-cluster --task-id xxxxxxxx --container test-container --shell /bin/sh --region ap-northeast-1
```

またAWSコマンドで実行した場合のコマンド例も出力されるので本ツールを使用していない方へのコマンドの共有も簡単です。
```
If you are using awscli, please copy the following:
aws ecs execute-command \
        --cluster test-cluster \
        --task xxxxxxxx \
        --container test-container \
        --interactive \
        --command /bin/sh \
        --region ap-northeast-1
```

# インストール

:::message
実行環境には[session-manager-plugin](https://docs.aws.amazon.com/ja_jp/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html)がインストールされている必要があります
:::

[assets](https://github.com/kishii4726/eclogin/releases)からバイナリを取得して使用してください。

## Mac(arm)の場合
```
$ curl -OL https://github.com/kishii4726/eclogin/releases/download/v0.0.19/eclogin_Darwin_arm64.tar.gz

$ tar -zxvf eclogin_Darwin_arm64.tar.gz

$ sudo mv eclogin /usr/local/bin
```

## Linuxの場合
```
$ curl -OL https://github.com/kishii4726/eclogin/releases/download/v0.0.19/eclogin_Linux_x86_64.tar.gz

$ tar -zxvf eclogin_Linux_x86_64.tar.gz

$ sudo mv eclogin /usr/local/bin
```
