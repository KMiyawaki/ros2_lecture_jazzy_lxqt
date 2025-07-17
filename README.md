# ROS2プログラミング開発環境

このページでは[大阪インテリジェントロボティクス株式会社](https://www.osakarobo.co.jp/)が開催する`ROS2`セミナーで使用する開発環境のインストール方法をご案内しています。

本開発環境で使用しているソフトウェアは世間一般で広く利用されているものです。また、構築した環境も担当講師が実際に動作確認してはいますが、残念ながら全てのPCにおいて安全な動作を保証することはできません。  
掲載された内容によって生じた損害等の一切の責任を負いかねますので、ご了承ください。不安がある場合は重要なファイル等のバックアップをされた上で実施されることをお勧めいたします。

- [ROS2プログラミング開発環境](#ros2プログラミング開発環境)
  - [必要なソフトのインストール](#必要なソフトのインストール)
  - [Docker イメージのインストール](#docker-イメージのインストール)
  - [起動](#起動)
  - [終了](#終了)
  - [ROS2の動作確認](#ros2の動作確認)
    - [コマンドターミナルの起動とTurtlesim](#コマンドターミナルの起動とturtlesim)
    - [talkerとlistener](#talkerとlistener)
    - [ロボットシミュレータ](#ロボットシミュレータ)
  - [VSCodeとの連携](#vscodeとの連携)
  - [VNCクライアントの利用（オプション）](#vncクライアントの利用オプション)

## 必要なソフトのインストール

下記のソフトをインストールしてください。

1. [`Docker Desktop for Windows`](https://www.docker.com/products/docker-desktop/)
    - インターネット上に[インストール方法の日本語解説](https://docs.docker.jp/docker-for-windows/install.html)もあります。
    - `WSL2(Windows Subsystem for Linux 2)`を併用する方法でインストールしてください。
2. `Visual Studio Code`
    - 以降、VSCodeと表記します。
3. `WEB`ブラウザ
    - 種類は任意ですが、本ページでは`Chrome`を前提にご説明します。

## Docker イメージのインストール

Wifi通信の良好な環境にて実施してください。

[ros2_lecture_humble-202401.zip](https://github.com/KMiyawaki/ros2_lecture_humble/archive/refs/tags/202401.zip)をダウンロードし展開してください。ここでは`%USERPROFILE%\Documents\ros2_lecture_humble-202401`に展開したとします。

なお、`%USERPROFILE%`は`Windows`の環境変数で`C:\Users\[ログインユーザ名]`に置き換えられます。  
`Windows`のファイルエクスプローラーでアドレス欄に`%USERPROFILE%\Documents`と入力してエンターキーを押し、どのフォルダが開くかを確認してみてください。
`C:\Users\[ログインユーザ名]\Documents`が開くはずです。

![2023-08-31_090809.png](./images/2023-08-31_090809.png)

展開後のフォルダには以下のファイルが含まれています。

```text
    docker-compose.yml
    README.md
    images # 説明文書の画像
```

まず[Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/)が**起動していることを必ず確認してください**。

以下の画面のようにWindowsのタスクトレイに`Docker Desktop for Windows`のアイコンが表示されていることを確認し、次の手順に移ってください。  
アイコンの表示が無ければ、`Windows`の[アプリ検索](https://support.microsoft.com/ja-jp/windows/%E3%81%99%E3%81%B9%E3%81%A6%E3%81%AE%E3%82%A2%E3%83%97%E3%83%AA%E3%81%A8%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%A0%E3%82%92%E6%A4%9C%E7%B4%A2%E3%81%99%E3%82%8B-cadb9c4b-459d-dfcb-2964-14aac1d7d964)で`Docker Desktop for Windows`を検索して実行してください。

![2022-12-11_102434.png](./images/2022-12-11_102434.png)

`Docker Desktop for Windows`の起動を確認できたら`Windows`のコマンドプロンプトを起動し、次のコマンドを入力してください。  
なお、ファイルパスの`\`バックスラッシュは![2022-12-11_102434.png](./images/yen.png)（円マーク）を意味しています。

```cmd
cd %USERPROFILE%\Documents\ros2_lecture_humble-202401
docker-compose pull
```

以下の画面のように環境のダウンロードが始まります。

![2024-01-13_132149.png](./images/2024-01-13_132149.png)

終了したら次のコマンドを入力してください。

```cmd
docker images
```

次のように`REPOSITORY`に`ros2_humble_lxde`、`TAG`に`202401`のイメージがダウンロードされていれば成功です。  
`9612864073fc   24 minutes ago   3.77GB`の部分は多少異なる可能性があります。

```cmd
REPOSITORY                     TAG               IMAGE ID       CREATED        SIZE
kmiyawaki20/ros2_humble_lxde   202401            9612864073fc   24 minutes ago   3.77GB
```

## 起動

以下の画面のようにWindowsのタスクトレイに`Docker Desktop for Windows`のアイコンが表示されていることを確認し、次の手順に移ってください。

`Docker Desktop for Windows`が起動されていなければ、起動してから次の手順に移ってください。

![2022-12-11_102434.png](./images/2022-12-11_102434.png)

`Docker Desktop for Windows`の起動を確認できたら、次のコマンドを入力してください。

```cmd
cd %USERPROFILE%\Documents\ros2_lecture_humble-202401
docker-compose up
```

次のようなメッセージが出力されます。

このコマンドを起動したターミナルは作業終了までは閉じないでください。

```cmd
・・・省略・・・
Attaching to ros2_lecture_humble
ros2_lecture_humble  |
ros2_lecture_humble  | New Xtigervnc server 'b4a97ab7c7a0:1 (ubuntu)' on port 5901 for display :1.
ros2_lecture_humble  | Use xtigervncviewer -SecurityTypes None,TLSNone b4a97ab7c7a0:1 to connect to the VNC server.
ros2_lecture_humble  |
ros2_lecture_humble  | WebSocket server settings:
ros2_lecture_humble  |   - Listen on :80
ros2_lecture_humble  |   - Web server. Web root: /usr/share/novnc
ros2_lecture_humble  |   - No SSL/TLS support (no cert file)
ros2_lecture_humble  |   - Backgrounding (daemon)
ros2_lecture_humble  | **********************************************
ros2_lecture_humble  | * Open 'http://127.0.0.1:6080/vnc.html'      *
ros2_lecture_humble  | * Or access '127.0.0.0:5901' via VNC viewer. *
ros2_lecture_humble  | **********************************************
```

任意のWEBブラウザで[http://127.0.0.1:6080/vnc.html](http://127.0.0.1:6080/vnc.html)に接続してください。  
次のような画面が出ますので、`Connect`を押してください。パスワードは不要です。

![2023-09-10_095247.png](./images/2023-09-10_095247.png)

次のように`Linux`の`GUI`が表示されれば成功です。

![2022-12-22_104159.png](./images/2022-12-22_104159.png)

もしも、`Linux`のデスクトップ画面が大きすぎてブラウザに入りきらない場合は`docker-compose.yml`をメモ帳で次のように変更してください。

```yaml
・・・
    environment:
      - USER=ubuntu
      - RESOLUTION=1024x600 # RESOLUTION=1280x800の数値を小さくする
      - TZ=Asia/Tokyo
    tty: true
・・・
```

編集後、以下の[終了](#終了)で一旦終了してから再度[起動](#起動)をしてみてください。

## 終了

WEBブラウザを閉じ、起動コマンドを実行したコマンドプロンプトで`Ctrl`キーと`C`キーを同時に押してください。

もし、終了しなければ２，３回`Ctrl`キーと`C`キーを同時に押してください。それでも終了しない場合は起動コマンドを実行したコマンドプロンプトを閉じてください。

## ROS2の動作確認

練習のために、できれば一度PCを再起動し、上記の「[起動](#起動)」をもう一度実施してみてから次の手順に移ってください。

### コマンドターミナルの起動とTurtlesim

WEBブラウザで表示されている`Linux`の`GUI`（以降単に`Linux`と表記します）のコマンドターミナルを起動してください。  
画面左下のアイコンをクリックし、`System Tools`->`LXTerminal`をクリックします。

![2024-01-12_115725.png](./images/2024-01-12_115725.png)

次のコマンドを実行してください。

```shell
ros2 run turtlesim turtlesim_node
```

なお、WEBブラウザ上での`Linux`に対し`Windows`側でコピーしたテキストを単純な方法ではペーストすることはできません。`Linux`->`Windows`も不可能です。  
`Linux`->`Linux`へのコピー＆ペーストは可能です。

`Windows`->`Linux`へのコピー＆ペーストをする場合は画面左のクリップボードアイコンをクリックし、そこにテキストをペーストしてください。  
その後、`Linux`側で右クリックからペーストを行ってください。

![2023-09-10_092351.png](./images/2023-09-10_092351.png)

`Linux`->`Windows`の場合は`Linux`側でコピーしたテキストが自動的にクリップボードに表示されますので、それを利用してください。

`ros2 run turtlesim turtlesim_node`を実行すると次の画面のように青いウィンドウと亀のキャラクタが表示されます。キャラクタは表示のたびに変わります。

![2022-12-11_110833.png](./images/2022-12-11_110833.png)

確認が終わったら`ros2 run turtlesim turtlesim_node`を起動したターミナルで`Ctrl+C`を押して終了させてください。

### talkerとlistener

次のコマンドを実行してください。

```shell
ros2 run py_pubsub talker
# 以下実行結果
[INFO] [1694675085.881202990] [minimal_publisher]: Publishing: "Hello World: 0"
[INFO] [1694675086.374024920] [minimal_publisher]: Publishing: "Hello World: 1"
[INFO] [1694675086.873766590] [minimal_publisher]: Publishing: "Hello World: 2"
・・・
```

画面左下のアイコンをクリックし、`System Tools`->`LXTerminal`をクリックして、新しいコマンドターミナルを起動し、次のコマンドを実行してください。

```shell
ros2 run py_pubsub listener
# 以下実行結果
[INFO] [1694675132.021339083] [minimal_subscriber]: I heard: "Hello World: 17"
[INFO] [1694675132.513064855] [minimal_subscriber]: I heard: "Hello World: 18"
[INFO] [1694675133.013289928] [minimal_subscriber]: I heard: "Hello World: 19"
[INFO] [1694675133.512461656] [minimal_subscriber]: I heard: "Hello World: 20"
[INFO] [1694675134.013059357] [minimal_subscriber]: I heard: "Hello World: 21"
・・・
```

もう一つコマンドターミナルを起動し次のコマンドを実行してください。

```shell
rqt_graph
```

次のような画面が出たら成功です。すべてのターミナルを`Ctrl+C`で終了させてください。

もしも、`rqt_graph`コマンドによって表示されたウィンドウでグラフ（直線で結ばれた二つの楕円）が表示されない場合は、図中赤丸内のウィンドウのリロードボタンを数回クリックしてみてください。

![2023-10-19_082759.png](./images/2023-10-19_082759.png)

### ロボットシミュレータ

次のコマンドを入力してください。

```shell
ros2 launch oit_minibot_light_01_ros2 stage_teleop.launch.py teleop:=mouse
```

シミュレータが起動しますので、「`Mouse Teleop`」と書かれたウィンドウ上でマウスをドラッグしてみてください。ロボットが移動します。

![2024-01-12-12-51-51.gif](./images/2024-01-12-12-51-51.gif)

シミュレータは起動コマンドを入力した`LXTerminal`で`Ctrl+C`を押して終了させてください。

## VSCodeとの連携

`Windows`のコマンドプロンプト（`Linux`ではありません）を起動し、次のコマンドを入力してください。`ros2_lecture_humble-202401`フォルダが`VSCode`で開かれるはずです。

```cmd
cd %USERPROFILE%\Documents\ros2_lecture_humble-202401
code . # 「code」に続けて半角スペースとドットを入力し、実行してください。
```

補足説明：`VSCode`は通常、前回起動時に開いていたフォルダを記憶しており、普通に再起動するとそのフォルダを開こうとします。  
例えば`WSL`や他の`Docker`コンテナに前回接続していた場合、その環境に自動的に接続しようとするため却って混乱を招く場合がありますので、ここではフォルダを指定して`VSCode`を起動します。

`VSCode`の扱いに慣れている場合は、任意の方法で`VSCode`を起動してかまいません。以下の拡張機能は`Windows`側にインストールするようにしてください。

`VSCode`の拡張機能から`Docker`、`Dev Containers`、`Remote Development`を選択してインストールしてください。

![2022-12-17_170506.png](./images/2022-12-17_170506.png)

![2022-12-22_150325.png](./images/2022-12-22_150325.png)

![2022-12-22_151530.png](./images/2022-12-22_151530.png)

インストール後は`VSCode`を一旦再起動してください。

[起動](#起動)の項目で実施した`docker-compose up`が正常に実行され、WEBブラウザで[http://127.0.0.1:6080/vnc.html](http://127.0.0.1:6080/vnc.html)に接続可能なことを確認してから、次の手順に移ってください。

`VSCode`のリモートエクスプローラをクリックして`開発コンテナ`をクリックしてください。  

![2023-09-10_093409.png](./images/2023-09-10_093409.png)

`ros2_lectures_humble`があるはずですので、それをクリックしてください。  
さらに、`現在のウィンドウでアタッチする`をクリックしてください。

![2024-01-12_120359.png](./images/2024-01-12_120359.png)

次のようなダイアログが出た場合は`Got It`をクリックしてください。

![2022-12-17_171023.png](./images/2022-12-17_171023.png)

`VSCode`が`Linux`に接続します。以下のような表示が出ることがありますが、問題はありません。「今後表示しない」や「X（バツボタン）」をクリックして次に進んでください。

![2022-12-22_113707.png](./images/2022-12-22_113707.png)

![2022-12-22_093913.png](./images/2022-12-22_093913.png)

左下の方に`Container kmiyawaki20/ros2_humble...`という表示が出ていることを確認してください。  
左側のフォルダアイコンをクリックし「フォルダを開く」を実行して、`/home/ubuntu/ros2_ws/src`を選択してください。

![2024-01-12_121035.png](./images/2024-01-12_121035.png)

`/home/ubuntu/ros2_ws/src/py_pubsub/py_pubsub/publisher_member_function.py`をダブルクリックし、エディタで表示できることを確認してください。

![2024-01-12_121433.png](./images/2024-01-12_121433.png)

`Ctrl+Shift+@`を同時に押してください。`VSCode`上でコマンドターミナルが開きます。  
`Ctrl+Shift+@`の同時押しを繰り返せばターミナルは複数個開くことができます。  
不要なターミナルは`exit`コマンド、もしくはゴミ箱のボタンを押すことで閉じることができます。

練習のために、`ros2 run py_pubsub talker`、`ros2 run py_pubsub listener`を`VSCode`から実行してみてください。

なお、`rqt_graph`を含むGUIのソフトを`VSCode`から実行する場合は次のようにしてください。

```shell
export DISPLAY=:1 # 新しいターミナルを開くごとにこのコマンドの実行が必要です。
rqt_graph
```

[http://127.0.0.1:6080/vnc.html](http://127.0.0.1:6080/vnc.html)に接続したWEBブラウザ上の`Linux`に`rqt_graph`の画面が表示されるはずです。  
シミュレータもこの方法で`VSCode`から起動できますので、一度試してみてください。

基本的な開発の流れとしては次のようになります。

1. WEBブラウザ上の`Linux`でシミュレータを起動する。
2. `VSCode`を`Linux`に接続し、`Python`コード編集を行う。
3. `VSCode`のターミナルから`Python`コードを実行する。

## VNCクライアントの利用（オプション）

VNCクライアントソフトを入れることで少し作業がしやすくなります。

1. コピー＆ペーストがダイレクトにできるようになる。
2. `Linux`側ターミナルで`Shift+Ctrl+T`によりタブを増やすことができる。
   - １つのターミナルウィンドウに複数のコマンド実行用のタブを作成できるのでデスクトップ画面がすっきりする。

VNCクライアントソフトには様々なものがありますが、ここでは[Real VNC](https://www.realvnc.com/en/connect/download/viewer/)を使います。リンクからダウンロードし、インストールしてください。

インストール後、`VNC Viewer`を実行し、接続先に`127.0.0.1:5901`を入力してエンターを押してください。

`Unencrypted Connection`の警告が出ますが`Don't warn me`にチェックを入れて`Continue`をクリックしてください。

![2022-12-22_160731.png](./images/2022-12-22_160731.png)

WEBブラウザのときと同じように`Linux`のデスクトップにアクセスできます。  
ターミナルソフトを起動して`Shift+Ctrl+T`を押すと、ターミナル内でタブを作成することができます。
ターミナルに`Windows`側からコピーしたコマンドを右クリックでペーストできます。

![2022-12-22_161539.png](./images/2022-12-22_161539.png)

`VNC Viewer`で接続した`Linux`デスクトップ表示の色数が少ない場合は、一旦`Linux`を「X」（バツボタン）で閉じ、`VNC Viewer`の下記画面で`127.0.0.1:5901`の接続先アイコンを右クリックし、`Properties`の画面を開いてください。

![2022-12-22_161354.png](./images/2022-12-22_161354.png)

`Options`の`Picture quality`を`HIGH`にして`OK`を押してください。  

![2022-12-22_161431.png](./images/2022-12-22_161431.png)

設定後、`127.0.0.1:5901`の接続先アイコンをダブルクリックして再接続してください。
