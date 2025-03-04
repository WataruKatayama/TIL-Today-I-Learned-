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
