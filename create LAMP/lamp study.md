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
  
  
