# Git学習メモ
## Gitコマンド
git init: ローカルレポジトリを作る
git status: ファイルの状態を確認
git add . : ファイルの登録
git commit : 登録したファイルのコミット
git checkout -- . : ワークエリアのファイルをステージングエリアのファイルの状態に戻す (git restore .)
git reset HEAD . : ステージングエリアのファイルを、最終コミットの状態に戻す
git rm aaa.txt : aaa.txtのファイル削除と同時にgit管理からも外す
git rm -r directory : directoryとそれ以下のファイルを削除すると同時にgit管理からも外す
git log : コミット履歴  git log -p 差分情報も表示


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