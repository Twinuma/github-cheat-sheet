# GitHubカンニング・ペーパー

これはGitやGitHubの隠された機能やよく知られていない機能の一覧だ。これはZach Holmanによる[Git and GitHub Secrets](https://github.com/tiimgreen/github-cheat-sheet)というAloha Ruby Conference 2012におけるセッションを元にしている。併せてZachの[スライド](https://github.com/tiimgreen/github-cheat-sheet)も参照した方が良いだろう。

## 空白の無視

DIFFを表示している時、そのURLに`?w=1`を加えると、空白の変化による差分は表示されなくなり、コード上の変化だけを参照することができる。

## リポジトリのクローン

リポジトリをクローンする時、URLの末尾の`.git`は無くても構わない。

```bash
$ git clone https://github.com/tiimgreen/github-cheat-sheet
```

## Hub - Gitラッパー

[Hub](https://github.com/github/hub)はGitのラッパーとして機能するコマンドライン・ツールで、これを利用するとGitHubをコマンドラインからとても簡単に扱えるようになる。

例えば以下のようにしてリポジトリのクローンが行える:

```bash
$ hub clone tiimgreen/toc
```

これは実際には以下のようなコマンドに変換される:

```bash
$ git clone git://github.com/tiimgreen/toc.git
```

## 直前のブランチ

コマンドラインで直前にいたディレクトリへ移動するには以下のようにすれば良い:

```bash
$ cd -
```

同じようにGitで直前のブランチをチェックアウトすることができる:

```bash
$ git checkout -
# Switched to branch 'master'

$ git checkout -
# Switched to branch 'next'
```

## git.io

[git.io](http://git.io)はGitHubの提供するGitHub専用のシンプルな短縮URLサービスだ。

[http://git.io/wO0xUg](http://git.io/wO0xUg)

## Gists

[Gists](https://gist.github.com/)は少量のコード群を管理する最適な手段だ。いちいちちゃんとしたリポジトリを作成する必要はない。

簡単なものとはいえ、完全なGitリポジトリとして機能するため、以下のようにすれば普通のGitリポジトリと同じようにクローンすることができるだろう:

```bash
$ git clone https://gist.github.com/tiimgreen/10545817
```

![Gists](http://i.imgur.com/dULZXXo.png)

## キーボード・ショートカット

リポジトリをブラウザーで開いている時は、ショートカットを利用して様々なページヘ簡単にアクセスできるようになっている。

`t`を押すとファイルの検索インターフェイスが起動する。

`w`を押すとブランチ選択インターフェイスが起動する。

`s`を押すとファイル検索フォームにフォーカスが当たる。

__ファイルを参照している時__（例: `https://github.com/tiimgreen/github-cheat-sheet/blob/master/README.md`)に`y`を押すと、参照している時の状態で固定されるURLに変更される。つまりそのファイルのコードが後に変化したとしても、そのURLでは今とまったく同じ状態で表示されるということだ。

`Shift+?`を押すとそのページで使える全ショートカットが表示されるだろう。

## コミットからイシューを閉じる

あるコミットでイシューを解決した場合、コミットメッセージで`fix/fixes/fixed`または`close/closes/closed`に続けてイシュー番号を指定すると、指定イシューを閉じることができる。

```bash
$ git commit -m "Fix cock up, fixes #12"
```

こうするとイシュー#12が閉じられ、イシューにはそのコミットへの参照が自動的に追加される。

![Closing Repo](http://i.imgur.com/URXFprQ.png)

## プルリクエストのチェックアウト

プルリクエストをローカル・リポジトリへチェックアウトするには、まず以下のようにコマンドを実行しその変更を取り込む:

```bash
$ git fetch origin '+refs/pull/*/head:refs/pull/*'
```

そして、プルリクエストを番号（例: 42）を指定してチェックアウトする:

```bash
$ git checkout refs/pull/42
```

別の方法としては、まずリモート・ブランチとして取り込み:

```bash
$ git fetch origin '+refs/pull/*/head:refs/remotes/origin/pr/*'
```

それから番号を指定して取り込むこともできる:

```bash
$ git checkout origin/pr/42
```

またプルリクエストの取り込みは、.git/configに以下の行を追加すると自動化することができる:

```
[remote "origin"]
    fetch = +refs/heads/*:refs/remotes/origin/*
    url = git@github.com:tiimgreen/github-cheat-sheet.git
```

```
[remote "origin"]
    fetch = +refs/heads/*:refs/remotes/origin/*
    url = git@github.com:tiimgreen/github-cheat-sheet.git
    fetch = +refs/pull/*/head:refs/remotes/origin/pr/*
```

## イシューの相互リンク

同じリポジトリの違うイシューへリンクを張り参照させたい場合、`#`に続けてイシュー番号を指定する。そうすると自動的にリンクが作成されるだろう。

別のリポジトリのイシューの場合は`user_name/repo_name#ISSUE_NUMBER`とすれば良い（例: `tiimgreen/toc#12`）。

## Markdownファイルでの構文強調

例えばMarkdownファイルでRubyのコードを構文強調したいならば以下のようにする:

    ```ruby
    require 'tabbit'
    table = Tabbit.new('Name', 'Email')
    table.add_row('Tim Green', 'tiimgreen@gmail.com')
    puts table.to_s
    ```

こうすると以下のように表示されることになる:

```ruby
require 'tabbit'
table = Tabbit.new('Name', 'Email')
table.add_row('Tim Green', 'tiimgreen@gmail.com')
puts table.to_s
```

GitHubでは[Linguist](https://github.com/github/linguist)を使って言語を判別し構文強調を行っている。構文強調がサポートされている言語の一覧は[言語定義YAMLファイル](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml)を参照すればわかるだろう。

## 特定のユーザーによるコミット履歴

特定のユーザーによるあるリポジトリへのコミット履歴のみを参照したい場合は、`?author=username`をURLの末尾に付ける。

```
https://github.com/rails/rails/commits/master?author=dhh
```

## 空のコミット

`--allow-empty`オプションを付けると、コードの変化がなくてもコミットを作成することができる。

```bash
$ git commit -m "Big-ass commit" --allow-empty
```

![Trololol](http://img1.wikia.nocookie.net/__cb20130905205853/flylikeabird3/images/0/0c/Mexican_troll_face_by_mariodude12312-d5mtl9z.png)

## ブランチ同士の比較

GitHubのブランチ比較は以下のようなURLで提供されている:

```
https://github.com/user/repo/compare/{range}
```

例えば`{range}`を`master...4-1-stable`に変更して利用する:

```
https://github.com/rails/rails/compare/master...4-1-stable
```

`{range}`には以下のように日付け指定を利用することもできる:

```
https://github.com/rails/rails/compare/master@{1.day.ago}...master
https://github.com/rails/rails/compare/master@{2014-10-04}...master
```
masterブランチと特定の期間または日時との比較が行えるだろう。

## コードの指定行の強調

コードのURLの末尾に`#L52`と付けると、その行番号が強調表示される。

これは範囲指定も可能だ（例: `#L53-L60）:

```
https://github.com/rails/rails/blob/master/activemodel/lib/active_model.rb#L53-L60
```

![Line Highlighting](http://i.imgur.com/8AhjrCz.png)

## Emoji

Emojiはプルリクエストやイシュー、READMEなどで`:name_of_emoji:`と書くと利用できる:

```
:smile:
:poop:
:shipit:
:+1:
```

:smile:
:poop:
:shipit:
:+1:

GitHubでサポートされているEmojiの完全なリストは[Emoji cheat sheet for Campfire and GitHub](http://www.emoji-cheat-sheet.com/)で確認できる。

GitHubで使われているEmojiのトップ5は以下の通りだ:

1. :shipit: `:shipit:`
2. :sparkles: `:sparkles:`
3. :-1: `:-1:`
4. :+1: `:+1:`
5. :clap: `:clap:`

## 画像及びアニメーションGIF

画像やアニメーションGIFはコミットのコメントやREADMEなどで利用できる:

```
![Alt Text](http://image_url.com/image.jpg)
```

![Jim Carrey](http://wac.450f.edgecastcdn.net/80450F/thefw.com/files/2013/05/Irene.gif)

あらゆる画像はGitHubでキャッシュされるので、画像のホスティング先が落ちていたとしても変わらず表示されるだろう。

## 素早く引用

イシューのスレッドでコメントを仕様とした時に他の人のコメントを引用したい場合、引用したい文章を選択した状態で`r`を押すと、ブロック引用の記法を使ってテキストエリアにコピーされる。

![Quick Quote](http://i.imgur.com/TzpMIOA.png)

## Gitステータスのスタイル

```bash
$ git status
```

![git status](http://i.imgur.com/o3PEHAA.png)

こうすることもできる:

```bash
$ git status -sb
```

![git status -sb](http://i.imgur.com/xNI1bT0.png)

## Gitクエリー

指定した文字列を今までのコミット・メッセージから検索して、もっとも新しいものを表示することができる。

```bash
$ git show :/query
```

`query`を検索したい文字列で置き換えると、最新のコミットがそのコミットにおける差分と同時に表示される。

![git show :/query](http://i.imgur.com/SA0oZbE.png)

注: 終了するには`q`を押す。

## マージ済みブランチ

```bash
$ git branch --merged
```

こうすると現在のブランチに既にマージされたブランチの一覧が表示される。

逆にまだマージされていないブランチを表示するには以下のようにする:

```bash
$ git branch --no-merged
```

## 推奨したい.gitconfig

`.gitconfig`とはあらゆる設定が書き込まれるファイルだ。

### エイリアス

エイリアスはGitの呼び出し方を自分で好きなように定義できるヘルパー機能だ。例えば`git a`で`git add --all`を実行するようにすることができる。

エイリアスを追加するには`~/.gitconfig`を開く、以下のような形式で記述していく:

```
[alias]
	co = checkout
	cm = commit
	p = push
```

またはコマンドラインからも行えるだろう:

```bash
$ git config alias.new_alias git_function
```

例えば以下のようにする:

```bash
$ git config alias.cm commit
```

注: エイリアスが複数のコマンドからなる場合はクオートで括る必要がある:

```bash
$ git config alias.ac 'add -A . && commit'
```

おすすめの設定を挙げておこう:

| エイリアス | コマンド | 設定方法 |
| --- | --- | --- |
| `git cm` | `git commit` | `git config --global alias.cm commit` |
| `git ac` | `git add . -A` `git commit` | `git config --global alias.ac '!git add -A && git commit'` |
| `git st` | `git status -sb` | `git config --global alias.st 'status -sb'` |

### コマンドの自動修正

多分今は`git comit`とタイプした場合、以下のような出力を得ることだろう:

```bash
$ git comit -m "Message"
# git: 'comit' is not a git command. See 'git --help'.

# Did you mean this?
# 	commit
```

これを`comit`とタイプした時に`commit`を実行させたい場合、自動修正を有効にすれば良い:

```bash
$ git config --global help.autocorrect 1
```

すると以下のような出力を得るようになるだろう:

```bash
$ git comit -m "Message"
# WARNING: You called a Git command named 'comit', which does not exist.
# Continuing under the assumption that you meant 'commit'
# in 0.1 seconds automatically...
```

### 色設定

Gitの出力をカラフルにするには以下のような設定を加えると良い:

```bash
$ git config --global color.ui 1
```

## 訳注

これは[GitHub Cheat Sheet](https://github.com/tiimgreen/github-cheat-sheet)の日本語訳である。
