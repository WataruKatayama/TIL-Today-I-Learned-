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
～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～  
chmod:パーミッション変更コマンド  
2775:権限の前に2を設定すると、ディレクトリ内で新規作成されたファイルやディレクトリは元のディレクトリと同じグループ所有権を持つようになる  
-exec:findで検索したファイルに対してコマンドを実行するオプション(ここではsudo chmod 2775)  
{}:findコマンドで検索された各ディレクトリを対象にするという意味  
～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～  
  
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
