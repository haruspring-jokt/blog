# Python3 + AWS LambdaでSlackbotをつくる（自分用メモ）

## 概要

会社のSlackに忍ばせました。しかしどうやって実装したのかまったく覚えていないため、改めて参考サイトを見ながら書き残すことにした。自分が不便だなと感じたことを中心に増築していく予定。

## 完成形

ワークスペースにチャンネルが作成されたときにお知らせする。

![](https://i.imgur.com/mMIcRu3.jpg)

また、カスタムemojiが追加された際にも通知することにした。

![](https://i.imgur.com/odPlyoj.jpg)

## 参考

当時はいろんなところを見ていたのだけど、だいたいここに書いてあった…

[AWS(API Gateway + Lambda(Python)) + Slack APIを使ったBot作成](https://nmmmk.hatenablog.com/entry/2018/10/10/001548)

## 実装メモ

だいたい以下の順番で作業した（ような気がする）

- API GatewayとかLambdaのセッティング
- Slack Appの作成
- コーディング

簡単な構成図。

![](https://imgur.com/0Gk9v2V.jpeg)

### API GatewayとかLambdaのセッティング

#### Lambdaの作成

Lambdaを作成する。作成方法は「一から作成」でOK。関数の名前（任意）とランタイムを選択する。今回はPython3.7を選択した。

権限については、予め`AWSLambdaBasicExecutionRole`を付与したロールを作成し、それをアタッチする。

コーディングは後で行うので一旦そのまま。

#### API Gatewayの作成・設定

LambdaにAPI Gatewayをトリガーとして追加する。新規APIをセキュリティは`オープン`で作成し、さらに任意のAPI名、任意のステージ名を記入する。

これを追加してLambda画面で保存すると、API Gatewayの設定が確定する。

作成したAPI Gatewayの設定画面から、メソッドの作成を行う。POSTメソッドにしておく。「ANY」メソッドは削除してよい。統合タイプに「Lambda関数」、Lambda関数名にさっき作ったものを入力する（候補が出てくるはず）。

作成したPOSTメソッドを選択し、「アクション」から「APIのデプロイ」を選択する。ステージはさっき入力したものと同じ名前で書く。

デプロイすると「URLの呼び出し」が表示される。このアドレスをコピーする。

#### Lambdaの設定

Lambdaの画面に戻り、API Gatewayを選択し、2つある設定のうち、APIエンドポイントの上のグレーの文字列が途中`{/*/*}`となっている方を削除して、Lambda画面右上の「保存」を押下する。

### Slack Appの作成

Appはこのページ内で管理する。

https://api.slack.com/apps

`Create New App`から、新しいAppの名前と動作するワークスペースを選ぶ。

#### Bot Userの作成

サイドバーの`Bot Users`を選択し、任意の`Display name`と`Default username`を記入して、更に`Always Show My Bot as Online`をONにして保存する。

#### Event Subscriptionsの設定

Slackで発生したイベントをAPI Gatewayに送るため設定する。

設定をONにし、API Gateway設定時に生成された「URLの呼び出し」のアドレスを貼り付ける。疎通確認してくれるので、失敗したときはここまでの設定を見直す。

「Subscribe to Bot Events」に、トリガーとしたいイベントを追加する。今回は、チャンネルの作成とemojiの追加なので、それぞれ`channel_created`と`emoji_changed`を追加した。Save Changesを押下。

「Install App」へ移動し、「Install App to Workspace」を押す。確認がでるのでそのまま進むとBotが作成される。

### コーディング

Lambdaにコードを書いてく。必要に応じて環境変数を使用する。

一旦一部をマスクしてクソコードを全部載せる。コメントにだいたい書いたはず。

なお、コード右上の「ハンドラ」には`lambda_function.handle_slack_event`と入力する。これは`{ファイル名}.{開始メソッド名}`となっているので適宜変更しても大丈夫なはず。

```python
# -*- coding: utf-8 -*-
import os
import json
import logging
import urllib.request

# ログ設定
logger = logging.getLogger()
logger.setLevel(logging.INFO)


def handle_slack_event(slack_event: dict, context) -> str:
    # 受け取ったイベント情報をCloud Watchログに出力
    logging.info(json.dumps(slack_event))

    # Event APIの認証
    if "challenge" in slack_event:
        return slack_event.get("challenge")

    # ボットによるイベントの場合反応させないためにそのままリターンする
    # Slackには何かしらのレスポンスを返す必要があるのでOKと返す
    # （返さない場合、失敗とみなされて同じリクエストが何度か送られてくる）
    if is_bot(slack_event):
        return "OK"

    if is_channel_created_event(slack_event):
        post_channel_created(slack_event)

    if is_emoji_added_event(slack_event):
        post_emoji_added(slack_event)

    # メッセージの投稿とは別に、Event APIによるリクエストの結果として
    # Slackに何かしらのレスポンスを返す必要があるのでOKと返す
    # （返さない場合、失敗とみなされて同じリクエストが何度か送られてくる）
    return "OK"


def post_channel_created(slack_event: dict):
    # チャンネル作成イベントの通知

    # 作成されたチャンネルの名前を取得
    channel_name = slack_event.get("event").get("channel").get("name")

    # Slackにメッセージを投稿する
    post_message_to_slack_channel(
        f"新しいチャンネルが作成されました: #{channel_name}",
        os.environ["DIST_CHANNEL_CH_CREATED"])

    return


def post_emoji_added(slack_event: dict):
    # emoji追加イベントの通知

    emoji_name = slack_event.get("event").get("name")

    post_message_to_slack_channel(
        f"emojiが追加されました! :{emoji_name}: `{emoji_name}`",
        os.environ["DIST_CHANNEL_EMOJI_ADDED"])

    return


def is_bot(slack_event: dict) -> bool:
    # botからのメッセージの場合true
    return slack_event.get("event").get("subtype") == "bot_message"


def is_channel_created_event(slack_event: dict) -> bool:
    # 発生したイベントがチャンネル作成の場合true
    return slack_event.get("event").get("type") == "channel_created"


def is_emoji_added_event(slack_event: dict) -> bool:
    # 発生したイベントがemoji追加の場合true
    return slack_event.get("event").get("type") == "emoji_changed" and slack_event.get("event").get("subtype") == "add"


def post_message_to_slack_channel(message: str, channel: str, link_names="true"):
    # Slackのchat.postMessage APIを利用して投稿する
    # ヘッダーにはコンテンツタイプとボット認証トークンを付与する
    url = "https://slack.com/api/chat.postMessage"

    headers = {
        "Content-Type": "application/json; charset=UTF-8",
        "Authorization": "Bearer {0}".format(os.environ["SLACK_BOT_USER_ACCESS_TOKEN"])
    }

    # "link_name: true"によって#や@のリンク化を有効化
    data = {
        "token": os.environ["SLACK_APP_AUTH_TOKEN"],
        "channel": channel,
        "text": message,
        "username": os.environ["BOT_NAME"],
        "link_names": link_names
    }

    req = urllib.request.Request(url, data=json.dumps(data).encode("utf-8"), method="POST", headers=headers)
    urllib.request.urlopen(req)
    return
```

環境変数は以下のようになっている。上2つは察してほしい。下2つはSlack Appの設定画面から拾うことができるのでそれを記入する。

![](https://i.imgur.com/2G1zHcG.jpg)

コード下の方の設定は今の所変更していない。


