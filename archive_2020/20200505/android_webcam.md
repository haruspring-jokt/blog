# 古いAndroidをWebカメラにする DroidCam

最近はZoomやリモートワーク、テレワーク需要のおかげでWebカメラが高騰しています。Amazonでは2倍以上の値段に設定されていることも珍しくないので、欲しくてもなかなか手を出しにくい…

私も最近Webカメラが欲しいなーと思っていたのですが（ノートPCにカメラはついているのですが、ノートは閉じてディスプレイにつなげているため使えない）、いつテレワーク需要が下火になるかわからない状況で高騰価格で入手する気にもならず…

そんなときに、AndroidをWebカメラ代わりに使えることが分かったので実際に使ってみました。ちょっと設定が面倒ですが、それさえできればかなり便利に使えるので記録しておきます。

## 使用するもの

- Androidスマホ
  - Webカメラとして使っているときは電話として使うことはほぼほぼ不可能なので、できれば機種変更で使わなくなった古い機種があると良いです
- Android用アプリ「[DroidCam](https://play.google.com/store/apps/details?id=com.dev47apps.droidcam&hl=ja)」
  - Webカメラとして使用するAndroidにこちらをインストールします
- （できれば）AndroidとPCを接続するUSBケーブル
  - なくても接続できますが、できればあると安定して使えます

## 手順

手順はざっくり、

- Android側の準備
- PC側の準備
- カメラテスト

の流れです。

ちなみに、DroidCamとPCを接続する方法として「Wi-fi接続」「USB接続」のいずれかがありますが、今回はUSB接続の方法を記載します。理由としては、自分の環境で使っていた際にWi-fi接続だと途切れることが度々あったためです。人によっては特に切断せず使えるかもしれません。

## Android側の準備

### DroidCamのインストール

Webカメラとして使用したいAndroidに[DroidCamnoアプリ](https://play.google.com/store/apps/details?id=com.dev47apps.droidcam&hl=ja)をインストールします。

インストール後開き、

- カメラとマイクの許可
- 初回のチュートリアルなどを確認（そのままボタン連打でOK）

で準備完了となります。

![](https://i.imgur.com/fnbmv64.jpg)

スクショ右上から設定を変更することも可能ですが特に変更する必要はありません。内容を見てわかる方のみいじってみてください。

### Androidをデバッグモードにする

PCとUSB接続するためにAndroidをデバッグモードにする必要があります。この方法については以下の記事が詳しいので、こちらを参考にしてください。

[AndroidでUSBデバッグを有効にする方法! 設定を解除する手順やリスクを解説](https://sp7pc.com/google/android/39763)

厄介なのが、見た目や画面項目の並びなどが機種によって微妙に違うところです。そのため、`{自分の機種 デバッグモード}`あたりで検索した方が早いかもしれません。

ちなみに私の機種（ASUS Zenfone3）だとこんな感じ。`USBデバッグ`がONになっていれば準備OKです。

![](https://i.imgur.com/XOmDZ7v.jpg)

## PC側の準備

PC側にも専用のソフトをインストールします。まずは以下のサイトにアクセス。

[dev47apps.com/](http://www.dev47apps.com/)

画面中のWindowsロゴ画像をクリックし、

![](https://i.imgur.com/tHYrgQx.png)

`DroidCam Client {バージョン番号}`とある2つのボタンのうち、Androidのロゴだけある方をクリックします（右側はAndroidとiOSに両対応？のような感じで書かれていますが、今回は私がiOSを持っていないためスキップします。iPhoneをカメラとして使ってみたい、という方は是非ためしてみてください）。

![](https://i.imgur.com/v14njRU.png)

インストーラがダウンロードされるのでそれを実行します。デフォルト設定のままインストールをすすめてOKです。途中、デバイスをインストールするか聞かれますが、必要なのでインストールします。

## カメラテスト

いよいよカメラを使えるか試します。

PC側の「DroidCam Client」を起動し、左上まんなかにあるUSB接続ボタンを押し、USB接続モードにしておきます。

![](https://i.imgur.com/yoqHJnu.png)

「DroidCam Port」は`4747`のままでOK。`Video`、`Audio`のチェックボックスは、使いたいものにチェックします（私は音声は別途マイクから取り込むためOFFにしています）。

Android側のDroidCamアプリを起動しておき、さらにUSBデバッグがONになっている状態にしておきます。

準備ができたら、PC側の`Start`ボタンを押します。設定がうまくいっていればカメラが起動します…

![](https://i.imgur.com/02WOgc0.png)

こんな感じでカメラのプレビューが表示されたら成功です。`Stop`ボタンでカメラは停止します。使用するときは、Startしっぱなしにしてウィンドウ自体は最小化すると良いでしょう。

### 通話ソフトなどで使用する方法

あとはこの映像をZoomなどのアプリで使えるようにします。専用のWebカメラやノートPC内蔵のカメラと同じく、使用するだけです。`DroidCam Source {数字}`を選択します。このとき、1、2、3と複数存在することがありますが、片っ端から選んで上のプレビュー表示されればOKです。

![](https://i.imgur.com/Qphom1A.png)

ちなみに配信ソフトのOBSで使う際は、

- ソースの追加→「映像キャプチャデバイス」
- デバイスで`DroidCam Source {数字}`を選択
  - 解像度/FPSタイプは既定のままでOK
  - ほかもデフォルト設定のままでOK

で使うことができます。

## 補足 有料版アプリの機能について

「DroidCam」は無料版と「DroidCamX」という有料版のアプリが用意されていて、2020/5/5現在で有料版は500円となっています。有料版の機能は

> - Chat using "DroidCam Webcam" on your computer, including Sound and Picture.
> - Connect over Wifi or USB cable*.
> - 720p video in HD Mode.
> - 'FPS Boost' setting, up to 2x more FPS**.
> - Use other (non camera) apps while DroidCamX is running in the background.
> - IP webcam MJPEG access (access camera via a browser or from another phone/tablet/etc).
> - Camera controls: camera flash, auto focus, zoom and more.
> - Save still frames to SD Card on mobile device, or on PC via Windows Client.
> - Extended controls on the Windows Client: Mirror, Flip, Brightness, Contrast, etc.
> - Simple and efficient: Designed to save battery and space as much as possible!

となっていて、一部を抜粋すると

- 720pの解像度で撮影可能（無料版は480p）
- PCの「DroidCam Client」でカメラのズームボタン、オートフォーカスボタン、反転、回転ボタンを使える
- カメラ画面をスクショして保存できる

などの機能が追加されます。個人的には2番目の機能は想像以上に便利でした。

### 720pの画質は不要

有料版の中で目を引くのは「720pの解像度で撮影可能」ですが、個人的には不要です。なぜなら、

- 480pでも十分キレイに撮れているから
- 高画質になるとPCが重くなり、スムーズに作業できなくなるから

といった理由からです。無料版で使ってみてもうちょっと使い込みたいな－と思ったら有料版に手をだせば良いと思います。500円なのでWebカメラ自体の価格と比べたら雲泥の差ですね。

## 参考

- [Android端末をPCのWEBカメラに！「DroidCam」手軽にOBSで配信も！ | 窓辺の自作PC雑書](https://www.madosyo.com/?p=2270)
  - Wi-Fiで接続する方法、PC側にソフトをインストールせずChromeなどのブラウザから接続する方法についても記載があります。
- [No devices detected Droidcam Error How to solve No devices detected Error in Droidcam - YouTube](https://www.youtube.com/watch?v=EtesoYl7xpU)
  - 作業中に`No devices detected`というエラーが表示されたので、こちらのYouTube動画を見ながら対処しました。同様のエラーが出なかった場合は特に対処不要です。


