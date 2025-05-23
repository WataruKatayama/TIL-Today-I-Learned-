# Springの環境構築設定

今回の動作環境は以下のものとする。  
#### ・開発環境フレームワーク：　Spring tools 4 for Eclipse  
#### ・実行環境：　Open JDK 11  
#### ・ビルドツール：　Maven 3.8   
#### ・SQL：　PostgreSQL 13  

今回はわかりやすくするため、上記のツールを下記のディレクトリにひとまとめに格納することとする。  
C:/Tools  

  


### ⓵下記のリンクからそれぞれダウンロードし、指定のディレクトリに格納する。(無ければ作る)  

  
#### Open JDK 11  
インストール先  
C:/Tools/AdoptOpenJDK/  
  
https://www.openlogic.com/openjdk-downloads?field_java_parent_version_target_id=406&field_operating_system_target_id=436&field_architecture_target_id=391&field_java_package_target_id=396  

#### PostgreSQL 13  
インストール先  
C:/Tools/PostgreSQL/13  
  
https://www.enterprisedb.com/downloads/postgres-postgresql-downloads  
  
#### Spring tools 4 for Eclipse  
インストール先  
C:/Tools/  

https://spring.io/tools
  
#### Maven 3.8  
インストール先  
C:/Tools/  

※Mavenは下記のリンクにある「File」項目の「Binary zip archive(Link列)」からダウンロードする。  
https://maven.apache.org/download.cgi  


### ⓶PCに環境変数を設定する  
・windowsの場合  
1)スタートメニューバーから環境変数で検索  
2)「システム環境変数の編集」を選択  
3)「システムのプロパティ」ウィンドウが開くので、「環境変数(N)」を選択  
4)「PATH」をクリックし、「編集(E)」を選択  
5)「新規(N)」をクリックし以下のリンク(binまでのリンク)を追加する  
  ※下記のリンクは自分のリンクをコピペしただけなので、下記をそのままコピペせず、  
  　詳しいbinまでのリンクは自分のものをコピペすること。  
  ・C:\tools\PostgreSQL\13\bin  
  ・C:\tools\AdoptOpenJDK\jdk-11.0.9.101-hotspot\bin  
  ・C:\tools\apache-maven-3.9.9\bin  

・Macの場合  
1)まずはターミナル(windowsでいうcmd)を開いて、MacのShell確認をする  
  使用コマンド　echo $SHELL  
  ※通常は、zsh か bash かのどちらかになる  
  　zshの場合：.zshrc ファイル  
　　bashの場合：.bash_profile ファイル  
2)自分のホームディレクトリに移動する  
　使用コマンド　cd ~  
3)今の環境に設定ファイルはあるかどうか確認し、なかった場合は作成する  
　使用コマンド　確認　ls -al　、　作成　touch .zshrc  

　＜なぜわざわざ作る必要があるのか＞  
   環境変数の導入はexportコマンドだが、exportでは環境変数の値を永続的に保持することがでない。  
　 そのため、 .zshrc または .bash_profile という設定ファイルを使用して、環境変数PATHの値を保持させることができる。  
　 このファイルに設定を加えれば、ターミナルを開いた時にその中に記述した設定を反映することができる。  

4)ls -a で一覧に.zshrcファイルがあるか確認する  
　※もしコマンド無効のコメントが返ってきた場合は、PATHをリセットする必要があるので、以下のコマンド操作を行う。  
 　　 環境変数のパス　リセットコマンド  
　　　ターミナルで以下のコマンドをそのままコピペ  
     ```
     export PATH=/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/sbin:/usr/local/bin  
     ```  


　　　ls -a でファイル一覧が表示されるか確認する。  

5)open .zshrc コマンドで.zshrcファイル自体を開く  
　※.zshrcファイルのウィンドウが開かれることを確認する。  
   
6).zshrcファイルのウィンドウの中に直接下記のコマンドを記述し、環境変数のパスを登録する。(/binまでのパスを記述する)  

  　```export PATH="追加したいパス:追加したいパス:追加したいパス:$PATH"```  

  使用例  
　```export PATH="/Applications/Tools/AdoptOpenJDK/openlogic-openjdk-11.0.25+9-mac-x64/jdk-11.0.25.jdk/Contents/Home/bin:/Applications/Tools/PostgreSQL/13/bin:/Applications/Tools/apache-maven-3.9.9/bin:$PATH"```  
  
   ※ 今回のように複数のパスを登録したい場合は、一つ目のパスの後に「:(コロン)」を記述し、その後に2つ目、３つ目のパスを記述する。  
     
