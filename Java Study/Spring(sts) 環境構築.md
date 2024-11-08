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
　　　export PATH=/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/sbin:/usr/local/bin  

　　　ls -a でファイル一覧が表示されるか確認する。  

5)
