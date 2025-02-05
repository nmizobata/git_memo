# Git学習メモ
## Gitコマンド
git config --global user.name mizobata
git config --global user.email bluesky@mizobata.com
git config --global core.editor "code --wait": 主エディタをvscodeにする
git config --global http.proxy http://proxy.fujixerox.com:8080
git config --global init.defaultBranch master
git config --list
git init: ローカルレポジトリを作る
git status: ファイルの状態を確認
git add . : すべてのファイルのステージング
git commit : ステージングしたファイルのコミット
git commit -a ： 変更したファイル(新規ファイルを除く)をステージング&コミット
git commit -m "メッセージ" : コミットコメントを付加してコミット
git checkout -- . : ワークエリアのファイルを直前の状態(ステージングエリアのファイルの状態)に戻す (git restore .)
git reset HEAD . : ステージングエリアのファイルを、最終コミットの状態に戻す
git rm aaa.txt : aaa.txtのファイル削除と同時にgit管理からも外す
git rm -r directory : directoryとそれ以下のファイルを削除すると同時にgit管理からも外す
git log : コミット履歴  git log -p 差分情報も表示
git branch ブランチ名： ブランチの作成
git checkout -b ブランチ名： ブランチの作成＋ブランチへの移動
git checkout ブランチ名： 操作対象をブランチに移行。git switch ブランチ名でも同じ。
git branch : 現在のブランチリストを表示 (現在のブランチには＊がついている) 
git diff：ワーキングエリアとステージングエリアのファイルの差分を表示
git diff --cached：ステージングエリアと最終コミットとの差分を表示
git diff master： (ブランチにいるとき)master(最終コミット)との差分を確認
git branch -d [ブランチ名]： ブランチの削除(マージ済)
git branch -D [ブランチ名]：ブランチの強制削除(未マージのものも含む)
git push origin ローカルブランチ名： github(origin)に指定したブランチをアップする

