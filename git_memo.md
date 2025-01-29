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

## .gitignore
