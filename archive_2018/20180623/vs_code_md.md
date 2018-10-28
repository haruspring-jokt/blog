# VS code の拡張機能を使ってMarkdownファイルからPDF/HTMLドキュメントを作成する(Windows)

作業の整理やメモを取る際は、txtで書いていてもよく分からなくなってくることが多くなってきてしまったため、最近はMarkdown形式で書くようにしている。ブログもMarkdownで書くことができるので覚えていて損はないはず。

ただ、Markdownをそのまま人に見せるのはあまり親切でないことがある（相手はMarkdownビューアーを用意していない場合など）ため、PDFやHTML形式に変換しておく。

## 目次

<!-- TOC -->

- [VS code の拡張機能を使ってMarkdownファイルからPDF/HTMLドキュメントを作成する(Windows)](#vs-code-の拡張機能を使ってmarkdownファイルからpdfhtmlドキュメントを作成するwindows)
    - [目次](#目次)
    - [注意](#注意)
    - [ツール](#ツール)
    - [設定](#設定)
        - [自分の環境（2018-06-23 19:45:09現在）](#自分の環境2018-06-23-194509現在)
    - [ファイルの生成](#ファイルの生成)
        - [PDF生成時の注意](#pdf生成時の注意)
        - [出力例](#出力例)

<!-- /TOC -->

## 注意

ExcelがどうとかPDFがどうとかの宗教戦争は別件です。

## ツール

- Visual Studio Code
    - 各種拡張機能（検索すれば見つかります）
        - Markdown PDF **必須**
        - Markdown All in One （文字通り様々な機能を搭載）
        - Markdown TOC （本記事の「目次」のようなインデックスを作成してくれる）
        - Markdown Table Formatter （テーブルを自動生成してくれる）

`Markdown PDF`以外は必須の拡張機能ではないが、Markdownを書く際には色々と便利なのでインストールしておく。

## 設定

設定画面で`markdown`、`markdown-pdf`とそれぞれ検索し、環境に合わせて設定する。

### 自分の環境（2018-06-23 19:45:09現在）

使用頻度の高くない自宅の環境なのでそこまでガチガチに組まれていないが、一応…

```json
// 編集中のmdファイルを保存した際に、自動的にpdf(他の設定次第でhtmlなども可能)を生成する。
"markdown-pdf.convertOnSave": true,
```

```json
// 自動生成の出力ファイルを選択する。
"markdown-pdf.type": "html",
```

```json
// 出力ファイルのCSSを設定する。デフォルトでもある程度設定されているが、これ次第で自由に見た目は変更できる。
// 自分はデフォルトのcssファイルを微修正したものを指定している。
// 日本語のフォントが設定されていないため、デフォルトのまま設定すると漢字などがおかしくなる（いわゆる中華フォント）ので、この部分の設定は必須。
// デフォルトのCSSは`C:\Users\{ユーザー名}\.vscode\extensions\yzane.markdown-pdf-1.2.0\styles`に配置されている。
"markdown-pdf.styles": [
    "C:\\Users\\{ユーザー名}\\vscode_settings\\yzane.markdown-pdf-1.0.1\\styles\\custom_markdown-pdf.css"
],
```

```json
// vs code内でプレビューする際に使用するフォントを選択する。デフォルトである程度設定されているが、
// 守備範囲が広い「源真ゴシック等幅」を選択している。
"markdown.preview.fontFamily": "'源真ゴシック等幅',-apple-system, BlinkMacSystemFont, 'Segoe WPC', 'Segoe UI', 'HelveticaNeue-Light', 'Ubuntu', 'Droid Sans', sans-serif",
```

```json
// これもプレビュー用の設定。
"markdown.preview.fontSize": 13,
```

この他にも様々。プレビューのCSSと出力ファイルのCSSは合わせたほうが良いかもしれない。

エディタは一番設定が楽しい。そしてあっという間に時間がなくなってしまう。

## ファイルの生成

設定で`"markdown-pdf.convertOnSave": true`を設定していない場合は、手動で生成する。

右クリックまたはコマンドパレット（`Ctrl + Shift + P` or `F1`）から、`Markdown PDF: Export(拡張子名)`を選択すると、生成を開始。成功すると元のMarkdownファイルと同じディレクトリに生成したファイルを保存が保存される。

### PDF生成時の注意

PDFの生成は`Chromium`のPDF印刷機能を用いているので、初回の生成時にはインストール作業などが行われるので、待ち時間が発生する。

また、プロキシ設定をしている環境では、`Chromium`のインストールに失敗したりしてPDFが生成できないので、プロキシ設定をする必要がある。

vs codeの`settings.json`に設定を追加することもできるが、Windows環境であれば環境変数に設定してしまえば、vs codeも受け継いでくれるので、支障がなければ環境変数に設定する。

PDF形式以外の生成はプロキシに関係なく実行できる模様。

### 出力例

本記事の編集中mdファイルから出力してみる。

<!-- 画像はhatenaで追加 -->

生成時点ではフォントくらいしか弄っていないが十分見られる形になっている。ヘッダーやフッターの表示方法も`settings.json`で設定出来るようになっているので、状況に合わせて変更できる。
