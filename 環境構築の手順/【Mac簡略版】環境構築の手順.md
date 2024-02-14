# 【Mac 簡略版】環境構築の手順

やっていることの意味は分からなくていいから、具体的にすべきことだけ知りたい場合はこの手順を参照する。  
意味が知りたいときや、やり方が分からないとき、エラーが起こったときはリンク先の元のマニュアルを見ると詳しい説明や対処法が書かれている。

## 1. VSCode をインストールする

元マニュアル: [1.VSCode をインストールする](./1.VSCodeをインストールする.md)

1. [参考サイト](https://qiita.com/watamura/items/51c70fbb848e5f956fd6)の「1. VSCode のサイトにアクセス」~「6. アプリケーションフォルダから VSCode を開いてください」までを参考にインストールする。
2. インストールした VSCode を開き、  
   [Command+Shift+P] -> "shell command"と入力 -> [Shell Command: Install 'code' command in PATH]を選択。

## 2. (Windows のみ) WSL2 をインストールする

Mac ではこの手順はしなくてよい。

## 3. Git をインストールする

元マニュアル: [3.(Mac のみ)Git をインストールする](<./3.(Macのみ)Gitをインストールする.md>)

[参考サイト](https://prog-8.com/docs/git-env)を参考にインストールする。  
なお、現時点では「2. Git のインストール」まで出来ていればよく、ターミナル上で`git --version`を入力して [Enter] を押したときにバージョンが表示されれ(エラーが起こらなけれ)ばよい。

## 4. Oh My Zsh をインストールする

元マニュアル: [4.Oh My Zsh をインストールする](<./4.Oh My Zshをインストールする.md>)

1. [公式マニュアル](https://support.apple.com/ja-jp/guide/terminal/apd5265185d-f365-44cb-8b09-71a064a42125/mac)を参考にしてターミナルを開く。

> Mac で、以下のいずれかの操作を行います:  
> Dock で Launchpad のアイコン <img src="https://help.apple.com/assets/63D8162D4F5E9E311D0CFA28/63D816334F5E9E311D0CFA30/ja_JP/a1f94c9ca0de21571b88a8bf9aef36b8.png" alt="" height="30" width="30" originalimagename="SharedGlobalArt/AppIconTopic_Launchpad.png"> をクリックして、検索フィールドに「ターミナル」と入力してから、「ターミナル」をクリックします。  
> Finder <img src="https://help.apple.com/assets/63D8162D4F5E9E311D0CFA28/63D816334F5E9E311D0CFA30/ja_JP/058e4af8e726290f491044219d2eee73.png" alt="" height="30" width="30" originalimagename="SharedGlobalArt/AppIconTopic_Finder.png"> で、「/アプリケーション/ユーティリティ」フォルダを開いてから、「ターミナル」をダブルクリックします。

2. ターミナルに下記をコピペして [Enter]を押す。  
   デフォルトシェルを Zsh にするか聞かれた場合は`y`と入力して [Enter].

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

3. 以下を実行。

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

4. 下記を実行して VSCode で`~/.zshrc`というファイルを開く。

```shell
code ~/.zshrc
```

5. `ZSH_THEME="robbyrussell"`という行を探し、以下の 2 行に置き換える。

```shell
ZSH_THEME="simplerich"
source ~/simplerich-zsh-theme/zsh-git-prompt/zshrc.sh
```

6.  `plugins=(git)`という行を探し、以下に置き換え、 [Command+S] で保存。  
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

7. 下記をターミナルで実行する。画面の指示に従って[Enter]を押す。

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

場合によっては最後に、`Run these two commands in your terminal to add Homebrew to your PATH:`と言ってくるので、その下の 2 行を実行する。  
(ない場合は気にしなくてよい。)

```shell
# これは例。実際に実行するのはインストーラが示したコマンドにすること。
(echo; echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"') >> /home/testuser/.zprofile
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```

8. 下記をターミナルで実行する。

```shell
brew install coreutils
source ~/.zshrc
```

## 5. Python の環境構築をする

元マニュアル: [5.Python の環境構築をする](./5.Pythonの環境構築をする.md)

以下をターミナルで実行する。

```shell
brew install xz
brew install pyenv
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
source ~/.zshrc
pyenv install 3.11.1
pyenv global 3.11.1
echo "export VIRTUAL_ENV_DISABLE_PROMPT=1" >> ~/.zshrc
source ~/.zshrc
```

## 6. エイリアスを設定する

元マニュアル: [6.エイリアスを設定する](./6.エイリアスを設定する.md)

1. ターミナルで下記を実行して、VSCode で`~/.gitconfig`というファイルを開く。

```shell
code ~/.gitconfig
```

2. 下記を書き込んで、[Command+S]で保存する。  
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

4. 下記を一番最後に追記して、[Command+S] で保存する。

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
```

5. ターミナルで下記を実行する。

```shell
source ~/.zshrc
```

## 7. VSCode の設定をする

元マニュアル: [7.VSCode の設定をする](./7.VSCodeの設定をする.md)

1. VSCode を開く。
2. [公式サイト](https://learn.microsoft.com/ja-jp/power-pages/configure/vs-code-extension#install-visual-studio-code-extension)の「Visual Studio Code 拡張機能のインストール」の項を参考に、以下の拡張機能を検索窓で検索して全てインストールする。

※リンターが何かについては[Python による開発のあれこれ](../開発の手順/Pythonによる開発のあれこれ.md#1-リンターについて)に記載がある。

- [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) (コードフォーマッタ)
- [Black Formatter](https://marketplace.visualstudio.com/items?itemName=ms-python.black-formatter) (Python のコードフォーマッタ)
- [isort](https://marketplace.visualstudio.com/items?itemName=ms-python.isort) (Python のコードフォーマッタ)
- [Flake8](https://marketplace.visualstudio.com/items?itemName=ms-python.flake8) (Python のリンター)
- [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker) (英語のタイポを教えてくれる)
- [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) (Git 情報を見やすくしてくれる)
- [Git History](https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory) (Git のログを見やすくしてくれる)
- [Code Runner](https://marketplace.visualstudio.com/items?itemName=formulahendry.code-runner) (コードの(コンパイルと)実行をショートカットキーで行えるようにする)

3. [Command+Shift+P]->[Preferences: Open User Settings (JSON)]を選択。
4. 開いたファイルに以下のように追記して保存する。ただし、元々記入してあるものとの間に`,`を入れるのを忘れないこと。

```json
{
    ...<元々記入してあるやつ。この後に,を入れることを忘れないこと>,
    "black-formatter.args": ["--line-length", "119"],
    "flake8.args": ["--extend-ignore", "E203", "--max-line-length", "119"],
    "isort.args": ["--profile", "black", "--line-length", "119"]
}
```

5. VSCode 上で [Command+,] -> "Format On Save"と検索してチェックを入れる。
6. 続けて"Default Formatter"と検索して Prettier を選ぶ。
7. [Command+Shift+P] -> [Reload Window]を選択。

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
brew install gh
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
