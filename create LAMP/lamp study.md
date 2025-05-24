下準備  
[ec2-user@ip-172-31-33-39 ~]$ sudo dnf update -y ←DNFを最新にアップデート　-yは回答に全てyesで実行するという意味  
Amazon Linux 2023 Kernel Livepatch repository                           82 kB/s |  14 kB     00:00      
Dependencies resolved.  
Nothing to do.  
Complete!　コンプリートが出ればOK  
  
[ec2-user@ip-172-31-33-39 ~]$ sudo dnf install -y httpd wget php-fpm php-mysqli php-json php php-devel  
  
httpd:webサーバ　Apache  
wget: ネットワークからデータをダウンロードするためのパッケージ  
php-fpm: fastCGI Process Managerの略。ウェブサーバでPHPを効率よく実行するためのもの。  
php-mysqli: PHPからMySQLデータベースに接続するためのツール  
php-json: PHPでjson形式のデータを処理するためのツール  
php: PHP言語のコアパッケージ  
php-devel: デベロップの略。PHP開発ツールとライブラリ。PHPをより良くカスタマイズするためのもの  
  
最後に　Complete!　と出ればインストール完了  

ApacheとPHPのバージョン確認  
[ec2-user@ip-172-31-33-39 ~]$ httpd -v　←バージョン確認コマンド  
Server version: Apache/2.4.62 (Amazon Linux)  
Server built:   Jul 23 2024 00:00:00  
[ec2-user@ip-172-31-33-39 ~]$ php -v　←バージョン確認コマンド  
PHP 8.3.16 (cli) (built: Jan 14 2025 18:25:29) (NTS gcc x86_64)  
Copyright (c) The PHP Group  
Zend Engine v4.3.16, Copyright (c) Zend Technologies  
    with Zend OPcache v8.3.16, Copyright (c), by Zend Technologies  
  
Apacheの起動  
[ec2-user@ip-172-31-33-39 ~]$ sudo systemctl start httpd ←管理者権限で実行する必要あり。systemctl:サービスの起動などをするコマンド  
[ec2-user@ip-172-31-33-39 ~]$ ←何も表示されなければ実行成功。  
  
Apacheの起動確認  
[ec2-user@ip-172-31-33-39 ~]$ sudo systemctl status httpd  
● httpd.service - The Apache HTTP Server  
     Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; preset: disabled)  
    Drop-In: /usr/lib/systemd/system/httpd.service.d  
             └─php-fpm.conf  
     Active: active (running) since Sun 2025-03-16 12:40:40 UTC; 2min 25s ago　←Activeステータスがactive (running)になっていればOK  
       Docs: man:httpd.service(8)  
   Main PID: 31113 (httpd)  
     Status: "Total requests: 0; Idle/Busy workers 100/0;Requests/sec: 0; Bytes served/sec:   0 B/sec"  
      Tasks: 177 (limit: 1111)  
     Memory: 13.0M  
        CPU: 138ms  
     CGroup: /system.slice/httpd.service  
             ├─31113 /usr/sbin/httpd -DFOREGROUND  
             ├─31139 /usr/sbin/httpd -DFOREGROUND  
             ├─31140 /usr/sbin/httpd -DFOREGROUND  
             ├─31141 /usr/sbin/httpd -DFOREGROUND  
             └─31142 /usr/sbin/httpd -DFOREGROUND  

Mar 16 12:40:40 ip-172-31-33-39.ap-northeast-1.compute.internal systemd[1]: Starting httpd.service - T>  
Mar 16 12:40:40 ip-172-31-33-39.ap-northeast-1.compute.internal systemd[1]: Started httpd.service - Th>  
Mar 16 12:40:40 ip-172-31-33-39.ap-northeast-1.compute.internal httpd[31113]: Server configured, liste>  
lines 1-21/21 (END)  
最後に:qで終了  

システムブート(起動)するたびにApacheウェブサーバが自動起動するようにする　←Linuxを起動するたびにApacheを自動で起動するということ  
[ec2-user@ip-172-31-33-39 ~]$ sudo systemctl enable httpd  
Created symlink /etc/systemd/system/multi-user.target.wants/httpd.service → /usr/lib/systemd/system/httpd.service.  
[ec2-user@ip-172-31-33-39 ~]$  
一度、サーバを停止、再起動をして自動起動することを確認  
→sudo systemctl status httpdコマンドでActiveステータスがactive (running)になっていればOK  
  
