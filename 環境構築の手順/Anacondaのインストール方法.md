# Anaconda のインストール方法

この環境構築の手順に従うと venv というアプリで Python の仮想環境の構築ができるようになる。  
venv は Python 標準が提供する仮想環境であり、基本これを使うことが推奨されるが、比較的新しく、古くから使われていた Anaconda という非標準のアプリを使っているプロジェクトも多い。  
このマニュアルはそのようなプロジェクトに対応するために、Anaconda のインストール方法について説明したマニュアルである。

## 必要なもの

- (Windows のみ)[2.WSL2 をインストールする](<./2.(Windowsのみ)WSL2をインストールする.md>)の通り、WSL が入っており、デフォルトシェルが Zsh になっていること。

## 手順

### 1. ターミナルを開く

#### Windows

[参考サイト](https://kb.seeck.jp/archives/20593)

> [スタート] -> [すべてのアプリ] –> [ターミナル]をクリック

#### Mac

[公式マニュアルより](https://support.apple.com/ja-jp/guide/terminal/apd5265185d-f365-44cb-8b09-71a064a42125/mac)

> Mac で、以下のいずれかの操作を行います:  
> Dock で Launchpad のアイコン <img src="https://help.apple.com/assets/63D8162D4F5E9E311D0CFA28/63D816334F5E9E311D0CFA30/ja_JP/a1f94c9ca0de21571b88a8bf9aef36b8.png" alt="" height="30" width="30" originalimagename="SharedGlobalArt/AppIconTopic_Launchpad.png"> をクリックして、検索フィールドに「ターミナル」と入力してから、「ターミナル」をクリックします。  
> Finder <img src="https://help.apple.com/assets/63D8162D4F5E9E311D0CFA28/63D816334F5E9E311D0CFA30/ja_JP/058e4af8e726290f491044219d2eee73.png" alt="" height="30" width="30" originalimagename="SharedGlobalArt/AppIconTopic_Finder.png"> で、「/アプリケーション/ユーティリティ」フォルダを開いてから、「ターミナル」をダブルクリックします。

### 2. (Windows のみ) WSL に切り替える

下記の「正しい方」がターミナルに表示されている(PS が表示されていない)ことを確認する。  
間違っている方が表示されている場合は、Linux ではなく Windows のシェルになっている。  
`wsl`とターミナルに打ち込んで [Enter] をすると Linux のシェルに切り替わる。

```shell
# 正しい方
[時刻] ユーザー名 ~
$

# 間違っている方
PS C:\Users\Windowsのユーザー名>

# 間違っていた場合下記を実行
wsl
```

### 3. Anaconda をインストールする

#### Windows

参考:[このサイト](https://www.salesanalytics.co.jp/datascience/datascience141/#LinuxAnaconda)の「Linux 版の Anaconda をインストール」の見出しから

> 以下を実行する。  
> URL は最新版ではない可能性があるので、エラーになるときは参考サイトを見て最新版を取得する。
>
> 途中`Please, press ENTER to continue`と言われたら [Enter] を押し、下矢印キーでライセンス条項を一番下まで降り、`yes`と入力して [Enter] を押す。  
> 次にインストール場所を聞かれるが、デフォルトのままでよいのでそのまま [Enter] を押す。  
> 最後に、Anaconda3 の初期化をするか聞かれるので、`yes`と答える。
>
> ```shell
> cd ~
> wget https://repo.anaconda.com/archive/Anaconda3-2022.10-Linux-x86_64.sh
> bash Anaconda3-2022.10-Linux-x86_64.sh
> ```
>
> 全て終わったら、 以下を実行する。  
> なお、参考サイトでは`source .bashrc`を実行するとなっているが、今回は Bash から Zsh に変えているのでこちらが正しい。
>
> ```shell
> source ~/.zshrc
> ```

#### Mac

[参考サイト](https://www.python.jp/install/anaconda/macos/install.html)の「パッケージのダウンロード」と「パッケージのインストール」を参考にインストールする。  
その後、ターミナルで下記を実行する。

```shell
/opt/anaconda3/bin/conda init zsh
```

### 4. Anaconda の設定をする

#### Windows/Mac 共通

下記を実行する。  
具体的には、Anaconda の仮想環境が自動でアクティベートされるのを停止しているのと、ターミナルの先頭の(base)の表示を消している。

```shell
conda config --set auto_activate_base false
conda config --set changeps1 false
source ~/.zshrc
```
