# master

　Gradleマルチプロジェクトの masterです。

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
　Gradleタスクでは、GitHubアカウントのパスワードを環境変数GITHUB_PWDから参照します。  
　環境変数GITHUB_PWDに、パスワードを設定してください。

~~~
git remote set-url origin https://longfish801:${GITHUB_PWD}@github.com/longfish801/[リポジトリ名]
~~~

## ブランチモデル

　以下のブランチがあります。  
　ただし master, longfish801.github.ioには developがありません。

* master
* develop
* current

　日々の更新は currentブランチに反映します。  
　改修の切りのよいところで developブランチにマージします（フィックス）。  
　マスターアップ時は masterブランチにマージします（マスターアップ）。
　加えて JARファイルと APIドキュメントを公開します（リリース）。

## コンパイルならびにテスト

　各プロジェクトで実施します。  
　コンパイルならびにテストを実施します。詳細は各プロジェクトの Readmeや build.gradleを参照してください。

~~~
gradle
~~~

　依存ライブラリが更新されても、すぐには反映されないことがあります。  
　その場合、以下を実行してください。

~~~
gradle --refresh-dependencies
~~~

## 日々の更新

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

　上記は masterプロジェクトの build.gradleに記述したタスクで実現しています。  
　これは以下の gitコマンドを実行した場合に相当します（上記 gradleコマンドで一括実行できるため、通常これらを実行する必要はありません）。

　GitHubからローカルへ最新版を反映したいときには、プロジェクトごとに以下を実行します。  
　Xtheirsオプションでリモート側に強制的に合わせていることに注意してください。

~~~
git checkout current
git pull -Xtheirs origin current
~~~

　編集内容をローカルリポジトリならびに GitHubへコミットするには、プロジェクトごとに以下を実行します。

~~~
git add -A
git commit -m "[編集内容のコメントを記述]"
git push origin current
~~~

## フィックス

　機能追加やバグフィックスなど、切りの良いところでフィックスします。  
　developブランチにマージならびに GitHubへ反映しています。

~~~
> currentをコミットおよび GitHubへ反映します
gradle clean push
> developに currentを上書きマージします
git checkout develop
git pull origin develop
git merge --squash -Xours current
gradle
　→マージが失敗している可能性があるため、確認をします
gradle clean
git status
git add -A
git commit -m "[コミット内容の説明]"
> リモートに反映します
git push origin develop
> currentを再作成します
git branch -D current
git push --delete origin current
git checkout -b current
git push -u origin current
~~~

## マスターアップ

　マスターアップするときは、以下を実施してください。  
　なお、先にフィックスを済ませている必要があります。

　build.gradleの version変数の値を確認し、必要であればインクリメントしてください。

　以下のコマンドで masterブランチへマージし GitHubへ反映してください。

~~~
> masterに developを上書きマージします
git checkout master
git pull origin master
git merge --squash -Xours develop
gradle
　→マージが失敗している可能性があるため、確認をします
gradle clean
git status
git add -A
git commit -m "[プロジェクト名] [バージョン] masterup"
> リモートに反映します
git push origin master
~~~

　必要に応じてタグを作成してください。以下はバージョンを 0.1.00とする場合の例です。

~~~
git checkout master
git pull origin master
git tag v0.1.00
git push origin v0.1.00
~~~

## リリース

　リリース対象のプロジェクトにて、以下のコマンドを実行してください。
　JARファイルとAPIドキュメントを longfish801.github.ioリポジトリに出力します。

~~~
git checkout master
git pull origin master
gradle release
~~~

　longfish801.github.ioリポジトリで以下を実行します。
　masterブランチにマージならびに GitHubへ反映しています。

~~~
> currentをコミットおよび GitHubへ反映します
gradle clean push
> masterに currentを上書きマージします
git checkout master
git pull origin master
git merge --squash -Xours current
git status
git add -A
git commit -m "[プロジェクト名] [バージョン] release"
> リモートに反映します
git push origin master
> currentを再作成します
git branch -D current
git push --delete origin current
git checkout -b current
git push -u origin current
~~~

## リポジトリ追加時

　GitHub上に新しいリポジトリを作成し、以下を実行してください。

~~~
> ローカルへクローンします
git clone https://github.com/longfish801/[リポジトリ名]
cd [リポジトリ名]
　→初期ファイルを格納します
> 認証を省略するためリポジトリURLを変更します
git remote set-url origin https://longfish801:${GITHUB_PWD}@github.com/longfish801/[リポジトリ名]
git remote -v
> コミットならびに GitHubへ反映します
git status
git add .
git commit -m "first commit"
git push -u origin master
~~~

　必要に応じて develop, currentブランチを作成してください。

~~~
> developブランチを作成します
git checkout master
git checkout -b develop
git push -u origin develop
> currentブランチを作成します
git checkout develop
git checkout -b current
git push -u origin current
~~~

　masterリポジトリの settings.gradleにリポジトリ名を追記してください。

