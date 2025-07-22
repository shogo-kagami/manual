# manual

L3ルームでVSOC 接続　

1	作業端末の電源 ON　　L3 ルーム入室し、作業端末の電源を入れる

2	作業端末にログイン　　GUIで作業端末にログインする
　　　　　　　　　　　　　　　　　user SBINT
		 
3	VSOC環境にログイン　　デスクトップ左上の「リモートデスクトップ接続」のアイコンをクリックし、VCOSにログインする
　　　　　　　　　　　　　　　　　接続先　10.0.50.101
		 
        　　　　　　　　　　　　user　prod.XXXX.XXXX@l4vsoc.sbint.local

      
		    
モデルのアップロード
1	login-node にログイン　　　Putty などのターミナルソフトを起動し、ログインノードに接続する

　　　　　　　　　　　　　　　　  接続先　10.120.169.2
		  
                　　　　　　　 user　userXXXXX　
			

2	PROXY 設定	以下のコマンドラインを実行する

　　　　　　　　　　　　　　　　$export HTTPS_PROXY=http://10.0.20.11:8080
		
　　　　　　　　　　　　　　　　$export https_proxy=http://10.0.20.11:8080
		
　　　　　　　　　　　　　　　　$export AWS_PROFILE=AWSAdministratorAccess-050451382758
		

3	aws コマンド存在確認　　　aws コマンドの存在確認

                           $aws --version
			   

4	ブラウザでAWSにログイン	VSOC デスクトップ上の chrome のアイコンをクリックし、ブラウザを起動し
　　　　　　　　　　　　　　　　　　　　以下のURL にアクセスする
		    
                          　　　　https://d-95675fea1b.awsapps.com/start/#
			      
　　　　　　　　　　　　　　　　　　　　prod でないアカウントでログインする
		    
　　　　　　　　　　　　　　　　　　　　prod アカウントでログインすると、次の aws configure sso で
		    
　　　　　　　　　　　　　　　　　　　　RD_Dev ( 730335521439 ) アカウントが選択できなくなる 
		    

5	SSO設定 (RD_Devアカウント)	　RD_Dev アカウントについて SSO 設定を行う

　　　　　　　　　　　　　　　　　$aws configure sso --no-browser --use-device-code

6	config 確認		~/.aws/config を確認する

　　　　　　　　　　　　　　　$cat ~/.aws/config
	       

7	SSO 認証			RD_Devアカウントで SSO 認証を行う

　　　　　　　　　　　　　　　$aws sso login --no-browser --use-device-code --profile AWSAdministratorAccess-730335521439
	       

8	ダウンロード		モデルのダウンロードを行う

                         $OBJ_KEY="share/sbint_models/reranker/sarashina2.2-0.5b-reranker-toy-0523"
			 
                         $aws s3 sync --profile AWSAdministratorAccess-730335521439 "s3://arn:aws:s3:ap-northeast-1:730335521439:accesspoint/reimei-engine-dev-hf-models-ap/$OBJ_KEY"  "/lustre/$OBJ_KEY"
			 

 コピー作業                 コピー元: /store/share/sbint_models/finetuned/release/sbint-2025-07-14/sbint-2025-07-14-70b-sft/
 　　　　　　　　　　　　　　　コピー先: /lustre/share/sbint_models/finetuned/release/sbint-2025-07-14/sbint-2025-07-14-70b-sft/
                
9	checksum 確認						事前に共有された checksum と比較し、同じであることを確認する
                         $find /lustre/share/sbint_models/finetuned/release/sbint-2025-07-14/sbint-2025-07-14-70b-sft/ -type f -exec md5sum {} \; | sort -k 2 | md5sum

10	ログアウト									

11	AWSからサインアウトし、									
	ブラウザを閉じる									
	(VSOC)									

                         
                 




                      
                      
