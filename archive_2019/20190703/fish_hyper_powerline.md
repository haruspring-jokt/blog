# fish Hyperterm Powerlineでターミナルを構築する自分用メモ（Mac編）

## 最終的な見た目

背景はデスクトップの壁紙です。最近vapor waveが来てる。

自分用のメモなので、細かく手順は説明していません（参考にしたリンクを貼ります）。書いてみたらあまりにも雑なので、自分が思い出せるか否かを観点として追記するかも

![完成例](https://i.imgur.com/VYnYeGm.png)

## fishのインストール

シェルはいろいろとメリデメあると思いますが、とっつきやすいfishを使います。

[Macでfishを使う - Qiita](https://qiita.com/uneco/items/0dd86b5a1b7039b6b947)

## HackGen フォントのインストール

[【文字幅 半角3:全角5 も追加】Ricty を神フォントだと崇める僕が、フリーライセンスのプログラミングフォント「白源」を作った話 - Qiita](https://qiita.com/tawara_/items/374f3ca0a386fab8b305)

私の環境では2019-07-03現在でv1.0を使用しています。GitHubからzipをインストールして言われるがままにインストールすればOK。

## fishのテーマ設定

[Draculaテーマ](https://draculatheme.com/)が好きでエディタやIDEなどなんでもかんでもDraculaにしているので、（実はHypertermもちゃっかり設定済みだが）fishも例に違わず染め上げる。

```bash
fish_config
```

コマンドを実行するとブラウザーでテーマが選べるようになるので、Draculaを選び、画面上に反映されたら`Set Theme`ボタンを押下すると自動的に設定が完了する。

## PowerLineの設定

Gitリポジトリの情報をかっこよく表示する。

[fish shellが結構良かった話 - Qiita](https://qiita.com/hennin/items/33758226a0de8c963ddf#oh-my-fishtheme-bobthefish)

上の記事のうち、Fisherのインストールとプラグイン「oh-my-fish/theme-bobthefish」のインストールのみ行う。他は今の所必要なさそうなので無視した。

```bash
fisher add oh-my-fish/theme-bobthefish
```

## Hypertermのインストール

最初はiTerm2かなと思っていましたが、そういえばWindows10を使っていたときにHypertermがうまく動かずに断念したことを思い出したので、リベンジの意味も込めて使ってみることにしました。

[Macで快適な作業環境を構築する(Hyper編) - Qiita](https://qiita.com/ucan-lab/items/ed0687e9cd4a8ea7c76c)

インストールしたプラグインは以下の通り。

```bash
$ hyper ls
hyper-dracula
hyper-statusline
hyper-search
hypercwd
hyper-opacity
hyper-tab-icons-plus
hyper-pane
hyperborder
```

設定ファイルはフォントと透明度のみ変更する。

```js
    // font family with optional fallbacks
    // HackGen Console for Powerlineフォントのダウンロードは以下で説明
    fontFamily: '"HackGen Console for Powerline", Menlo, "DejaVu Sans Mono", Consolas, "Lucida Console", monospace',

    // 省略

    // 透明度の設定。微調整する
    opacity: 0.88,
  },
```

多分これくらいしかやってなかったはず…と信じて一旦終わりにする。Mac編としたがWin編はあるのか。
