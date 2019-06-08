# Slackにカスタム絵文字を一括登録する

## 概要

Slackへのカスタム絵文字追加作業って面倒だな－と思っていたので、前から気になっていた一括登録ツールを利用してみました。そうしたところ、ちょっとしたエラーが出て回り道することになったので共有します。

## 環境

node環境が必要です。

```
Windows 10 Home
$ node -v
v10.15.3
$ npm -v
6.4.1
```

## 手順

### emojipacksのインストール

github.com/lambtron/emojipacks

```
$ npm install -g emojipacks
```

これで後は`README.md`に書いてある手順を実行するだけだったのですが...

（一部マスクしています）

```
$ emojipacks.cmd
Slack subdomain: hogehoge
Email address login: hogehoge@gmail.com
Password: **********
Path or URL of Emoji yaml file: https://raw.githubusercontent.com/lambtron/emojipacks/master/packs/mario-8bit.yaml
Starting import
Got tokens
Logged in
Uh oh! Error: Login error: could not get emoji upload crumb for https://harukawatips.slack.com
(node:41512) UnhandledPromiseRejectionWarning: Error: Login error: could not get emoji upload crumb for https://harukawatips.slack.com
    at Slack.emoji (C:\Users\XXXXX\AppData\Roaming\npm\node_modules\emojipacks\lib\slack.js:162:34)
    at Slack.emoji.next (<anonymous>)
    at onFulfilled (C:\Users\XXXXX\AppData\Roaming\npm\node_modules\emojipacks\node_modules\co\index.js:65:19)
    at process._tickCallback (internal/process/next_tick.js:68:7)
(node:41512) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled with .catch(). (rejection id: 1)
(node:41512) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.
```

このようにログインに失敗してしまうみたいです。

### emoji-importのインストール

代替手段としてemoji-importというツールを利用すると良いという情報がemojipacksのissueに書かれていたので、以下をインストール＆実行します。

```
npm i -g slack-emoji-import
slack-emoji-import path/to/emoji-pack.yaml

$ slack-emoji-import C:\Users\81902\Downloads\emojipacks\packs\twitch.yaml
prompt: Slack Host:  hogehoge
prompt: Slack login email:  hogehoge@gmail.com
prompt: Slack password:
prompt: Show browser:  (false) true
```

上記のように、プロンプトにしたがって追加対象のワークスペース、ログイン情報を入力すればあとはpuppeteerという自動ブラウジングしてくれる方がせっせと追加してくれるみたいです。`prompt: Show browser:`を`true`にするとその様子が見られます。愛らしいですが基本`false`でOKです。

### 追加するemoji情報について

どのemojiを追加するということに関しては、上のコマンドでもしれっと書いていますがyamlファイルを指定します。これは自分で作らなければならないわけではなく、すでにネット上にあがっているものを使います。

もちろん、自分でyamlを作ることもできます。yamlの構造はシンプルなのでそこまで難しい作業ではありません（面倒だけど）。


