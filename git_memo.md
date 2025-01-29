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
git add . : ファイルの登録
git commit : 登録したファイルのコミット
git checkout -- . : ワークエリアのファイルを直前の状態(ステージングエリアのファイルの状態)に戻す (git restore .)
git reset HEAD . : ステージングエリアのファイルを、最終コミットの状態に戻す
git rm aaa.txt : aaa.txtのファイル削除と同時にgit管理からも外す
git rm -r directory : directoryとそれ以下のファイルを削除すると同時にgit管理からも外す
git log : コミット履歴  git log -p 差分情報も表示
git branch ブランチ名： ブランチの作成
git checkout ブランチ名： 操作対象をブランチに移行。git switch ブランチ名でも同じ。
git branch : 現在のブランチリストを表示 (現在のブランチには＊がついている) 
git diff：ワーキングエリアとステージングエリアのファイルの差分を表示
git diff --cached：ステージングエリアと最終コミットとの差分を表示
git diff master： (ブランチにいるとき)master(最終コミット)との差分を確認


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
- 改変する場合は必ず作業用のブランチを新たに作って行い、作業が完了(動作が安定していることも確認)したらmasterにマージすること。
- 常にどのブランチに対して操作を行うのか、を意識する。操作対象のブランチは git checkout ブランチ名で指定する。
- ブランチを作らずに編集を始めてしまっても、ステージングエリアに登録する前なら、後付けてbranchを作成、移動することができる。
- 元のファイルとの差分はgit diff masterで確認することができる。
- (コミットする前に)元のファイルに戻したい場合はgit restore . (git checkout -- .)で戻すことができる
- (コミットした後に)masterブランチの状態に戻したい場合は、masterに移動しブランチを削除したうえでまた新しくブランチを作成する？
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
   git pull origin master
どうしても、リポジトリはともかく自分のワークツリーに反映したくないときはフェッチだけ行う。git fetch origin
