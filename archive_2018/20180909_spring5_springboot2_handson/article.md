# 「Spring 5 & Spring Boot 2の初心者向け入門ハンズオン」をやった

自宅でちゃちゃっとできるハンズオンだったのでやってみた。

## 参考記事一覧

業務ではSpringを扱った経験が殆どなく（あったとしてもかなりアレンジがされていたりして、Springを意識して開発することは無く、現在もバリバリ開発業務に関わることができていないので、思い出すきっかけが欲しいなあというところで、Qiita記事を見つけた。

[Spring 5 & Spring Boot 2の初心者向け入門ハンズオンを公開しました - Qiita](https://qiita.com/suke_masa/items/b654423099ee32c30bd2)

この記事と、記事にリンクされている準備手順に従ってざっくり環境を構築するだけでハンズオンを行うことができる。一応、本記事にも参照したページを一通り並べておく。

[Spring 5 & Spring Boot 2ハンズオン準備手順 - Qiita](https://qiita.com/suke_masa/items/44463518fdbbc13e0087)

この記事を見て必要なものを揃えていく。

[MasatoshiTada/spring5-boot2-handson: Spring 5 & Spring Boot 2ハンズオンの資料です。準備手順はこちら→](https://github.com/MasatoshiTada/spring5-boot2-handson)

ハンズオン用リポジトリ。README内にハンズオン手順へのリンクがあるので、それの指示通りに進める。

手順の「`TODO`」とソースコード内の`TODO`が対応しているので、対象のソースを探すのにはそこまで苦労しないはず。

## 私が使用した環境

OSはWindows10を使用。

- eclipse（`Pleiades Photon`を使用）
    - IntelliJ IDEAのCommunity版も一応持って入るが、まだ使い慣れていないため今回はeclipseを使用。
    - プラグインとして`STS`（`Spring Tool Suite`）、`propedit`（プロパティエディタ）、`Thymeleaf Plugin`を追加。いずれもマーケットプレイス経由で取得できる。
- JDK 8
- curlコマンド（オプションの演習でのみ必要）
    - Windowsの場合は http://www.paehl.com/open_source/ から「CURL 7.xx.x」の「Download WITHOUT SSL」を選択してダウンロードし、環境変数`PATH`を設定する。
    - `PATH`を通せばコマンドプロンプト上では問題なく動く。ただ、PowerShellを使用する場合はひと手間必要になる。
        - [curlをWindows10にインストールする - Qiita](https://qiita.com/shiftsphere/items/5831cd2863689c41f993)
- jqコマンド（オプションの演習でのみ必要）
    - Windowsの場合は https://stedolan.github.io/jq/download/ からダウンロードし、環境変数`PATH`を設定する。
    - `jq-win64.exe`というファイル名でダウンロードされるので、これを`jq.exe`に変更する。

ほか、GitHubからハンズオン用のリポジトリを`Clone`するため、これに関する各種準備が必要となる。また、`curl`コマンド、`jq`コマンドはオプション演習のみの使用ではあるが、普段の開発にも何かと役立つと思う。

また、インメモリデータベースの`H2`を使用しているため、**自分でDB環境を用意する必要はない**。

## 感想

最後のオプション演習以外を終了した時点。

ハンズオンの流れとしては、まずSpring5のコーディングを行う。Springの設定には大まかに`xml`と`Java Config`の2種類があるが、ハンズオンでは主に`Java Config`によって進めていく。

コーディングの`TODO`を終えたあとは、予め用意されているテストコードを実行することで確認を行う。このテストコードは（従来よりも利便性が高まっている）`JUnit5`によって書かれており、`JUnit3`、`4`のチュートリアルやサンプルコードくらいしか見たことがなく、かつテストコードを有効に使用するプロジェクトに参加したことがない身としては発見が多かった。

一通りSpring5の`TODO`を消化すると、次はSpring Boot2のハンズオンに移行する。ここでの`TODO`は、ここまでの`TODO`で作成したSpring5の各種コード、（場合によっては）設定用のファイルそのものを削除する作業が大半となる。つまり、Spring Boot2に移行することで、設定作業が大幅に省かれることになる、ということを意味している。

オプション（応用編）の`TODO`では、準備していた`curl`, `jq`の各コマンドを使用してSpring Bootの`Actuator`機能を実装する。`Actuator`を使用して何がどう便利になるのか、ということに関しては幾つかリンクを貼っておく。

[Spring Boot Reference Guide 2.1.0.BUILD-SNAPSHOT](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#production-ready-endpoints)

[SpringBoot Actuator - Qiita](https://qiita.com/MariMurotani/items/01dafd2978076b5db2f3)

[Spring Boot Actuatorを使ってヘルスチェックする - Qiita](https://qiita.com/yoshidan/items/a6515d37784c6779efc5)

最後の`TODO`には、ここまでで作成したアプリケーションに受講者自身で機能を追加する課題が用意されている。ノーヒントだが特別難しいものではないので、来週はこれを進めようと思う。