以下のコマンドでも自動起動しているか確認できる  
[ec2-user@ip-172-31-33-39 ~]$ sudo systemctl is-enabled httpd  
enabled　←enabledが出れば自動起動が有効になっているということ  
  
AWSのセキュリティグループの設定を確認する  
(わかりやすくいうとファイアウォールの確認を行う)  
→AWSでは何も設定しないとEC2インスタンスの外部からアクセスができないようなっている  
 →必要なポートをあらかじめ開けておくことで特定のアクセスができるようになる  
→サーバの外部からWebのアクセスを許可しなければならないので、80番のポートを開ける必要がある  
確認手順  
1.AWSブラウザのEC2インスタンス一覧ページ(インスタンス起動があるページ)にいく  
2.下にセキュリティタブがあるのでそこをクリック  
3.インバウンドルールにポート番号80があり、ソースが0.0.0.0/0になっていればOK  
　→0.0.0.0/0 すべてのアドレスからアクセスできる設定という意味  
ない場合  
1.上記と同様の場所へいき、セキュリティグループのID(青くなっている)をクリック  
2.インバウンドルールの編集をクリック  
3.ルールの追加をクリックし、HTTPを選択し、ソースをanywhere-IPv4、0.0.0.0/0に設定し保存する  
  
Webブラウザの表示確認  
1.セキュリティグループの確認でも表示したインスタンス一覧ページをひらく  
2.詳細タブをクリックし、パブリックIPv4アドレスにあるアドレスをコピペ  
3.ブラウザで検索すると起動初期では　It Works!　と表示される  
  
  
Apacheドキュメントルートでファイルを編集し、webページを表示させるようにする  
[ec2-user@ip-172-31-33-39 ~]$ ls -l /var/www/ ←このパスにApacheのドキュメントルートが格納されている  
total 0  
drwxr-xr-x. 2 root root 6 Jul 30  2024 cgi-bin  
drwxr-xr-x. 2 root root 6 Jul 30  2024 html　←このように/var/www/htmlディレクトリにドキュメントルートが格納されていることが確認できる  
※ドキュメントルート：Webサイトで公開されるHTMLファイルや画像データなどが格納される場所  
  
[ec2-user@ip-172-31-33-39 ~]$ touch  /var/www/html/index.html  
touch: cannot touch '/var/www/html/index.html': Permission denied　←権限がないと作成できない  
  
このままではwebページが作成できないので権限を付与する（ついでにapacheグループにも権限を付与する)  
1⃣現在のユーザをapacheグループに加える(グループはapacheインストール時にすでに作成されている)  
[ec2-user@ip-172-31-33-39 ~]$ sudo usermod -aG apache ec2-user  
[ec2-user@ip-172-31-33-39 ~]$ groups  
ec2-user adm wheel apache systemd-journal  
[ec2-user@ip-172-31-33-39 ~]$ sudo chown -R ec2-user:apache /var/www/　←/var/www配下を所有ユーザec2-user、所有グループapacheに権限変更するように設定  
[ec2-user@ip-172-31-33-39 ~]$ ls -l /var/www/  
total 0  
drwxr-xr-x. 2 ec2-user apache 6 Jul 30  2024 cgi-bin  
drwxr-xr-x. 2 ec2-user apache 6 Jul 30  2024 html  
  
2⃣グループに対する書き込み権限を付与させる  
[ec2-user@ip-172-31-33-39 ~]$ sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;  
※コマンドの内容の要約  
ルートユーザとして/var/wwwディレクトリとその配下にあるすべてのサブディレクトリのパーミッションを変更し、  
所有者は読書き実行の権限、グループとその他のユーザは読取りと実行のみの権限を持つようにする。  
更に、新しく作られるファイルやディレクトリは元のディレクトリと同じグループ所有権を持つようにする。  
～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～  
chmod:パーミッション変更コマンド  
2775:権限の前に2を設定すると、ディレクトリ内で新規作成されたファイルやディレクトリは元のディレクトリと同じグループ所有権を持つようになる  
-exec:findで検索したファイルに対してコマンドを実行するオプション(ここではsudo chmod 2775)  
{}:findコマンドで検索された各ディレクトリを対象にするという意味  
～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～  
  
