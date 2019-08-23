# Yokoso

- Slack とビル来客システム https://srd-gate.com/03/login.cgi を連携させるツール
- 概要は [SmartHR Tech Blog](https://tech.smarthr.jp/entry/2019/06/28/134701)

<img src="https://github.com/kufu/yokoso/blob/images/sample_animation.gif?raw=true" width="480px">

# セットアップ

## サーバサイド

### 前提

- 以下を使っています
  - Heroku
  - Redis
- 以下の環境変数は後で設定するので空で大丈夫です
  - `SLACK_TOKEN`
  - `MAIL_ADDRESS_WEBHOOK`

### 起動

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

ローカル実行はこちら

```
heroku local -e .env
```

## Slack App 作成

- Slack App を新規作成して各種設定をする
- https://api.slack.com/apps
- <img src="https://github.com/kufu/yokoso/blob/images/slack_app_create.png?raw=true" width="480px">

### Interactive Components

- 設定をオンにしてサーバのアドレスを入力する
- `https://{{server_url}}/app/interactive`
- <img src="https://github.com/kufu/yokoso/blob/images/slack_app_intractive.png?raw=true" width="480px">

### Slash Commands

- スラッシュコマンドの設定を追加する
- `https://{{server_url}}/app/dialog`
- <img src="https://github.com/kufu/yokoso/blob/images/slack_app_dialog.png?raw=true" width="480px">

### OAuth & Permissions

- 権限を追加する
    - channels:history
    - chat:write:bot
    - users:read
    - commands
      - スラッシュコマンドを設定していれば自動で追加されているはず
    - <img src="https://github.com/kufu/yokoso/blob/images/slakc_app_scope.png?raw=true" width="480px">
- Token は後でサーバサイド環境変数へセットする
  - <img src="https://github.com/kufu/yokoso/blob/images/slack_app_token.png?raw=true" width="480px">

### App Install

- ワークスペースへのインストールを忘れずに実施

## Email Webhook

- メール受信 -> Webhook を作成
- 受信したメールの body を json で post すれば OK
- サービスは何でも良い
    - zapier なら [コチラ](https://zapier.com/app/editor/template/9205?selected_apis=ZapierMailAPI%2CWebHookAPI)
- Webhook 用メールアドレスは後でサーバサイド環境変数へセットする

## サーバーサイド環境変数の再設定

- 以下の環境変数を設定する
  - `SLACK_TOKEN`
  - `MAIL_ADDRESS_WEBHOOK`
- リソース割り当て
  - <img src="https://github.com/kufu/yokoso/blob/images/heroku_dynos.png?raw=true" width="480px">
