# PR(プルリクエスト)の方法

[参考サイト](https://book.st-hakky.com/docs/github-how-to-pull-request/)

## このマニュアルを理解するのに必要な前提知識

- [開発の手順](./開発の手順.md)に書かれた内容が理解できること。
- このマニュアルでは、コマンドは[【Windows 簡略版】環境構築の手順](../環境構築の手順/【Windows簡略版】環境構築の手順.md)か[【Mac 簡略版】環境構築の手順](../環境構築の手順/【Mac簡略版】環境構築の手順.md)の「6. エイリアスを設定する」に示す方法で登録したエイリアスを使って説明している。

## PR(プルリクエスト)とは

複数人で開発する際は、各々の改変がごちゃ混ぜになることを防ぐために、PR(プルリクエスト)を立てて、一人一人の変更を分けて`main`ブランチに反映させる。

大まかには`main`ブランチとは別にサブブランチを作って、リポジトリを二つのバージョンに分け、サブブランチにコミットを行って、最後にサブブランチの変更を`main`にマージするという流れである。

リポジトリの管理者だけが`main`ブランチをいじることができ、他のエンジニアが`main`ブランチを変更する際は、管理者に自分の作ったサブブランチを"プルしてもらう"という構造のため、プルリクエストという。

## 1. リモートのメインブランチの変更をローカルに反映する

リモートリポジトリの反対に、パソコン上のリポジトリをローカルリポジトリという。  
まずはリモートリポジトリの`main`ブランチの最新の内容を、ローカルリポジトリに反映させる。

1.`main`ブランチにいることを確認する。  
 ローカルリポジトリ内でターミナルを開いたとき、プロンプトの`[Git情報]`の部分に`[main]`と表示されていることを確認する。  
 表示されていない場合は`gco main`というコマンドを実行する。

```console
[時刻] ユーザー名 (Python仮想環境名) ワーキングディレクトリ [Git情報]
$
```

```shell
# [main]と表示されていない場合は下記を実行する
# 本来はgit checkout mainというコマンド
# gcoはg coでもよい
gco main
```

2. リモートの`main`ブランチの内容をローカルに反映する。  
   `gpull`は`g pull`でもよい。  
   下記をターミナルで実行する。

```shell
gpull
# There is no tracking information for the current branch.というエラーが出る場合は
# 下記を実行してorigin mainをデフォルト(上流ブランチ)に設定する
# gpull --set-upstream origin main
```

ここでエラーが起こった際はリモートに無い変更がローカルの`main`ブランチにあるということである。  
この場合は[コンフリクトが発生したときの対処法](#コンフリクトが発生したときの対処法)の[リモートの`main`ブランチとローカルの`main`ブランチの間で起きたコンフリクトの場合](#リモートのmainブランチとローカルのmainブランチの間で起きたコンフリクトの場合)を参照する。

## 2. ブランチを切る(生やす)

以下のコマンドを実行して、ブランチを切る。  
例えばブランチ名を`add-some-function`にすると、ローカルリポジトリは`main`ブランチと`add-some-function`ブランチの二つのバージョンができることになる。  
それぞれのブランチに別々にコミットすることができ、リポジトリが 2 バージョン出来ることになる。  
ブランチ名は、そのブランチや PR で作りたい機能や達成したいことが分かりやすい名前にする。

```shell
# 新しくブランチを作り、そのブランチに切り替える
# 本来はgit checkout -b ブランチ名というコマンド
# gcobはg cobでもよい
gcob ブランチ名
```

なお、ブランチに関して、他に以下のようなコマンドがある。

```shell
# すでにあるブランチに切り替える
# 本来はgit checkout ブランチ名というコマンド
gco ブランチ名

# ローカルにあるブランチの一覧を取得する
# 本来はgit branchというコマンド
# g bでもよい
gb
```

## 3. コードを編集してコミットする

やり方は[開発の手順](./開発の手順.md)と同じである。

## 4. PR(プルリクエスト)を出す

PR(プルリクエスト)とは、「自分のブランチを`main`ブランチにプル(反映)してください」というリクエストを送ることである。  
これにより、自分のブランチ(`add-some-function`)にためたコミットを一気に`main`ブランチに反映することができる。  
なお、普通はこの際に他の人のコードレビューが入り、本当にプルしても大丈夫な内容かどうかが確認される。

やり方は、[参考サイトの「4. 作業が完了したら push する」](https://book.st-hakky.com/docs/github-how-to-pull-request/#4-%E4%BD%9C%E6%A5%AD%E3%81%8C%E5%AE%8C%E4%BA%86%E3%81%97%E3%81%9F%E3%82%89push%E3%81%99%E3%82%8B)以降を参考にして進める。

ただし、コンフリクトが発生しているときは[コンフリクトが発生したときの対処法](#コンフリクトが発生したときの対処法)を参考にコンフリクトを解消する。

## コンフリクトが発生したときの対処法

コンフリクトとは、異なる変更履歴が引き起こすソースコードの競合のことで、自分のブランチで変更した部分と同じ部分に、別な変更が、リモートリポジトリや`main`ブランチにされた場合などに発生する。  
複数人で開発している場合は、自分のブランチで作業している間にどんどん`main`ブランチが変更されていくので、プルリクエストを出したときにコンフリクトが発生することはよくある。

以下ではコンフリクトの種類に応じて対処法を示す。

### サブブランチと`main`ブランチの間に起きたコンフリクトの場合

PR はサブブランチ(`main`でないブランチ)を`main`ブランチにマージするものなので、PR で指摘されるコンフリクトはこれである。  
または以下のコマンドでエラーが起きたとき、このコンフリクトが発生している(ただし`main`ブランチで下記を実行してエラーになった場合は[リモートの`main`ブランチとローカルの`main`ブランチの間で起きたコンフリクトの場合](#リモートのmainブランチとローカルのmainブランチの間で起きたコンフリクトの場合)を参照)。

```shell
gpull origin main
# エラー！
# (このサブブランチにとってorigin mainは上流ブランチではないので省略できない)
```

対処法は以下。

1. 自動マージをする。

```shell
# リモートのmainの内容を取得する
# g fetch origin mainでもよい
# (このサブブランチにとってorigin mainは上流ブランチではないので省略できない)
gfetch origin main

# ローカルに自動マージをする(新たなコミットが作られる)
# g merge origin mainでもよい
# (このサブブランチにとってorigin mainは上流ブランチではないので省略できない)
gmerge origin main
```

2. 上のコマンドで自動でマージできた場合は、WSL では nano、Mac では vim というコンソール上のエディタが起動し、コミットメッセージの編集を求められる。  
   が、デフォルトのままで問題ないので、そのままエディタを閉じればよい。  
   エディタの閉じ方は WSL では [Ctrl+X] 、Mac では`:`、`q` を続けて押す。

   しかし、自動マージできない場合もある。  
   その場合は VSCode の Merge Editor を使って自動マージできなかったファイルを編集し、最後に GUI からコミットを行ってマージする。

   [Merge Editor の使い方](https://futureys.tokyo/lets-resolve-conflict-on-g-by-vs-code-3-way-merge-editor/)

3. ローカルで行われた変更をリモートに反映する。
   以下のコマンドで、ローカルの内容をリモートに反映させることができる。  
   `gpush`は`g push`でもよい。  
   ここで、ブランチ名は`main`ではなく自分のブランチ名なので気を付けること。

```shell
# 初回のみ
gpush -u origin ブランチ名
# 2回目以降
gpush
```

### リモートブランチとローカルブランチの間で起きたコンフリクトの場合

一つのブランチを複数人で編集した際等に、以下のコマンドでエラーが起き、このコンフリクトが発生する(ただし`main`ブランチで下記を実行してエラーになった場合は[リモートの`main`ブランチとローカルの`main`ブランチの間で起きたコンフリクトの場合](#リモートのmainブランチとローカルのmainブランチの間で起きたコンフリクトの場合)を参照)。

```shell
# (origin ブランチ名はgpush -u origin ブランチ名か
# gpull --set-upstream origin ブランチ名を実行したことがあれば省略可能)
gpull origin ブランチ名
# エラー！
```

この場合のやり方は[サブブランチと`main`ブランチの間に起きたコンフリクトの場合](#サブブランチとmainブランチの間に起きたコンフリクトの場合)で`main`ブランチとして指定していたものを該当のブランチ名に置き換えるだけである。

1. 自動マージをする。

```shell
# リモートブランチの内容を取得する
# (origin ブランチ名はgpush -u origin ブランチ名か
# gpull --set-upstream origin ブランチ名を実行したことがあれば省略可能)
gfetch origin ブランチ名

# ローカルに自動マージをする(新たなコミットが作られる)
# (origin ブランチ名はgpush -u origin ブランチ名か
# gpull --set-upstream origin ブランチ名を実行したことがあれば省略可能)
gmerge origin ブランチ名
```

2. 以降の手順は[サブブランチと`main`ブランチの間に起きたコンフリクトの場合](#サブブランチとmainブランチの間に起きたコンフリクトの場合)と同じ。

### リモートの`main`ブランチとローカルの`main`ブランチの間で起きたコンフリクトの場合

この場合、ローカルの`main`ブランチが変更されているということであり、PR で管理されているはずなのに PR によらない変更が`main`ブランチにあるというのは危険なので、他の人に相談するのが一番良い。

まず、[開発の Tips](./開発のTips.md)に`[Git情報]`の意味が記されているので、ローカルの`main`ブランチにある変更がどのような変更かを確認する。  
その後、変更の種類に応じて以下のような対処法がある。

#### コミット済みの変更はない(`[main +x●y…z]`という状態)場合

`git stash`([開発の Tips](./開発のTips.md)参照)を使ってコミットせずに変更を退避させる。

```shell
# 変更を退避させる
# 本来はgit stash -uというコマンド
# g suでもよい
gsu

# エラーにならない
gpull origin main

# ブランチを切るなどする
gcob ブランチ名

# 変更を任意のタイミングで戻す
# 本来はgit stash popというコマンド
# g spでもよい
gsp
```

#### コミット済みの変更がある(`[main x|y]`という状態)場合

以下の二つの方法がある。

- `git reset`を使ってコミットした変更をステージング前まで戻し、`git stash`([開発の Tips](./開発のTips.md)参照)を使ってコミットせずに変更を退避させる。  
   この方法はローカルのコミットが消えるという注意点がある。

```shell
# リモートリポジトリにない変更がなくなるまで(`[main x|0]`という表記になるまで)このコマンドを繰り返す
g reset HEAD^ --soft

# 変更を退避させる
# 本来はgit stash -uというコマンド
# g suでもよい
gsu

# エラーにならない
# (origin mainはgpush -u origin mainか
# gpull --set-upstream origin mainを実行したことがあれば省略可能)
gpull origin main

# ブランチを切るなどする
gcob ブランチ名

# 変更を任意のタイミングで戻す
# 本来はgit stash popというコマンド
# g spでもよい
gsp
```

- [サブブランチと`main`ブランチの間に起きたコンフリクトの場合](#サブブランチとmainブランチの間に起きたコンフリクトの場合)と同じように処理する。  
  この方法ではローカルの変更もリモートの変更もうまく保存することが出来るが、PR を通さずに`main`ブランチにコミットを追加しており、これは避けたほうが良い。

1. [サブブランチと`main`ブランチの間に起きたコンフリクトの場合](#サブブランチとmainブランチの間に起きたコンフリクトの場合)と同じように処理する。
2. なお、最後のローカルで行われた変更をリモートに反映する際のブランチ名は`main`とする。

```shell
# (origin mainはgpush -u origin mainか
# gpull --set-upstream origin mainを実行したことがあれば省略可能)
gpush origin main
```
