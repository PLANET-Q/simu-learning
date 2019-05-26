# TrajecSimuのセットアップ

ここではTrajecSimuインストールと、実行するための仮装環境構築について説明していきます。
python(anaconda)のインストールが終わっていない場合は先にインストールをしてください

## 必要なもの

- Anaconda
- Github Desktop([こちら](setup_githubdesktop.md)をみてインストールしてください)

## TrajecSimuのコードをダウンロード（Clone）

### TrajecSimuページからGithub Desktopを開く

TrajecSimuは **github** に公開されています。  
以下のサイト上の方の **Clone or download** をクリックし、**Open in Desktop**
をクリックしてGithubDesktopを開きましょう。  
[TrajecSimuのGithub](https://github.com/PLANET-Q/TrajecSimu)

**注意** ：この時点でGithub DesktopをPCにインストールしていない場合は何も起こりません。[こちら](setup_githubdesktop.md)をみてインストールしてください  

![Open in Desktop](https://i.imgur.com/9Me6BnM.png)

### TrajecSimuをローカルにクローンする

Open in Desktopを押すと GithubDesktop.exeを開きますか？と聞かれるので「はい」で開くと、Github Desktopが起動して以下のような画面が出てきます。  

- Repository URLの部分はいじらなくてOK
- Local pathにTrajecSimuをダウンロードするフォルダを選択

ダウンロード先フォルダを適切に設定したら Cloneを押してTrajecSimuをクローンします。

![TrajecSimuをクローン](https://i.imgur.com/zYQfq6s.png)

正常にクローンできたら、以下のような画面になっているはずです。
![Sccess to clone](https://i.imgur.com/7cNeaRX.png)

## Anacondaの環境を作ろう

シミュ用のAnaconda環境を作っていきます。

### 仮装環境作成

`conda create` コマンドを使用して仮装環境を新たに作成します。  

```
conda create -n simu python=3.7
```

`-n` の後にその仮装環境の名前を指定します。ここでは `simu` という名前で指定しています。  
`python=3.7` の部分は使用するPythonのバージョン。必要に応じて最新のものに変えても良いですが動作の保証はできません、とりあえず3.7にすれば動きます(2019/4現在)

### 仮装環境の切り替え

#### for Windors

`activate` コマンドによって仮装環境を切り替えます。

```
activate simu
```

#### for Mac/Linux

`conda activate` コマンドによって仮装環境を切り替えます。

```
conda activate simu
```

#### 切り替えた仮装環境を元に戻す場合

```
deactivate
```

### 必要なパッケージのインストール

TrajecSimuの[ダウンロードページ](https://github.com/PLANET-Q/TrajecSimu)の`必要なPythonパッケージ`に、TrajecSimuを動かすのに必要なPythonパッケージが記載されています。
それらを `pip install` コマンドを使ってインストールしていきます。

例えば `numpy` と `scipy` をインストールする場合以下のように実行します。

```
pip install numpy scipy
```

> pythonパッケージのインストールには `conda install` コマンドを利用することもできますが、ここでは `pip`の方で説明していきます。 `numpy-quaternion` パッケージのインストールがこっちの方が楽なので。

インストールが終わったら、TrajecSimuを実行するためのセットアップは終了です。
TrajecSimuに同梱しているサンプルコードを実行してうまく動くか試してみましょう。

## サンプルコードの実行

TrajecSimuのフォルダ内で以下のコマンドを実行すると、コマンドプロンプト上にいろいろ表示された後に、グラフが5つくらい出てくるはずです。

```
python driver_sample.py
```

## パラメータ設定ファイルについて

TrajecSimuを動かすために設定するパラメータファイルの説明を[こちら](about_parameters.md)にまとめているので参照してください

## 問い合わせ

何か質問があれば、yumail6[at]gmail.com にメールしてください。メール件名に `[TrajecSimuに関する質問]` を入れてくれるとスムーズです。  
質問や意見は、この文書の改善につながります。遠慮せず連絡してください。
