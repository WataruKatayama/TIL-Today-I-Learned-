[ec2-user@ip-172-31-15-116 ~]$ pwd  
/home/ec2-user  
[ec2-user@ip-172-31-15-116 ~]$ mkdir original_dir  
[ec2-user@ip-172-31-15-116 ~]$ ls  
original_dir  
[ec2-user@ip-172-31-15-116 ~]$ cp -r original_dir/ copied_dir　←　copied_dirを作ってその中にoriginal_dir/の中身をコピーするイメージ  
[ec2-user@ip-172-31-15-116 ~]$ ls  
copied_dir  original_dir  
[ec2-user@ip-172-31-15-116 ~]$ rm original_dir/  
rm: 'original_dir/' を削除できません: ディレクトリです　←　-rオプションがないとディレクトリは削除できない  
[ec2-user@ip-172-31-15-116 ~]$ rm -r original_dir/  
[ec2-user@ip-172-31-15-116 ~]$ rm -r copied_dir/  


ディレクトリとファイルの区別  
[ec2-user@ip-172-31-15-116 ~]$ touch myfile ←ファイルの作成  
[ec2-user@ip-172-31-15-116 ~]$ ls  
myfile  
[ec2-user@ip-172-31-15-116 ~]$ mkdir mydir ←ディレクトリの作成  
[ec2-user@ip-172-31-15-116 ~]$ ls  
mydir  myfile  
[ec2-user@ip-172-31-15-116 ~]$ ls -l　←詳細を見るオプション  
合計 0  
drwxr-xr-x. 2 ec2-user ec2-user 6  3月  5 03:37 mydir ← パーミッションアクセス権の先頭にdがあればディレクトリ 
-rw-r--r--. 1 ec2-user ec2-user 0  3月  5 03:37 myfile  
  
[ec2-user@ip-172-31-15-116 ~]$ touch original.txt  
[ec2-user@ip-172-31-15-116 ~]$ cp original.txt cpied.txt  
[ec2-user@ip-172-31-15-116 ~]$ ls  
cpied.txt  original.txt  
[ec2-user@ip-172-31-15-116 ~]$ rm cpied.txt original.txt ←スペース区切りで複数削除可能   
[ec2-user@ip-172-31-15-116 ~]$ ls  
[ec2-user@ip-172-31-15-116 ~]$   


[ec2-user@ip-172-31-15-116 ~]$ touch sample.txt  
[ec2-user@ip-172-31-15-116 ~]$ ls  
sample.txt  
[ec2-user@ip-172-31-15-116 ~]$ mv sample.txt renamed.txt ←mvにはリネームの機能もある  
[ec2-user@ip-172-31-15-116 ~]$ ls  
renamed.txt  
[ec2-user@ip-172-31-15-116 ~]$ mkdir mydir  
[ec2-user@ip-172-31-15-116 ~]$ ls  
mydir  renamed.txt  
[ec2-user@ip-172-31-15-116 ~]$ mv renamed.txt mydir/renamed2.txt　←移動のついでにリネームしても問題ない  
[ec2-user@ip-172-31-15-116 ~]$ ls  
mydir  
[ec2-user@ip-172-31-15-116 ~]$ ls mydir/  
renamed2.txt  

  less コマンドの機能  
[ec2-user@ip-172-31-15-116 ~]$ less sample.txt  
  
・/検索ワード　でvimのように検索できる  
  
・:qでlessコマンドを終了できる。  
  
  
ユーザーの追加  
[ec2-user@ip-172-31-15-116 ~]$ sudo useradd katayama ←sudo 管理者権限として実行する　useradd ユーザを追加する  
[ec2-user@ip-172-31-15-116 ~]$ cat /etc/passwd　←　ユーザのアカウント情報を記録しているパス  
root:x:0:0:root:/root:/bin/bash  
bin:x:1:1:bin:/bin:/sbin/nologin  
daemon:x:2:2:daemon:/sbin:/sbin/nologin  
adm:x:3:4:adm:/var/adm:/sbin/nologin  
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin  
sync:x:5:0:sync:/sbin:/bin/sync  
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown  
halt:x:7:0:halt:/sbin:/sbin/halt  
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin  
operator:x:11:0:operator:/root:/sbin/nologin  
games:x:12:100:games:/usr/games:/sbin/nologin  
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin  
nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin  
dbus:x:81:81:System message bus:/:/sbin/nologin  
systemd-network:x:192:192:systemd Network Management:/:/usr/sbin/nologin  
systemd-oom:x:999:999:systemd Userspace OOM Killer:/:/usr/sbin/nologin  
systemd-resolve:x:193:193:systemd Resolver:/:/usr/sbin/nologin  
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/sbin/nologin  
rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/sbin/nologin  
libstoragemgmt:x:997:997:daemon account for libstoragemgmt:/:/usr/sbin/nologin  
systemd-coredump:x:996:996:systemd Core Dumper:/:/usr/sbin/nologin  
systemd-timesync:x:995:995:systemd Time Synchronization:/:/usr/sbin/nologin  
chrony:x:994:994:chrony system user:/var/lib/chrony:/sbin/nologin  
ec2-instance-connect:x:993:993::/home/ec2-instance-connect:/sbin/nologin  
stapunpriv:x:159:159:systemtap unprivileged user:/var/lib/stapunpriv:/sbin/nologin  
rpcuser:x:29:29:RPC Service User:/var/lib/nfs:/sbin/nologin  
tcpdump:x:72:72::/:/sbin/nologin  
ec2-user:x:1000:1000:EC2 Default User:/home/ec2-user:/bin/bash  
katayama:x:1001:1001::/home/katayama:/bin/bash ←　最後の行に追加設定したユーザ名が記載されている  
  
  
[ec2-user@ip-172-31-15-116 ~]$ ls /home/ ←ユーザを追加するとホームディレクトリもユーザー用のものも追加される
ec2-user  katayama
