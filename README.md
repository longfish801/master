# master

　Gradleマルチプロジェクトのmasterです。

## Github

　Githubの URLは以下のとおりです。

~~~
Github - longfish801
https://github.com/longfish801
~~~

　GitHub Pagesの URLは以下のとおりです。

~~~
GitHub Pages - longfish801
https://longfish801.github.io/
~~~

## 環境構築

　以下をインストールしてください。

* Java Developers Kit 1.8
* Gradle 4.0
* Git 2.10

　任意のフォルダを作成し、以下のコマンドを実行してください。GitHubからリポジトリをダウンロードします。  
　すべてのリポジトリについて実行してください。リポジトリ名は settings.gradleを参照してください。

~~~
git clone https://github.com/longfish801/[リポジトリ名]
~~~

　パスワード入力を省略するため、リモートリポジトリのURLを修正してください。

~~~
git remote set-url origin https://longfish801:[パスワード]@github.com/longfish801/[リポジトリ名]
~~~

　Gradleタスクでは、GitHubアカウントのパスワードを環境変数GITHUB_PWDから参照します。
　環境変数GITHUB_PWDに、パスワードを設定してください。

## ブランチモデル

　以下のブランチがあります。  
　ただし master, longfish801.github.ioには developがありません。

* master
* develop
* current

　日々の更新は currentブランチに反映します。  
　改修の切りのよいところで developブランチにマージします。  
　マスターアップ時は masterブランチにマージします。

## Gradle
### masterプロジェクト

　全プロジェクトについて、GitHub上の currentブランチの内容をローカルへ反映します。
　他の環境から pushしたならば、作業の最初にこれを実行してください。

~~~
gradle clean pull
~~~

　全プロジェクトについて、currentブランチの編集内容を GitHubへ反映します。  
　編集作業の終了時にこれを実行してください。

~~~
gradle clean push
~~~

### 各プロジェクト

　コンパイルならびにテストを実施します。

~~~
gradle
~~~

　依存ライブラリが更新されても、すぐには反映されないことがあります。  
　その場合、以下を実行してください。

~~~
gradle --refresh-dependencies
~~~

## Git
### リリース

　master, longfish801.github.ioリポジトリでは以下を実行してください。  
　masterブランチにマージならびに GitHubへ反映しています。

~~~
git checkout master
git pull origin master
git merge --squash current
git commit -m "[編集内容のコメントを記述]"
git push origin master
~~~

　master, longfish801.github.io以外のリポジトリでは以下を実行してください。  
　developブランチにマージならびに GitHubへ反映しています。

~~~
git checkout develop
git pull origin develop
git merge --squash current
git commit -m "[編集内容のコメントを記述]"
git push origin develop
~~~

### マスターアップ

　master, longfish801.github.io以外のリポジトリでマスターアップするときは、以下を実行してください。  
　なお、developブランチへのマージは済んでいると仮定しています。

　build.gradleの version変数の値をインクリメントしてください。  
　以下のコマンドで、JARファイルとAPIドキュメントを longfish801.github.ioリポジトリに出力してください。

~~~
gradle release
~~~

　以下のコマンドで masterブランチへマージし GitHubへ反映してください。

~~~
git checkout master
git pull origin master
git merge --squash develop
git commit -m "[編集内容のコメントを記述]"
git push origin master
~~~

　必要に応じてタグを作成してください。以下はバージョンを 0.1.00とする場合の例です。

~~~
git checkout master
git pull origin master
git tag v0.1.00
git push origin v0.1.00
~~~

### 他環境から GitHubを更新した場合にローカルへ反映

　上記 gradle pullコマンドで一括実行できるため、通常これを実行する必要はありません。  
　各リポジトリ毎に GitHubからローカルへ最新版を反映したいときには以下を実行します。

~~~
git checkout current
git pull origin current
~~~

### ローカルの編集内容を GitHubへ反映

　上記 gradle pushコマンドで一括実行できるため、通常これを実行する必要はありません。  
　各リポジトリ毎に編集内容をローカルリポジトリならびに GitHubへコミットするには以下を実行します。

~~~
git checkout current
git pull origin current
git add -A
git commit -m "[編集内容のコメントを記述]"
git push origin current
~~~

## リポジトリ追加時の作業

　GitHub上に新しいリポジトリを作成してください。  
　ローカルへ cloneし、初期ファイルを格納してコミット、リモートリポジトリへ反映してください。

~~~
git clone https://github.com/longfish801/[リポジトリ名]
cd [リポジトリ名]
　→初期ファイルを格納します。

git remote set-url origin https://longfish801:${GITHUB_PWD}@github.com/longfish801/[リポジトリ名]
git remote -v
　→認証を省略するため、リポジトリURLを変更します。

git status
git add .
git commit -m "first commit"
git push -u origin master
~~~

　必要に応じて master, developブランチを作成してください。

~~~
git checkout master
git checkout -b develop
git push -u origin develop

git checkout develop
git checkout -b current
git push -u origin current
~~~

　masterの settings.gradleにリポジトリ名を追記してください。

