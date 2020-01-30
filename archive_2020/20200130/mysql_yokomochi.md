# Metabaseの円グラフを表示するためにMySQLで横持ち？をする

## 経緯

以下の過去記事にも書きましたが、モルックというスポーツの個人成績データを集めて、Metabaseという可視化ツールでダッシュボードを作っています。

今回は新たに円グラフを追加しようとしましたが、そのために表題の考えが必要だったのでメモがてら記事にしておきます。

### 過去記事

[https://m4usta13ng.hatenablog.com/entry/molkky_metabase:embed:cite]

[https://m4usta13ng.hatenablog.com/entry/molkky_moritacup_preview:embed:cite]

## やりたいこと

以下のような円グラフをMetabase上に作ろうと思いました。

![](https://i.imgur.com/VETPWhs.jpg)

本線ではないものの一応説明をすると、モルックでは目標となるピン（スキットルと言います）を狙って投げるのですが、その場合の「ミス」を目標を外すことと定義し、その内訳として前後左右の4方向、また地面でイレギュラーバウンドをしたことにより外す、という5種類に分類しています。

- OVER: スキットルの上を越えてしまう
- SHORT: スキットルに届かない
- LEFT: スキットルの左に逸れる
- RIGHT: スキットルの右に逸れる
- IRREGULAR: コースはあっていたのに変則的なバウンドをして外れる

で、本線に戻る

MetaBaseでは、円グラフを作るのに以下のような形でデータを取得する必要があるみたいです。

![](https://i.imgur.com/uKaiuyr.jpg)

ですが、データ入力をしていたスプレッドシートは以下のようになっていました

![](https://i.imgur.com/Jb322YC.jpg)

このため、このまま`GROUP BY`で集計してもほしい形にはなりません

## とりあえず

色々方法はあるっぽいですが、データ量も多くないし手っ取り早かったので以下で対応

```sql
SELECT
    "OVER",
    SUM(MISTAKE_OVER)
FROM
    games
UNION
ALL
SELECT
    "SHORT",
    SUM(MISTAKE_SHORT)
FROM
    games
UNION
ALL
SELECT
    "LEFT",
    SUM(MISTAKE_LEFT)
FROM
    games
UNION
ALL
SELECT
    "RIGHT",
    SUM(MISTAKE_RIGHT)
FROM
    games
UNION
ALL
SELECT
    "IRREGULAR",
    SUM(MISTAKE_IRREGULAR)
FROM
    games
```

```
+-----------+-------------------+
| OVER      | SUM(MISTAKE_OVER) |
+-----------+-------------------+
| OVER      |                 5 |
| SHORT     |                 2 |
| LEFT      |                 6 |
| RIGHT     |                13 |
| IRREGULAR |                 6 |
+-----------+-------------------+
```

ミスの種類1個ずつに対してSELECTをして最後にくっつける。明らかに良くはないんだけど最低限やりたいことはできました。

## 参考

- https://teratail.com/questions/190023
- https://teratail.com/questions/87232
- https://qiita.com/ka215/items/fd8f4291b32a9aa96149