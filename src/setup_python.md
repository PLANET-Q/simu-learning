# pythonをインストールしよう

まずはパソコンでpythonが使えるように環境を整えていきます。  
pythonのインストール方法はたくさんあってどの方法でも動くんですが、
今回は **Anaconda** というpythonの仮想環境構築ツールというのを使ってインストールしていきます。  

## Anacondaとは

Anacondaって何だよ！っていう人向けに説明しますがそんなに重要ではないので飛ばしてインストールに進んでもいいよ。  
通常は、1つのPCで利用されるpythonは1つだけなのですが、用途に応じて別のバージョンのpythonを使いたかったり、あるパッケージ(ArduinoとかC言語でいうライブラリ)を使いたいけど、今使ってる別のライブラリと干渉して使えない・・と言った問題があります。  
Anacondaは、PCのなかに **仮装環境** というのを構築して、1つの仮装環境の上で1つのpythonを独立して動作させます。pythonのバージョンとかパッケージとかは他の環境とは独立するので、管理がとても楽になります。

## anaconda のダウンロード&インストール

### ダウンロード

以下のサイトからAnacondaをダウンロードします。下の方にスクロールしていくとAnaconda xxx For Windows(/Mac) Installerってのが出てくるので、
**Python 3.x version** の方のDownload をクリックしてダウンロードします。  

Anaconda　ダウンロードページ:  
https://www.anaconda.com/download/

![python3.x versionのAnacondaをダウンロードしよう](https://i.imgur.com/DeBavIz.png)

>2018年11月現在、Pythonは2系と3系のバージョンが混在しています。最近はほとんど3系で書かれるようですが古いコードなどではまだ2系が使用されるためです。2系と3系では言語仕様に結構違いがあるので注意しましょう。Anacondaの仮装環境を分ければ、両方のバージョンを使い分けることができますが、とりあえず最初に使用するPythonのバージョンは3系としたいので3.* versionをダウンロードましょう。

### インストール

ダウンロードした exeファイルを実行します。
いろいろ聞かれたりしますが基本的にはそのまま Nextで大丈夫ですが、
 **下の画像のAdd Anaconda to my PATH environment variableの部分のチェックを付けてください（デフォルトではチェックがついていません)**

![Anaconda to my pathにチェック](https://i.imgur.com/8vENWk4.png)

インストールが終わったら、最後に　VisualStudio Codeというソフトをインストールすることを勧められますが、しなくてもAnacondaを動かすことはできるのでどちらでも大丈夫です。ただVisualStudio Codeは非常に優秀なエディタなのでインストールしておくことをおすすめします。VSCodeの使い方はググって調べてください。

![Skipで大丈夫](https://i.imgur.com/Faw2QvU.png)

Skipした次の画面が最後の画面です。チェックは２つとも外してFinishを押してインストール完了です。

![チェックは2つとも外してFinish](https://i.imgur.com/S6xGqPZ.png)

## 次のステップ

- [GithubDesktopをインストールする](setup_githubdesktop.md)
- [TrajecSimuをインストールする](setup_simu.md)
