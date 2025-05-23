「Teratermで鍵ファイルをアップロードする方法」  
  
1.EC2インスタンスへTera Termでログインする  
　⓵Tera Termを起動  
　⓶「ホスト」欄にEC2のパブリックIPアドレスを入力（例：13.115.xx.xx）  
　⓷「サービス」で SSH を選択し「OK」  
　⓸「ユーザー名」は ec2-user（Amazon Linuxの場合）  
　⓹「RSA/DSA/ECDSA/ED25519を使う」にチェック  
　⓺.pem ファイルを指定してログイン  
  
2. ファイル転送（SCP）機能を使う  
ログイン後、次の手順でローカルからEC2インスタンスへファイルをアップロードする。  
　⓵Tera Termの画面上部メニューから  
　「ファイル → SSH SCP... 」の順でクリック  
　⓶「From」欄の ... をクリックし、アップロードしたいファイル（例：your-key.pem）を選択  
　⓷「To」欄にアップロード先のパスを指定（例：/tmp）  
　⓸「送信」ボタンをクリック  
  
3.確認する  
　⓵ アップロード先のディレクトリへ移動し、ls -l pemファイル名で検索する。  
  
「ssh -iコマンドを使用してログインしようとしてもエラーが出たとき」  
  
bash```$ ssh -i test-ec2-key.pem ec2-user@10.0.2.105
The authenticity of host '10.0.2.105 (10.0.2.105)' can't be established.
ED25519 key fingerprint is SHA256:9WSUF+/hbvsQ3k6EI9G7PPpg+/zfH41NMftwn3s5ePM.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.0.2.105' (ED25519) to the list of known hosts.
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for 'test-ec2-key.pem' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "test-ec2-key.pem": bad permissions
ec2-user@10.0.2.105: Permission denied (publickey,gssapi-keyex,gssapi-with-mic)
```