[ec2-user@ip-172-31-33-39 ~]$ find /var/www -type f -exec sudo chmod 0664 {} \;  
※コマンドの内容の要約  
ルートユーザとして/var/wwwディレクトリとその配下にあるすべてのサブディレクトリの中で "ファイルに対して" パーミッションを変更し、  
所有者とグループは読書きのみの権限、グループとその他のユーザは読取りのみの権限を持つようにする。   
  
実行前  
[ec2-user@ip-172-31-33-39 ~]$ ls -l /var/www/  
total 0  
drwxr-xr-x. 2 root root 6 Jul 30  2024 cgi-bin  
drwxr-xr-x. 2 root root 6 Jul 30  2024 html  
実行後  
[ec2-user@ip-172-31-33-39 ~]$ ls -l /var/www/  
total 0  
drwxrwsr-x. 2 ec2-user apache 6 Jul 30  2024 cgi-bin　←実行前と比較するとグループパーミッションの実行がxからsになっている  
drwxrwsr-x. 2 ec2-user apache 6 Jul 30  2024 html　　　これはchmod設定時、権限の前に2を設定したから  

[ec2-user@ip-172-31-33-39 ~]$ touch /var/www/index.html  
[ec2-user@ip-172-31-33-39 ~]$ 　　　　　　　　　　　　　　　←メッセージが出ることなく無事に作成できた  
  
これにより、ドキュメントルートを編集できるようになる。  
  
PHP動作確認  
1⃣phpinfoページを作成しPHPの動作を確認する。ブラウザ上でphpinfoページを表示させる。  
→新規PHPファイルを作成することでLAMPサーバが正常に動作するかをテストする  
  
[ec2-user@ip-172-31-33-39 ~]$ echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php ←phpinfoファイルをドキュメントルートに作成しつつ、その中にphpの情報を表示させる関数を記述する  
[ec2-user@ip-172-31-33-39 ~]$ 　←何も表示されなければOK  
" ":echoで内容を記述する際に囲み記号として必要  
<?PHP ?>:PHP関数を記述する際に囲み記号として必要  
phpinfo();:PHPの設定情報を表示させる関数  
  
2⃣セキュリティグループの確認でも表示したインスタンス一覧ページをひらき、詳細タブをクリックし、パブリックIPv4アドレスにあるアドレスをコピペする  
  
3⃣2⃣のアドレスのすぐ後ろに/phpinfo.phpと入力し検索する  
  
4⃣PHPの詳細情報ブラウザが表示されればPHPが正常に動作していることになる  
※この情報は世界に公開されてしまっているので確認出来たらすぐに削除することをおすすめする  
 削除コマンド　rm  /var/www/html/phpinfo.php  
  
5⃣PHPのブラウザを更新して削除されていることを確認する  

もっと簡単なPHP動作確認方法  
[ec2-user@ip-172-31-33-39 ~]$ echo "<?php echo 'Hello World'; ?>" > /var/www/html/index.php ←indexファイルをドキュメントルートに作成しつつ、その中に単純な文字列を表示させる関数を記述する  
[ec2-user@ip-172-31-33-39 ~]$　←何も表示されなければOK  
[ec2-user@ip-172-31-33-39 ~]$ cat /var/www/html/index.php　←catコマンドでも中身を確認できる  
<?php echo 'Hello World'; ?>  
※ブラウザでアドレスの後に/index.phpで実際の表示も確認できる  
  
SQLのインストール、動作確認（MySQLの互換であるMariaDBをインストール）  
1⃣インストール  
[ec2-user@ip-172-31-33-39 ~]$ sudo dnf install mariadb105-server -y  
Installed:  
  mariadb-connector-c-3.1.13-1.amzn2023.0.3.x86_64                                                     
  mariadb-connector-c-config-3.1.13-1.amzn2023.0.3.noarch                                              
  mariadb105-3:10.5.25-1.amzn2023.0.1.x86_64                                                           
  mariadb105-backup-3:10.5.25-1.amzn2023.0.1.x86_64                                                    
  mariadb105-common-3:10.5.25-1.amzn2023.0.1.x86_64                                                    
  mariadb105-cracklib-password-check-3:10.5.25-1.amzn2023.0.1.x86_64                                   
  mariadb105-errmsg-3:10.5.25-1.amzn2023.0.1.x86_64                                                    
  mariadb105-gssapi-server-3:10.5.25-1.amzn2023.0.1.x86_64                                             
  mariadb105-server-3:10.5.25-1.amzn2023.0.1.x86_64                                                    
  mariadb105-server-utils-3:10.5.25-1.amzn2023.0.1.x86_64                                              
  mysql-selinux-1.0.4-2.amzn2023.0.3.noarch                                                            
  perl-DBD-MariaDB-1.22-1.amzn2023.0.4.x86_64                                                          
  perl-DBI-1.643-7.amzn2023.0.3.x86_64                                                                 
  perl-FileHandle-2.03-477.amzn2023.0.6.noarch                                                         
  perl-Math-BigInt-1:1.9998.39-2.amzn2023.0.2.noarch                                                   
  perl-Math-BigRat-0.2614-458.amzn2023.0.2.noarch                                                      
  perl-Math-Complex-1.59-477.amzn2023.0.6.noarch                                                       
  perl-Sys-Hostname-1.23-477.amzn2023.0.6.x86_64                                                       
  perl-base-2.27-477.amzn2023.0.6.noarch                                                               