7)env または　printenvで環境変数が追加されているか確認する。  

### ③メイブンコマンドでプロジェクトを作成する。  
※今回は/Toolsディレクトリ下に作成することを前提としている。  
  
1)コマンドプロンプト(macはターミナル)でToolsディレクトリに移動する。  
 ```cd ~/.....Tools```

2)Maven Archetype Plugin の mvn archetype:generate を利用して、プロジェクトを作成する。  

  コマンド   
  ```
  mvn archetype:generate -DarchetypeGroupId=com.github.macchinetta.blank -DarchetypeArtifactId=macchinetta-batch-archetype -DarchetypeVersion=2.3.0.RELEASE
```
3)以下の内容を対話式で入力する  
 項目名         設定例  
groupId : com.example.batch  
artifactId : macchinetta-batch-tutorial  
version : 1.0.0-SNAPSHOT  
package : com.example.batch.tutorial  

4)y（yesという意味)を入力した後ににEnterで「BUILD SUCCESS」が出ることを確認する。  
```bash
C:\xxx>mvn archetype:generate -DarchetypeGroupId=com.github.macchinetta.blank -DarchetypeArtifactId=macchinetta-batch-archetype -DarchetypeVersion=2.3.0.RELEASE
[INFO] Scanning for projects&#8230;&#8203;
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] ------------------------------------------------------------------------
(.. omitted)
  Define value for property 'groupId': com.example.batch
  Define value for property 'artifactId': macchinetta-batch-tutorial
  Define value for property 'version' 1.0-SNAPSHOT: : 1.0.0-SNAPSHOT
  Define value for property 'package' com.example.batch: : com.example.batch.tutorial
  Confirm properties configuration:
  groupId: com.example.batch
  artifactId: macchinetta-batch-tutorial
  version: 1.0.0-SNAPSHOT
  package: com.example.batch.tutorial
   Y: : y
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Archetype: macchinetta-
  batch-archetype:2.3.0.RELEASE
[INFO] ----------------------------------------------------------------------------
  [INFO] Parameter: groupId, Value: com.example.batch
  [INFO] Parameter: artifactId, Value: macchinetta-batch-tutorial
  [INFO] Parameter: version, Value: 1.0.0-SNAPSHOT
  [INFO] Parameter: package, Value: com.example.batch.tutorial
  [INFO] Parameter: packageInPathFormat, Value: com/example/batch/tutorial
  [INFO] Parameter: package, Value: com.example.batch.tutorial
  [INFO] Parameter: groupId, Value: com.example.batch
  [INFO] Parameter: artifactId, Value: macchinetta-batch-tutorial
  [INFO] Parameter: version, Value: 1.0.0-SNAPSHOT
  [INFO] Project created from Archetype in dir: C:\xxx\macchinetta-batch-tutorial
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD SUCCESS
  [INFO] ------------------------------------------------------------------------
  [INFO] Total time: 45.293 s
  [INFO] Finished at: 2020-03-04T16:48:55+09:00
  [INFO] Final Memory: 16M/197M
  [INFO] ------------------------------------------------------------------------
```

5)macchinetta-batch-tutorialというプロジェクトが/Tools下に作成されたので、macchinetta-batch-tutorialのディレクトリがあるか確認する。  
 ※cdコマンドで「macchinetta-batch-tutorial」が存在するか確認できる。存在していたら作成されたということ。  
 ※実際にエクスプローラーで確認してもよい。
 ```bash
cd macchinetta-batch-tutorial
```

6)作成されたプロジェクトをきれいに整理する。  
  コマンド  
  ```bash
  mvn clean dependency:copy-dependencies -DoutputDirectory=lib package
  ```

7)正しくプロジェクトが作成されたかどうか、サンプルのジョブを実行してみる。  
  ※今回は、javaのcp(クラスパス)コマンドでクラスパス（ライブラリやクラスファイルを検索する場所）を指定して、必要な処理を読み込み、実行するコマンドを使用してみる。      
  windowsの場合  
  ```bash
  java -cp "lib/*;target/*" org.springframework.batch.core.launch.support.CommandLineJobRunner META-INF/jobs/job01.xml job01
  ```

  macまたはlinuxの場合  
  ```bash
  java -cp 'lib/*:target/*' org.springframework.batch.core.launch.support.CommandLineJobRunner META-INF/jobs/job01.xml job01
  ```

8)実行結果に　「status: [COMPLETED] in　~~~ms」が表示されていれば成功。  


### ④プロジェクトをインポートする  






  
