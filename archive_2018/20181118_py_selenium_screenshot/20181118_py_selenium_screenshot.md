# Selenium with Python: Chrome driverでWeb特定部分のスクリーンショットを取得する

## はじめに

### 概要

PythonでSeleniumをつかって、特定要素のスクリーンショットを保存する方法を書きます。

### 使用した環境

- OS
    - windows 10
- python
    - 3.7.1
- selenium
    - 3.141.0
- chromeDriver
    - ChromeDriver 2.43
- chrome
    - 70.0.3538.102（Official Build）（64 ビット）

### 説明しないこと

- seleniumの基本的な使い方
- webdriverの準備
- chrome以外のブラウザーでの動作

## 方法

### seleniumのpip install

以下はPowerShellでの実行例です。seleniumの使用自体はこれだけでOKです。

```powershell
(env) PS C:\python\pywork\selenium> pip install selenium
```

### seleniumライブラリのインポート

seleniumを使用する場合はだいたいこの2行を書きます。理由は分かっていないのですがどこで調べてもこうなっています。

```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
# seleniumには関係ないけど今回は以下も使用します
import os
import time
```

### webdriverの起動

chromeとwebdriverのパスは直接指定しても問題ないですが、今回はそれぞれ環境変数から取得します。コメントアウトしていますが、headlessでもスクリーンショットの取得は可能です。

```python
options = Options()
# chromeのパスを指定する。今回は環境変数から取得
options.binary_location = os.environ['CHROME_BINARY_LOCATION']

# headlessで使用する場合は以下の2行を利用する。
# options.add_argument('--headless')
# options.add_argument('--disable-gpu')

# webdriverを起動する。引数executable_pathにwebdriverのパスを指定する。
# こちらも環境変数から取得
driver = webdriver.Chrome(
    options=options, executable_path=os.environ['CHROME_DRIVER_PATH'])

# ドライバが設定されるまでの待ち時間を設定する。
driver.implicitly_wait(10)
```

### 画像オブジェクトの取得

画像を取得するのに特別な操作は必要ありません。seleniumでよく使う要素取得の方法を用いて、取得した要素の`screenshot_as_png`を利用するだけでOKです。

```python
#トップ画面を開く。
driver.get('http://m4usta13ng.hatenablog.com/')

# ローディング待ち
time.sleep(3)

# タイトル部分の画像オブジェクトを取得する。
png = driver.find_element_by_id('blog-title-inner').screenshot_as_png
```

### 画像の保存

Pythonで一般的に使っているファイル操作方法がそのまま使えます。

```python
# バイナリ書き込みモードで`open`し、保存する。
with open('./img.png', 'wb') as f:
    f.write(png)
```

### ドライバーの終了

使用しなくなったwebdriverは終了する必要があります。

```python
# ドライバーを終了
driver.close()
# driver.quit()
```


## コード全体

```python
# -*- coding: utf-8 -*-
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
import os
import time

options = Options()
# chromeのパスを指定する。今回は環境変数から取得
options.binary_location = os.environ['CHROME_BINARY_LOCATION']

# headlessで使用する場合は以下の2行を利用する。
# options.add_argument('--headless')
# options.add_argument('--disable-gpu')

# webdriverを起動する。引数executable_pathにwebdriverのパスを指定する。
# こちらも環境変数から取得
driver = webdriver.Chrome(
    options=options, executable_path=os.environ['CHROME_DRIVER_PATH'])

# ドライバが設定されるまでの待ち時間を設定する。
driver.implicitly_wait(10)

#トップ画面を開く。
driver.get('http://m4usta13ng.hatenablog.com/')

# ローディング待ち
time.sleep(3)

# タイトル部分の画像オブジェクトを取得する。
png = driver.find_element_by_id('blog-title-inner').screenshot_as_png

# 画像を保存
with open('./img.png', 'wb') as f:
    f.write(png)

# ドライバーを終了
driver.close()
# driver.quit()
```

## 結果

今回はブログのタイトル部分を取得しました。



## 備考

### driverの終了方法について

下記リンクを参照。`driver.quit()`をしないままだとセッションが残存し、メモリリークする可能性があります。

[What is close() and quit() commands in Selenium Webdriver? | Zyxware Technologies](https://www.zyxware.com/articles/5552/what-is-close-and-quit-commands-in-selenium-webdriver)

### ウインドウサイズの変更

画面サイズによって要素も変わってくる場合は、webdriverのサイズを変更することで対応できます。

```python
#カレントウインドウのサイズを幅:1280,高さ:720に設定する
driver.set_window_size(1280, 720)
```

## 参考

[Python: Selenium + Headless Chrome で Web ページ全体のスクリーンショットを撮る - CUBE SUGAR CONTAINER](https://blog.amedama.jp/entry/2018/07/28/003342)

[set_window_size-Python](http://www.seleniumqref.com/api/python/window_set/Python_set_window_size.html)

[7. WebDriver API — Selenium Python Bindings 2 documentation](https://selenium-python.readthedocs.io/api.html)