Complete!  
上記のように　Complete!　がでればOK  
  
2⃣DBサーバを起動、初期設定をする  
[ec2-user@ip-172-31-33-39 ~]$ sudo systemctl start mariadb  
[ec2-user@ip-172-31-33-39 ~]$  ←何もでなければOK  
[ec2-user@ip-172-31-33-39 ~]$ sudo mysql_secure_installation ←対話形式でパスワード設定をするコマンド  
  
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB  
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!  
  
In order to log into MariaDB to secure it, we'll need the current  
password for the root user. If you've just installed MariaDB, and  
haven't set the root password yet, you should just press enter here.  
  
Enter current password for root (enter for none):  ←現在のMariaDBのルートパスワードを入力する(デフォはルートパスワードは設定されていないので入力せずにEnterキー)  

OK, successfully used password, moving on...  
  
Setting the root password or using the unix_socket ensures that nobody  
can log into the MariaDB root user without the proper authorisation.  
  
You already have your root account protected, so you can safely answer 'n'.  
  
Switch to unix_socket authentication [Y/n]  Y　←Unixソケット認証に切り替えるかどうかを設定  
  
Enabled successfully!  
Reloading privilege tables..  
 ... Success!  
  
  
You already have your root account protected, so you can safely answer 'n'.  
  
Change the root password? [Y/n] ←MariaDBのルートパスワードを変更するか設定  
New password:  ←パスワードを設定  
Re-enter new password: 　←再度設定したパスワードを入力  
Password updated successfully!  
Reloading privilege tables..  
 ... Success!  
  
  
By default, a MariaDB installation has an anonymous user, allowing anyone  
to log into MariaDB without having to have a user account created for  
them.  This is intended only for testing, and to make the installation  
go a bit smoother.  You should remove them before moving into a  
production environment.  
  
Remove anonymous users? [Y/n] Y ←匿名ユーザアカウントを削除する設定  
 ... Success!  
  
Normally, root should only be allowed to connect from 'localhost'.  This  
ensures that someone cannot guess at the root password from the network.  
  
Disallow root login remotely? [Y/n] Y ←リモートルートログインを無効にする設定  
 ... Success!  

By default, MariaDB comes with a database named 'test' that anyone can  
access.  This is also intended only for testing, and should be removed  
before moving into a production environment.  
  
Remove test database and access to it? [Y/n] Y ←テスト用のデータベースを削除する設定  
 - Dropping test database...  
 ... Success!  
 - Removing privileges on test database...  
 ... Success!  
  
Reloading the privilege tables will ensure that all changes made so far  
will take effect immediately.  
  
Reload privilege tables now? [Y/n] Y　←権限テーブルを再ロードし変更を保存する  
  
 ... Success!  
  
Cleaning up...  
  
All done!  If you've completed all of the above steps, your MariaDB ←All doneの記載があればOK  
installation should now be secure.  
  
Thanks for using MariaDB!  
[ec2-user@ip-172-31-33-39 ~]$  
  
2⃣MariaDBの状態を確認する  
[ec2-user@ip-172-31-33-39 ~]$ sudo systemctl status mariadb  
● mariadb.service - MariaDB 10.5 database server  
     Loaded: loaded (/usr/lib/systemd/system/mariadb.service; disabled; preset: disabled)  
     Active: active (running) since Tue 2025-04-08 18:06:37 UTC; 22min ago ←active (running)と出ていればOK　qキーで終了  
     
