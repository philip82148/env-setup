# 【Windows 簡略版】環境構築の手順

やっていることの意味は分からなくていいから、具体的にすべきことだけ知りたい場合はこの手順を参照する。  
意味が知りたいときや、やり方が分からないとき、エラーが起こったときはリンク先の元のマニュアルを見ると詳しい説明や対処法が書かれている。

## 1. VSCode をインストールする

元マニュアル: [1.VSCode をインストールする](./1.VSCodeをインストールする.md)

[参考サイト](https://www.javadrive.jp/vscode/install/index1.html)の「Visual Studio Code をダウンロードする」と「Visual Studio Code をインストールする」の項を参考にインストールする。

## 2. WSL2 をインストールする

元マニュアル: [2.(Windows のみ)WSL2 をインストールする](<./2.(Windowsのみ)WSL2をインストールする.md>)

1. ターミナルを開く。  
   [スタート] -> [すべてのアプリ] –> [ターミナル]をクリック

2. 下記をターミナルにコピペして [Enter] を押し、処理が終わったら、パソコンを再起動する。  
   その後開いたターミナルで Linux のアカウントを作る。  
   ユーザー名は Windows のものと同じでもよいが、パスワードは頻繁に使うので覚えやすいものが良い。

```shell
wsl --install -d Ubuntu-22.04
```

3. ターミナルが下記の「正しい方」のように表示されている(PS が表示されていない)のを確認する。  
   **なお、今後ターミナルを開いたときは必ず「正しい方」になっていることを確認してから入力を行う。**  
   「間違っている方」のようになっていた場合は`wsl`コマンドを実行すると「正しい方」に切り替わる。

```shell
# 正しい方
アカウント名@パソコン名:~$
# もしくは
[時刻] アカウント名 ~
$

# 間違っている方
PS C:\Users\Windowsのユーザー名>

# 間違っていた場合下記を実行
wsl
```

4. その後、下記をターミナルにコピペして [Enter]。  
   パスワードを求められるので Linux のパスワードを入力する。  
   最後に設定画面が表示されたら、`2`と入力する。

```shell
sudo apt -y update
sudo apt -y upgrade
sudo apt -y install zsh
chsh -s $(which zsh)
zsh
```

## 3. (Mac のみ) Git をインストールする

Windows ではこの手順はしなくてよい。

## 4. Oh My Zsh をインストールする

元マニュアル: [4.Oh My Zsh をインストールする](<./4.Oh My Zshをインストールする.md>)

1. 下記をターミナルにコピペして [Enter]。  
   デフォルトシェルを Zsh にするか聞かれた場合は`y`と入力して [Enter].

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

2. 以下を実行。

```shell
cd ~/.oh-my-zsh/plugins/
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
git clone https://github.com/zsh-users/zsh-completions.git
git clone https://github.com/zsh-users/zsh-autosuggestions.git
git clone https://github.com/zsh-users/zsh-history-substring-search.git
cd ~
git clone --recursive https://github.com/philip82148/simplerich-zsh-theme
cp ./simplerich-zsh-theme/simplerich.zsh-theme ~/.oh-my-zsh/themes/
```

3. 下記を実行して VSCode で`~/.zshrc`というファイルを開く。

```shell
code ~/.zshrc
```

4. `ZSH_THEME="robbyrussell"`という行を探し、以下の 2 行に置き換える。

```shell
ZSH_THEME="simplerich"
source ~/simplerich-zsh-theme/zsh-git-prompt/zshrc.sh
```

5.  `plugins=(git)`という行を探し、以下に置き換え、 [Ctrl+S] で保存。  
    その後、VSCode を閉じる。

```shell
plugins=(
   git
   zsh-syntax-highlighting
   zsh-completions
   zsh-autosuggestions
   zsh-history-substring-search
)
```

6. 下記をターミナルで実行する。

```shell
sudo apt -y install bc
source ~/.zshrc
```

## 5. Python の環境構築をする

元マニュアル: [5.Python の環境構築をする](./5.Pythonの環境構築をする.md)

以下をターミナルで実行する。

```shell
sudo apt -y upgrade python3
sudo apt -y install python3-pip
sudo apt -y install python3-venv
echo "export VIRTUAL_ENV_DISABLE_PROMPT=1" >> ~/.zshrc
echo "alias python=python3" >> ~/.zshrc
source ~/.zshrc
```

## 6. エイリアスを設定する

元マニュアル: [6.エイリアスを設定する](./6.エイリアスを設定する.md)

1. ターミナルで下記を実行して、VSCode で`~/.gitconfig`というファイルを開く。

```shell
code ~/.gitconfig
```

2. 下記を書き込んで、[Ctrl+S]で保存する。  
   なお、すでに書き込みがある場合は一番下に追記する。

```shell
[alias]
    a = add
    c = commit
    cm = commit -m
    ad = commit --amend
    adm = commit --amend -m
    b = branch
    co = checkout
    cob = checkout -b
    l = log
    lp = log --graph --pretty=format:'%C(yellow)%h%C(cyan)%d%Creset %s %C(green)- %an, %cr%Creset'
    lpn = log --graph --pretty=format:'%C(yellow)%h%C(cyan)%d%Creset %s %C(green)- %an, %cr%Creset' --name-status
    sl = stash list
    su = stash -u
    svu = stash save -u
    sp = stash pop
```

3. ターミナルで下記を実行して、VSCode で`~/.zshrc`というファイルを開く(先ほど[4. Oh My Zsh をインストールする](#4-oh-my-zsh-をインストールする)で開いたものと同じファイルである)。

```shell
code ~/.zshrc
```

4. 下記を一番最後に追記して、[Ctrl+S] で保存する。

```shell
alias g=git
alias ga='git a'
alias ga.='git a .'
alias gc='git c'
alias gcm='git cm'
alias gad='git ad'
alias gadm='git adm'
alias gb='git b'
alias gco='git co'
alias gcob='git cob'
alias gl='git l'
alias glp='git lp'
alias glpn='git lpn'
alias gsl='git sl'
alias gsu='git su'
alias gsvu='git svu'
alias gsp='git sp'
alias greset='git reset'
alias gpull='git pull'
alias gpush='git push'
alias gfetch='git fetch'
alias gmerge='git merge'
```

5. ターミナルで下記を実行する。

```shell
source ~/.zshrc
```

## 7. VSCode の設定をする

元マニュアル: [7.VSCode の設定をする](./7.VSCodeの設定をする.md)

1. 以下をターミナル上で実行し VSCode を**WSL 上で**開く。  
   なお、[6. エイリアスを設定する](#6-エイリアスを設定する)で開いた VSCode がそのまま残っていれば、下記を実行せずともそれを使ってよい。

```shell
code ~
```

#### 重要: WSL 上で setup した場合も、`WSL: Ubuntu-22.04`と表示されない場合がある。

その場合、VSCode に、WSL の拡張機能がインストールされているかチェックする。入っていない場合、追加でインストールする。Code の再起動を促された場合、再起動する。

2. [公式サイト](https://learn.microsoft.com/ja-jp/power-pages/configure/vs-code-extension#install-visual-studio-code-extension)の「Visual Studio Code 拡張機能のインストール」の項を参考に、以下の拡張機能を検索窓で検索して全てインストールする。

- [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) (コードフォーマッタ)
- [Black Formatter](https://marketplace.visualstudio.com/items?itemName=ms-python.black-formatter) (Python のコードフォーマッタ)
- [isort](https://marketplace.visualstudio.com/items?itemName=ms-python.isort) (Python のコードフォーマッタ)
- [Flake8](https://marketplace.visualstudio.com/items?itemName=ms-python.flake8) (Python のリンター)
- [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker) (英語のタイポを教えてくれる)
- [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) (Git 情報を見やすくしてくれる)
- [Git History](https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory) (Git のログを見やすくしてくれる)
- [Code Runner](https://marketplace.visualstudio.com/items?itemName=formulahendry.code-runner) (コードの(コンパイルと)実行をショートカットキーで行えるようにする)

※リンターが何かについては[Python による開発のあれこれ](../開発の手順/Pythonによる開発のあれこれ.md#1-リンターについて)に記載がある。

3. VSCode 上で[Ctrl+,] -> "Format On Save"と検索してチェックを入れる。
4. 続けて"Default Formatter"と検索して Prettier を選ぶ。
5. [Ctrl+Shift+P]->[Reload Window]を選択。
6. [Ctrl+Shift+P]->[Preferences: Open User Settings (JSON)]を選択。
7. 開いたファイルに以下のように追記して保存する。ただし、元々記入してあるものとの間に`,`を入れるのを忘れないこと。

```json
{
    ...<元々記入してあるやつ。この後に,を入れることを忘れないこと>,
    "black-formatter.args": ["--line-length", "119"],
    "flake8.args": ["--extend-ignore", "E203", "--max-line-length", "119"],
    "isort.args": ["--profile", "black", "--line-length", "119"]
}
```

8. [Ctrl+Shift+P]->[Reload Window]を選択。

#### WSL 上で Code の拡張設定を追加しなかった場合

この場合、後ほど Code の拡張機能で WSL 対応機能を追加することになるが、code runner の設定を変更しなければならない。

read-only なので setting json から変更できないことにも注意
"Code Runner"の Extension settings から

```shell
Code-runner: Run in terminal
```

のチェックボックスにチェックを入れる。

すると、Preferences: Open Default Settings (JSON)が

```json
{
  "code-runner.runInTerminal": true
}
```

となる。これで code-runner の実行がターミナル上で[Ctrl+Alt+N]で可能になる。

## 8. Git の認証情報を設定する

元マニュアル: [8.Git の認証情報を設定する](./8.Gitの認証情報を設定する.md)

1. [参考サイト](https://yakiimosan.com/github-account-create/)を参考に[GitHub](https://github.co.jp/)のアカウントを作る。

2. 以下の 2 つをターミナルで実行する。日本語の部分は書き換えること。  
   **これらは外部に公開される。**それが嫌な場合は任意のメールと名前を設定してよい。  
   メールに関しては GitHub 側が用意したメールを使うこともできる。  
   詳しくは元マニュアルを参照。

```shell
git config --global user.email "GitHubアカウントのメールアドレス"
```

```shell
git config --global user.name "GitHubアカウントのID"
```

3. 以下をターミナルで実行する。

```shell
sudo apt -y install gh
gh auth login
```

4. いくつか質問されるので以下のように答える。

```console
What account do you want to log into?
 -> GitHub.com
What is your preferred protocol for Git operations?
 -> HTTPS
Authenticate Git with your GitHub credentials?
 -> Y
How would you like to authenticate GitHub CLI?
 -> Login with a web browser
First copy your one-time code: XXXX-XXXX
 -> XXXX-XXXXの部分をコピーする
Press Enter to open github.com in your browser...
```

5. [Enter] を押す。  
   ブラウザが自動で開かれるはずだが、開かなかったら直接 [https://github.com/login/device](https://github.com/login/device) を開く。  
   その後、コピーしたコードを張り付けて認証する。
