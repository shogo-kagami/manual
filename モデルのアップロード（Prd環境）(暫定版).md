## L3ルームでVSOC 接続
1. 作業端末の電源 ON
   - L3 ルーム入室し、作業端末の電源を入れる
1. 作業端末にログイン
   - GUIで作業端末にログインする
1. VSOC環境にログイン
   - デスクトップ左上の「リモートデスクトップ接続」のアイコンをクリックし、VCOSにログインする

## モデルのアップロード
1. login-node にログイン
	- Git Bash を起動する
1. PROXY 設定
	- 以下のコマンドラインを実行する  
	```
 	export HTTPS_PROXY=http://10.0.20.11:8080
	export https_proxy=http://10.0.20.11:8080
	export AWS_PROFILE=AWSAdministratorAccess-050451382758
 	```
1. ブラウザでAWSにログイン(VSOC)
   - VSOC デスクトップ上の chrome のアイコンをクリックし、ブラウザを起動し  
     以下のURL にアクセスする  
     `https://d-95675fea1b.awsapps.com/start/#`  
     > prod アカウントでログインすること
1. gitの削除(※gitがある場合)
   - 以下のコマンドラインを実行する  
	```
	rm -rf reimei_RAG/terraform/environments_ray/prod
 	```
1. git cloneを実施
   - 以下のコマンドラインを実行する    
	```
	cd reimei_RAG/terraform/environments_ray/prod
	git clone https://github.com/sbintuitions/reimei_RAG.git
 	```
1. SSO設定
    - 以下のコマンドラインを実行する  
      `aws configure sso`
		> L3ルームにてデータ取り込みを行う場合、VSOCアカウントを選択する

1. Service_Prod のアカウントを選択する
   		> config 確認:`cat ~/.aws/config`
1. SSO 認証
   - Service_Prod のアカウントで SSO 認証を行う  
     `aws sso login`
1. モデルの確認
    - 以下のコマンドラインを実行する  
   	`ls -l`
1. コピー作業
    - 以下のコマンドラインを実行する  
   	`cp -a /store/share/sbint_models/finetuned/release/sbint-YYYY-MM-DD/sbint-YYYY-MM-DD-70b-sft/ /lustre/share/sbint_models/finetuned/release/`
1. checksum 確認
    - 事前に共有された checksum と比較し、同じであることを確認する  
      `find /lustre/share/sbint_models/finetuned/release/sbint-YYYY-MM-DD/sbint-YYYY-MM-DD-70b-sft/ -type f -exec md5sum {} \; | sort -k 2 | md5sum`
1. terraform 初期化
   - 以下のコマンドラインを実行する  
     `terraform init`
1. 変更内容のプレビュー
   - 以下のコマンドラインを実行する  
     `terraform plan`
1. ログアウト  
   		`exit`
1. AWSからサインアウトし、ブラウザを閉じる(VSOC)