3⃣システムブート(起動)するたびにMariaDBが自動起動するようにする  
[ec2-user@ip-172-31-33-39 ~]$ sudo systemctl enable mariadb  
Created symlink /etc/systemd/system/mysql.service → /usr/lib/systemd/system/mariadb.service.    
Created symlink /etc/systemd/system/mysqld.service → /usr/lib/systemd/system/mariadb.service.  
Created symlink /etc/systemd/system/multi-user.target.wants/mariadb.service → /usr/lib/systemd/system/mariadb.service.  
[ec2-user@ip-172-31-33-39 ~]$ exit ←一度ログアウトしてsystemctl statusで起動しているか確認  
  
phpMyAdminのインストール  
1⃣sudo dnf install php-mbstring php-xml -y　コマンドでインストール  
  
[ec2-user@ip-172-31-33-39 ~]$ sudo dnf install php-mbstring php-xml -y  
Last metadata expiration check: 0:01:31 ago on Sat May 24 13:05:55 2025.  
Package php8.3-mbstring-8.3.16-1.amzn2023.0.1.x86_64 is already installed.  
Package php8.3-xml-8.3.16-1.amzn2023.0.1.x86_64 is already installed.  
Dependencies resolved.  
Nothing to do.  
Complete!　←コンプリートが出ればOK  
  
2⃣反映させるためにsystemctlコマンドで再起動（2種類の再起動実施）  
[ec2-user@ip-172-31-33-39 ~]$ sudo systemctl restart httpd  
[ec2-user@ip-172-31-33-39 ~]$ sudo systemctl restart php-fpm  
  
3⃣phpmyAdminをダウンロードする  
[ec2-user@ip-172-31-33-39 ~]$ cd /var/www/html/  
※事前にダウンロード先のディレクトリに移動しておく  
```
[ec2-user@ip-172-31-33-39 html]$ wget https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz
--2025-05-24 13:25:12--  https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz
Resolving www.phpmyadmin.net (www.phpmyadmin.net)... 95.173.204.22, 178.249.213.26, 178.249.213.3, ...
Connecting to www.phpmyadmin.net (www.phpmyadmin.net)|95.173.204.22|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://files.phpmyadmin.net/phpMyAdmin/5.2.2/phpMyAdmin-5.2.2-all-languages.tar.gz [following]
--2025-05-24 13:25:13--  https://files.phpmyadmin.net/phpMyAdmin/5.2.2/phpMyAdmin-5.2.2-all-languages.tar.gz
Resolving files.phpmyadmin.net (files.phpmyadmin.net)... 178.249.213.26, 95.173.204.15, 95.173.204.25, ...
Connecting to files.phpmyadmin.net (files.phpmyadmin.net)|178.249.213.26|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13764534 (13M) [application/octet-stream]
Saving to: ‘phpMyAdmin-latest-all-languages.tar.gz’

phpMyAdmin-latest-all-languages.tar.gz               100%[=====================================================================================================================>]  13.13M  27.3MB/s    in 0.5s    

2025-05-24 13:25:13 (27.3 MB/s) - ‘phpMyAdmin-latest-all-languages.tar.gz’ saved [13764534/13764534]
```
  
4⃣下記のコマンドでphpAdminファイルを解凍する  
mkdir phpMyAdmin && tar -xvzf phpMyAdmin-latest-all-languages.tar.gz -C phpMyAdmin --strip-components 1  
意味：phpmyAdminというディレクトリを作成し、その中に圧縮ファイル「phpMyAdmin-latest-all-languages.tar.gz」を入れて、解凍するという意味  
  
5⃣ダウンロードしたパッケージを削除しておく  
[ec2-user@ip-172-31-33-39 html]$ rm phpMyAdmin-latest-all-languages.tar.gz　←何も表示されなければOK  
  
4⃣phpMyAdminの中身を確認しておく（下記のようになっていればOK）  
```
[ec2-user@ip-172-31-33-39 html]$ ls phpMyAdmin/
CONTRIBUTING.md  LICENSE  RELEASE-DATE-5.2.2  composer.json  config.sample.inc.php  examples     index.php  libraries  package.json  setup                   sql        themes   vendor
ChangeLog        README   babel.config.json   composer.lock  doc                    favicon.ico  js         locale     robots.txt    show_config_errors.php  templates  url.php  yarn.lock
```
