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

## Gradle
### masterプロジェクト

　全プロジェクトについて、GitHub上の状態をローカルへ反映します。  
　他の環境から pushしたならば、作業の最初にこれを実行してください。

~~~
gradle clean pull
~~~

　全プロジェクトについて、ローカルの編集内容を GitHubへ反映します。  
　編集作業の終了時にこれを実行してください。

~~~
gradle clean push
~~~

　全プロジェクトとは、正確には master/settings.gradleに記述されているプロジェクトです。

### 各プロジェクト

　ソース編集時、コンパイルならびにテストを実施します。

~~~
gradle
~~~

　依存ライブラリが更新されても、すぐには反映されないことがあります。  
　その場合、以下を実行してください。

~~~
gradle --refresh-dependencies
~~~

　バージョンアップします。  
　事前に build.gradleの version変数の値をインクリメントしてください。  
　プロジェクトの編集内容を GitHubへ反映します。  
　JARファイルとAPIをlongfish801.github.ioフォルダに出力し、GitHubへ反映します。

~~~
gradle versionup
~~~

## Git
### 環境構築

　任意のフォルダを作成し、以下のコマンドを実行してください。  
　GitHubからリポジトリをダウンロードします。  
　すべてのリポジトリについて実行してください。

~~~
git clone https://github.com/longfish801/[リポジトリ名]
~~~

### 新規リポジトリ作成

　GitHub上で新規リポジトリを作成します。  
　以下のコマンドで GitHubからダウンロードします。

~~~
git clone https://github.com/longfish801/[リポジトリ名]
~~~

　master/settings.gradleにリポジトリ名を追記してください。

### 他環境から GitHubを更新した場合にローカルを更新

　上記 gradle pullコマンドで一括実行できるため、通常はこれを実行する必要がありません。  
　各リポジトリ毎に GitHubからローカルへ最新版を反映したいときには以下を実行します。

~~~
git pull
~~~

　なんらかの事情で、ローカルでの編集をすべて破棄する場合は以下を実行します。  

~~~
git checkout .
~~~

### ローカルの編集内容を GitHubに反映

　上記 gradle pushコマンドで一括実行できるため、通常はこれを実行する必要がありません。  
　各リポジトリ毎に編集内容をローカルリポジトリならびに GitHubへコミットするには以下を実行します。

~~~
# すべての変更を含むワークツリーの内容をインデックスに追加
# とりけしは「git reset」
git add -A
# 追加されたか確認する
git status
# コミットする
git commit -m "[編集内容のコメントを記述]"
# リモートリポジトリに反映する
git push
~~~

## gstartプロジェクト
### 動作確認環境の作成

　以下のコマンドで releaseフォルダ配下に動作確認環境が作成されます。

~~~
gradle release
~~~

### Grapeのログ出力を強化

　Grape関連の動作で問題が生じた場合は、ログ出力を強化して調査します。  
　以下を参考にしました。

~~~
groovy grape verbose - Stack Overflow
https://stackoverflow.com/questions/3722280/groovy-grape-verbose
~~~

　gstart.l4j.iniに以下を追記します。

~~~
"-Divy.message.logger.level=4"
"-Dgroovy.grape.report.downloads=true"
~~~

　build.gradleについて headerTypeの指定を有効にします。

~~~
createExe {
	...
	headerType = 'console';
}
~~~

　gradle releaseを実行します。  
　生成された gstart.exeを介してスクリプトを実行します。  
　たとえば WashTxtを実行する場合、以下のコマンドを実行します。  
　stdout.logに Grapeのログが出力されます。

~~~
gstart.exe menu\WashTxt\main.gvy > stdout.log
~~~

### Groovyコマンドで実行

　menuフォルダ配下のスクリプトを groovyコマンドで実行する例を示します。  
　以下の例では Hello.gvyを実行しています。  
　事前に gradle releaseの実行が必要となります。

~~~
set CLASSPATH='.;lib/*;conf'
set JAVA_OPTS=-Dfile.encoding=UTF-8
groovy -c UTF-8 menu\サンプル\Hello.gvy
~~~

以上
