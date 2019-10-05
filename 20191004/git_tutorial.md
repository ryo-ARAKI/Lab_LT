# Git/GitHub入門

## Git とは何か？

### 研究とデータのバックアップ

#### 一度つくったらそれを保存しておく

- 実験結果，計算結果（.bin, .dat, HDF5, ...）

#### 継続的に改良・修正を重ねていく

- ソースコード（.f90, .c, .py, ...）
- (卒|修|博)論・投稿論文（.tex, .pdf）
- → **Git/GitHubで管理しよう！**

### Git ≠ GitHub

- [Git](https://git-scm.com/)：分散型バージョン管理システム（の中で一番広く使われているもの）
  - 他には **集中型** バージョン管理システムである[Subversion](https://subversion.apache.org/)が有名
- [GitHub](https://github.co.jp/)：Gitを用いたWebサービス（の中で一番広く使われているもの）
  - 他にも[Bitbucket](https://bitbucket.org/)や **自身で用意したサーバにインストールできる** [GitLab](https://about.gitlab.com/)が有名

### ownClowd/研究室サーバ/GoogleDrive/DropBox/で良くない？

- 変更箇所を明示的に記録できる（ `commit` ）
  - ↔ 自動で保存されてしまう
    > 前の状態に戻したいんだけど，あれっていつのバージョンだっけ？
  - ↔ 古い状態を残しておくことができない
    > hogehoge_v1.pdf
    > hogehoge_v2.pdf
    > hogehoge_v3.pdf
  - ↔ コメントアウトまみれのソースコードからの脱却
    > 今はこの関数やサブルーチンを使ってないけど，また使うかもしれないしとりあえずコメントアウトして残しておこう

- 共同編集に向く（ `branch`, `issue`, `pull request` ）
  - ↔ 編集の競合をうまく処理してくれなかったりする
    > 今からこのファイル編集するから触らないでね！

> 実験データ，計算データなど，一度作成したらもう編集しないデータ → （普通の意味での）バックアップ
ソースコード，論文データなど，継続的に改良していくデータ → （Gitなどを使った）バージョン管理

- 参考：[研究のプログラミングにおける悲劇を無くすためのGitとテスト](http://kesin.hatenablog.com/entry/2014/02/27/%E7%A0%94%E7%A9%B6%E3%81%AE%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0%E3%81%AB%E3%81%8A%E3%81%91%E3%82%8B%E6%82%B2%E5%8A%87%E3%82%92%E7%84%A1%E3%81%8F%E3%81%99%E3%81%9F%E3%82%81)

----

## Gitの構造

### 4つの「場所」

- 作業ツリー（作業用ディレクトリ）
  - 普通の意味のディレクトリ/フォルダ
- ステージングエリア
  - Gitに変更を登録するため，変更点をためておく領域
- ローカルリポジトリ
  - 「ひとまとめの変更（= `commit`）」を登録する領域
- リモートリポジトリ
  - サーバ上に存在するローカルリポジトリの複製

### Gitのファイル管理

- Gitはファイルの差分を管理している
  - ↔ファイルに別名をつけてバックアップとして保存
  - ファイルの変更点に名前をつけ，まとめて管理している

----

## Gitの基本的なコマンド

### ローカルでの作業

- `git add`
  - `作業ツリー` にあるファイルを， **Gitで管理するものとして** `ステージングエリア` に登録する
  - `git rm` ： `ステージングエリア` に登録されているファイルを **削除**
  - `git mv` ： `ステージングエリア` に登録されているファイルを **移動**
  - `git add .` ：全てのファイルの変更点を `ステージングエリア` に登録
- `git commit`
  - `ステージングエリア` に登録されている変更点（の集まり）を `ローカルリポジトリ` に登録する
- `git reset HEAD`
  - `ローカルリポジトリ` に登録されたコミットを取り消す
  - `git reset --hard` ：ローカルリポジトリに登録された変更点を **破棄** （ファイルは元の状態に戻る）
  - `git reset --hard` ：ローカルリポジトリに登録された変更点を **保持** （コミットのみ取り消され，ファイルは変更した状態のまま）
- `git checkout`
  - `作業ツリー` を `ステージングエリア` の状態に戻す
  - `checkout` は `作業ツリー` を切り替えるコマンド→branchの切り替えにも使う
- `git stash`
  - `作業ツリー` の現状に（名前をつけて）退避しておき， `ステージングエリア` の状態を呼び出す

### リモートとの作業

- `git push`
  - `ローカルリポジトリ` に記録されたコミットを `リモートリポジトリ` に登録する
- `git fetch`
  - `リモートリポジトリ` の情報と `ローカルリポジトリ` の情報を同期する
  - **※この時点ではローカルの作業ディレクトリはリモートリポジトリの状態を反映していない**
- `git merge`（もしくは`git rebase`）
  - （ `リモートリポジトリ` から情報をとってきた） `ローカルリポジトリ` の情報を `作業ディレクトリ` に統合する
  - `merge` と `rebase` の違い
    - `merge` ：変更内容を「取り込む」
      - 「マージコミット」が作成され，そこに取り込まれる変更点がまとめられる
    - `rebase` ：変更内容を「つなげる」
      - 取り込まれるコミット群を現在のリビジョンの先頭につなぐ
- `git pull`
  - `git fetch` + `git merge`

----

## GitHubの使い方

### アカウント作成

1. [GitHub](https://github.com/) にアクセス， `Sign up for GitHub` から登録する
  研究室のメールアドレス（ `*@fm.me.es.osaka-u.ac.jp` ）を使うこと

### 学生プランの申請

有料プランの機能を無料で使える！

1. [Get GitHub Education benefits](https://education.github.com/discount_requests/new) にアクセス，空欄を埋めて `Submit your information` を押す．数日でメールが来るはず

### SSH接続

1. 秘密鍵の作成（分かってる人は次へ）
  1-1. sshのディレクトリに移動
  1-2. 鍵を生成する
  `ssh-keygen -t rsa`
  1-3. パスフレーズを登録する
2. GitHubに秘密鍵を登録
  2-1. [GitHubのSSH設定ページ](https://github.com/settings/keys) にアクセス
  2-2. `New SSH Key` をクリック
  2-3. `Title` に名前（ `GitHub_access` とかで良い）， `key` に公開鍵（ `~/.ssh/*.pub` ファイルの中身）を入力
3. SSH接続の確認
  3-1. ターミナルで次を入力
  `ssh -T git@github.com`
  3-2. 次の文章が帰ってきたら成功
    > Hi username! You've successfully authenticated, but GitHub does not provide shell access.

### リポジトリの作成

1. [GitHub](https://github.com/) にアクセス
2. `+ New repository` をクリック
3. `repository name` を入力， `Public/Private` を選択， `Initialize this repository with a README` をクリックして `Create repository` を押す

### リポジトリのクローン

1. リモートリポジトリの情報を取得
  1-1. リモートリポジトリのページにアクセス
  1-2. `Clone or download` をクリック
  1-3. `git@github.com:username/repositoryname.git` をコピーする
2. ローカルリポジトリを作成
  2-1. 当該リポジトリを置きたいディレクトリで次を実行
  `git clone git@github.com:yousername/repositoryname.git`
  2-2. `ls` でローカルリポジトリができていることを確認

----

## グループでの開発（orトピックブランチを使った個人開発）に向けて

### 目標

次の文章を理解する：

> forkしてcloneしてpullしてbranch切ってaddしてcommitしてpushしてpull requestしてmergeしてもらう

- 出典：[forkしてcloneしてbranch切ってpullしてcommitしてpushしてpull requestしてmergeしてもらおう](http://uraway.hatenablog.com/entry/2015/11/26/fork%E3%81%97%E3%81%A6clone%E3%81%97%E3%81%A6branch%E5%88%87%E3%81%A3%E3%81%A6pull%E3%81%97%E3%81%A6commit%E3%81%97%E3%81%A6push%E3%81%97%E3%81%A6pull_request%E3%81%97%E3%81%A6merge%E3%81%97%E3%81%A6)

### 問題

- Q. 上の一連の工程で，実際にファイルの編集や新規作成，削除をおこなうのはどのタイミングでしょうか？
- A. `pull` と `add` の間

### コマンドの説明

- `fork`
  - ある（自分のものではない）リポジトリを自身の `リモートリポジトリ` にコピーする
  - ※forkしたことが元リポジトリのオーナーに通知される
- `clone`
  - `リモートリポジトリ` をローカルに複製し， `ローカルリポジトリ` を作成する
  - 自分のものではないリポジトリを `clone` して自身のローカルに複製することも可能．この場合，cloneしたことは元リポジトリのオーナーに通知されない
- `pull`
  - リモートの最新状態を取得しておく
  - ここまでの手順を一気におこなったのであれば必要ない
- `branch` （を切る）
  - 元のコードに影響を与えないパラレルワールドを作成する
  - `git checkout -b ブランチ名`
  - 作業したい内容の名前でブランチを作る（トピックブランチ）
- ここで実際の作業を行なう
- `add`
  - 作業した内容を `ステージングエリア` に登録する
- `commit`
  - `ステージングエリア` にまとめた変更点を `ローカルリポジトリ` に登録する
- `push`
  - `ローカルリポジトリ` に登録されたコミットを `リモートリポジトリ` に送信する
- `pull request`
  - 自身の `リモートリポジトリ` の状態を元リポジトリに反映してもらうお願いを送る
- `merge`
  - バグがないことなどをチェックした後，元リポジトリに自身のおこなった変更が統合される

----

## 課題

> 次のトピック（のうち一つ以上）について自分自身で調べ，この資料に追記してください．
その際，前節で紹介した「グループでの開発」手順を用い，[この資料が管理されているGitHubリポジトリ](https://github.com/ryo-ARAKI/git_tutorial) を元に，適切な手順でファイルの編集とその反映をおこなってください．

### トピックリスト

- `.gitignore` ファイル
- issue機能
- GUIで(Windowsで)使えるGitソフト
- GitHubとslackの連携
- SSHを使ったGitHubへの接続
- commit messageのわかりやすい書き方
- 研究室運営とGitHub organisation
- 論文執筆とGitHub
- GitHub Educationで使える機能
- [意外と知らない？ Gitコマンド 100本ノック](https://qiita.com/ueki05/items/5c233773e3186989bfd3) で気になったコマンドを調べる
- 「GitHub tips」とかで調べて役立ちそうなコマンドを調べる

----

## リンク集

### 必読のやつ

- [Pro Git book（日本語版）](https://git-scm.com/book/ja/v2)
  - 有名なGitの書籍で，オンラインで読むことができます（PDFもダウンロードできます）．Linuxでのコマンド操作をベースに解説されており，やや敷居が高いかもしれませんが，とてもよい資料と思います．
- [サルでもわかるGit入門](https://backlog.com/ja/git-tutorial/)
  - 「まずGitの勉強をしたい」ときにおすすめします．豊富な（かわいい）絵で説明されているので，かなりとっつきやすいと思います．
- [いつやるの？Git入門 v1.1.0](https://www.slideshare.net/matsukaz/git-28304397)

### 目的別

- [(翻訳)【GitHub公式】Gitコマンドチートシート](https://qiita.com/okamu_/items/d52a6900311ad9073628)
  - 「とにかく使い方を把握したい」人は読みましょう．公式のチートシートです．これに沿って作業していればなんとかなる…と思います．
- [今日からはじめるGitHub 〜 初心者がGitをインストールして、プルリクできるようになるまでを解説](https://employment.en-japan.com/engineerhub/entry/2017/01/31/110000)
  - 「一つの記事でgit使えるようになりたい！」ときにおすすめします．よくまとまっており，最低限の使い方は分かるようになると思います．
- [君には1時間でGitについて知ってもらう(with VSCode)](https://qiita.com/jesus_isao/items/63557eba36819faa4ad9)
- [VSCodeでのGitの基本操作まとめ](https://qiita.com/y-tsutsu/items/2ba96b16b220fb5913be)
  - 「ターミナルで作業するの嫌だ」「VSCode使ってる」人におすすめの記事です．
- [Github勉強会](https://www.slideshare.net/FromAtom/github-26243260)
  - 研究室内でのGitHub勉強会の資料です．モチベーションは我々と同じなので，読みやすい資料だと思いす

### 中級者以上向け

- [永久保存版】Gitのあらゆるトラブルが解決する神ノウハウ集を翻訳した](https://blog.labot.jp/entry/2019/07/01/183204)
  - Gitを使っていると「やっちまった！」という瞬間は多々ありますが，そんな時に役立つノウハウ集（の翻訳）です．最初は何を説明しているのかわからないと思いますが，とてもありがたい記事です．
- [Git/GitHubレベル別オススメ学習サイトまとめ完全保存版【2019.06】](https://qiita.com/think-a-lot/items/b3c2e9060f46f5d4ea46)
- [こわくない Git](https://www.slideshare.net/kotas/git-15276118)
  >「マージがなんとなく怖い」「リベースするなって怒られて怖い」「エラーが出て怖い」 Git 入門者にありがちな「Git 怖い」を解消するため、Git のお仕事（コミット、ブランチ、マージ、リベース）について解説します。

### その他

- [クソ簡単にgitの説明をする](https://anond.hatelabo.jp/20190203175803)
  > Q．git完全に理解した
  > A．絶対わかってないから気をつけろ