## 戻したいときのコマンド
[参考1](https://qiita.com/rch1223/items/9377446c3d010d91399b) [参考2](https://git-scm.com/book/ja/v2/Git-のさまざまなツール-リセットコマンド詳説#_チェックアウトとの違い)

編集済(未ステージング)→前回コミット:  git checkout [ファイル名] / git checkout .  (すべて)
編集済(ステージング済)→編集済(未ステージング): git reset [ファイル名] / git reset (すべて)
編集済(ステージング済)→前回コミット: git checkout HEAD -- [ファイル名] / git reset --hard HEAD
任意→指定コミット: git logでコミットidを確認後git reset --hard [コミットid] (すべて)
特定ファイルのみ指定コミット: git checkout [コミットid] [ファイルパス]
ブランチ削除後の指定コミット復活: git reflogでHEAD@{番号}確認後、git branch [新ブランチ] HEAD@{番号}。 [新ブランチ]が作成され復活。なおHEAD@{}はすぐに変わるのでreflogを確認したらすぐに実行すること。

## .gitignore

.gitignoreの記入例: https://github.com/github/gitignore

## Github SSH Keyの設定
### SSH keyの生成
> ssh-keygen -t ed25519 -C "bluesky@mizobata.com"
### 公開キーをクリップボードにコピー
> clip < /c/Users/mizobata/.ssh/id_ed25519.pub
Github設定画面に設定
Github Setting> SSH Key > SSH andGPG Keys > New SSH Keyをクリック。
### 公開キーを張り付ける
Title: (どのパソコンで発行したものかを入れておくとよい) 
Key: 公開キーをペースト
Add SSH keyを押す
### 設定の確認
> ssh -T git@github.com
パスフレーズを入力

## 他者のGithubリポジトリのコピー取得
公開されている他者のリモートリポジトリを開く
右上のForkで自分のGithubアカウントにリポジトリをコピーする。
自分のGithubアカウントにコピーしたリポジトリをローカルリポジトリにクローンする。
1. 適当にフォルダを作成
2. フォルダの中で右クリック > git bashを起動
3. > git clone https:/(URL)を実行
4. git remote -v : リポジトリ名(origin) とURLを表示
   (一つのローカルリポジトリに複数のリモートリポジトリを設定可能なため、名前を付ける)
   
## VSCodeの起動
開いているフォルダで右クリック VSCodeを起動
開いているフォルダで実行されているGit Bashコマンドラインでcode .を実行
(参考: エクスプローラの起動 start .)

## ブランチの運用
- masterブランチは「安定したもの」「確定したもの」に限定すること。
- ブランチ名には日本語も使用できる。
- 改変する場合は必ず作業用のブランチを新たに作って行い、作業が完了(動作が安定していることも確認)したらmasterにマージすること。
- 常にどのブランチに対して操作を行うのか、を意識する。操作対象のブランチは git checkout ブランチ名で指定する。
- ブランチを作らずに編集を始めてしまっても、ステージングエリアに登録する前なら、後付けてbranchを作成、移動することができる。
- 元のファイルとの差分はgit diff masterで確認することができる。
- (コミットする前に)元のファイルに戻したい場合はgit restore . (git checkout -- .)で戻すことができる
- (コミットした後に)masterブランチの状態に戻したい場合は、masterに移動しブランチを削除したうえでまた新しくブランチを作成する？
- ブランチで作業中に、他のブランチのmasterへのマージが行われた場合は、作業中のブランチにmasterの最新状態を取り込んでおく。
### 共同作業をしている場合
- プルリクエストを行い、承認を得たのちブランチをmasterにマージする。
1. ローカルでブランチをコミット
2. リモートリポジトリへpush(リモートで同じ名前(or違う名前に指定も可)
	git push origin <ブランチ名>
3. ブラウザでリポジトリを開きpull request作成を行う。
- どのブランチ(右側)から、ど--のブランチ(左側)にマージしたいのかを指定する。
- 注意: フォークした場合は、フォーク元が右側に表示されているので、自分のリモートリポジトリにするように注意
- コメントとレビュワーを指定。
- レビュワーはコメントを入れて返す。LGTM (looks good to me)はよく使われる
- コミュニケーションが終了したらブランチをmasterブランチにマージする。
-- マージ方法①Create a merge commit: すべてのコミット履歴を残してマージ
--マージ方法②Squash and merge: ブランチの履歴を一つのコミットにまとめてマージ。
-- マージ方法③Rebase and merge: ブランチの起点をベースブランチの最新時点に切り替えてマージ。複数のブランチが一直線の履歴で表現できるようになる。
3a. レビュワーが自分のローカルリポジトリにブランチを取り込んでみたい場合(自分のところでアプリケーションを実行するなど)、以下のように行えばgitはリモートからブランチを取り込んでくる。
- git fetch originでフェッチだけを行う
- git checkout <ブランチ>
4. リモートリポジトリでマージが行われたら、ローカルリポジトリへプル(＝フェッチ＋マージ)を行う。(自分が編集中であってもbranchで行っておりmasterにプルしても影響はないはず)
- masterブランチに切り替える。(ブランチを切り替えずにプルを行うと今のブランチにマスターブランチがマージされてしまうので注意)
   git checkout master (ローカルのmasterブランチに移動するという意味。作業中のブランチがある場合はコミットしておく必要がある)
   git pull origin master  (originのmasterブランチを取り込む、という意味)
どうしても、リポジトリはともかく自分のワークツリーに反映したくないときはフェッチだけ行う。git fetch origin
5. ブランチで作業中に、他のブランチのmasterへのマージが完了したら、都度、作業中のブランチにもマージしておく。
   git checkout 作業ブランチ名  (ローカルの作業ブランチに移動)
   git pull origin master  (originのmasterブランチを取り込む)
   (マージコメントが求められるが、特に不要なのですぐに閉じてよい)


## コンフリクト
### 発生原因
自分と自分以外の編集者によって同じ箇所が変更されているものをマージしようとした。
### 発生するタイミング
- @ローカル:自分のローカルブランチ作成/編集作業中に、GitHubから最新MasterをローカルMasterにプル、ローカルMasterをローカルブランチにマージしようとしたときに発生。(GitHubのMaster変更部分と自分のローカルブランチの変更部分が同じ場所)
-- git merge master > git statusで、both modifiedと表示。 
- @GitHub:自分のローカルブランチをプルリクエストした際、自分の変更箇所がGitHub Masterの変更箇所と同一だった場合に発生。
-- GitHubのプルリクエストの分析結果に、This branch has conflicts that must be resolved.と表示。
### 対処方法
- コンフリクトが発生しているファイルを確認し、vscoldeで編集。その際、gitが付加したコンフリクトマークも消すこと。
- 編集完了後、ステージング > コミット、でコンフリクトが解消した形でマージ完了。
- GitHubへプッシュ。(GitHubで表示されていたConflictメッセージも解消されている)

## オープンソフトウェアの世界
### 検索方法
- "/"で検索。https://help.github.com/articles/searching-for-repositories/を参照。
- stars :>= 10000 でStarが10000個以上のリポジトリ。
- starts:>=10000 language:pythonでpython言語の検索。
- ReadMEファイルにおけるMarkDownの記法。http://guides.github.com/features/mastering-markdown/
