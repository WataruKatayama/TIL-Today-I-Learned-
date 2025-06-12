# 最初の環境構築  
### WebサーバとAPIサーバの二つの部分に分け、サーバはそれぞれ、Amazon VPC内に構築したPublic Subnetの中に、Amazon EC2を用いて構築する  
  
※Webサーバ：ブラウザ上で実行されるWebアプリケーションで、必要な情報をAPIサーバから取得し、ユーザーに表示させる  
　APIサーバ：Webサーバからのリクエストに基づき適切な処理を行い、レスポンスを返す  

  
## VPCの作成  
　● 新規VPCとして、reservation-vpcを作成する  
　● CIDRの指定は10.0.0.0/21とする  
 <手順>  
 1.ダッシュボードから「お使いのVPC」を選択し、右上「VPCを作成」を選択  
 2.作成するリソース「VPCのみ」、名前タグ、オプション「reservation-vpc」、IPv4CIDRブロック「IPv4CIDRの手動入力」、IPv4CIDR「10.0.0.0/21」、テナンシー「デフォルト」、タグ「Name」「reservation-vpc」に設定し「VPCを作成」をクリック  
 3.正常にVPCが作成されれば完了  

 ## web-subnet-01を作成  
● Webサーバを配置するためのサブネットとして、web-subnet-01を作成する  
● CIDRの指定は10.0.0.0/24とする  
● ルートテーブルとしてweb-routetableを作成し、web-subnet-01に関連付けする  
<手順 サブネットの作成>  
1.ダッシュボードから「サブネット」を選択し、右上「サブネットを作成」を選択  
2.VPCID「reservation-vpc（作成したVPC名）」、サブネット名「web-subnet-01」、アベイラビリティゾーン「ap-nothest-1a」、IPv4 VPC CIDRブロック「そのまま」、IPv4サブネットCIDRブロック「10.0.0.0/24」、タグ「Name」「web-subnet-01」に設定し「サブネットを作成」をクリック  
3.正常にweb-subnet-01が作成されれば完了  
  
<手順　ルートテーブルの作成>  
1.
