# Metabase内のデータをバックアップする

![](https://i.imgur.com/zS56nBV.jpg)

距離感ちかー

## 概要

モルックの個人練習記録のダッシュボードを作るために利用しているMetabaseについて、中のデータをバックアップする方法を自分用にメモしました。

## 過去記事

[https://m4usta13ng.hatenablog.com/entry/molkky_metabase:embed:cite]

[https://m4usta13ng.hatenablog.com/entry/molkky_moritacup_preview:embed:citeKV]

[https://m4usta13ng.hatenablog.com/entry/2020/01/30/223326:embed:cite]

## 経緯

自分はMetabaseをMetabaseコンテナとMySQLコンテナを使って利用しています。`docker-compose`はパスワードとか除きこんな感じ。ほぼどっかからのコピペです。

```yaml
version: "3"
services:
  metabase:
    container_name: metabase
    image: metabase/metabase
    ports:
      - "3000:3000"
    links:
      - mysql
  mysql:
    container_name: metabase_mysql
    build: ./mysql
    image: mysql:5.7.22
    ports:
      - "3306:3306"
    environment:
      MYSQL_DATABASE: molkky
      MYSQL_USER: molkky
      MYSQL_PASSWORD: molkky
      MYSQL_ROOT_PASSWORD: root
    volumes:
      - ./mysql/initdb.d:/docker-entrypoint-initdb.d
      - ./mysql/conf.d:/etc/mysql/conf.d
      - ./log/mysql:/var/log/mysql
```

で、特に問題意識はなかったのですが、ある時他のコンテナやイメージを掃除していたらうっかりこのコンテナも消して？しまい、慌てて再起動したら1ヶ月以上時を戻されてしまっていました。誰も傷つけないはずのツッコミに傷ついてしまったのです。

練習データの元はGoogle Driveのスプレッドシートに書いていたので無事でしたが、復旧の作業は面倒くさい…

ということで、ちゃんとバックアップしようとおもいざっくり調べて実行してみました。MetabaseとMySQL、それぞれの手順を載せておきます。

## 環境情報

- Mac OS 10.15.3
- Docker for Mac

## MySQLのバックアップ

https://weblabo.oscasierra.net/mysql-mysqldump-01/

上の記事を参考にしました。MySQLのバックアップ方法は色々ありますが、大量データではないのでひとまずは手軽な方法を選んだつもりです。

[MySQLでのバックアップ方法のまとめ — A Day in Serenity (Reloaded) — PHP, FuelPHP, Linux or something](http://blog.a-way-out.net/blog/2016/03/14/mysql-backup/)

```bash
# MySQLコンテナに入る
$ docker exec -it mysql bash

# docker-compose.yamlの`volume`に記述しているディレクトリにバックアップを出力することで、ホスト側からも取り出すことができる
$ mysqldump --single-transaction -u molkky -p molkky > /var/log/mysql/backup/filename.dump
```

以下のコマンドでリストアが可能です。

```bash
$ mysql -u molkky -p molkky < dumpファイル名
```

## Metabaseのバックアップ

Metabaseにはデータはありませんが、可視化方法の設定が残っているので、これが消えてしまうのも避けたいです。以下の記事を参考にして実施しました。

https://qiita.com/shotanue/items/dd036c9ed6960a9c3924

```bash
# コンテナの確認
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
be72613019f5        metabase/metabase   "/app/run_metabase.sh"   3 days ago          Up 2 days           0.0.0.0:3000->3000/tcp   metabase
cd21a0a8b5f6        mysql:5.7.22        "docker-entrypoint.s…"   3 days ago          Up 2 days           0.0.0.0:3306->3306/tcp   metabase_mysql

# 特にマウントせずにコンテナを立ち上げている場合はルート直下にmetabase.dbというディレクトリがある

docker exec -it metabase ls -l /metabase.db
total 2048
-rw-r--r--    1 metabase metabase   2093056 Feb  3 13:38 metabase.db.mv.db
-rw-r--r--    1 metabase metabase       187 Jan 30 15:16 metabase.db.trace.db

# docker cp コマンドでホスト側にファイルをコピーしてバックアップできる。

docker cp metabase:/metabase.db ./metabase/backup/
```

Dockerを遊びでしか使っていないので、`docker cp`コマンドは初見。以下の記事を参考に書きました。

https://noumenon-th.net/programming/2019/03/29/docker-cp/

というわけで、これでバックアップのとり方がわかりました〜やっとかよ〜
