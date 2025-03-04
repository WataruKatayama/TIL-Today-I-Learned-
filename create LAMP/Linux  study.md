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
