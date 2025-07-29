## L3ルームでVSOC 接続
1. 作業端末の電源 ON
   - L3 ルーム入室し、作業端末の電源を入れる
1. 作業端末にログイン
   - GUIで作業端末にログインする
1. VSOC環境にログイン
   - デスクトップ左上の「リモートデスクトップ接続」のアイコンをクリックし、VSOCにログインする

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
1. ブラウザでAWSにログイン
   - VSOC デスクトップ上の chrome のアイコンをクリックし、ブラウザを起動し  
     以下のURL にアクセスする  
     `https://d-95675fea1b.awsapps.com/start/#`  
     > prod アカウントでログインすること

1. SSO設定（Service_Prod アカウント）
    - 以下のコマンドラインを実行する  
      `aws configure sso`  
	```
	SSO start URL [None]: https://d-95675fea1b.awsapps.com/start/#
	SSO region [None]: ap-northeast-1
	SSO registration scopes [None]: sso:account:access
	Default client Region [None]: ap-northeast-1
	CLI default output format (json if not specified) [None]: json
	Profile name [123456789011_ReadOnly]: prod
	```
	- configの確認  
	`cat ~/.aws/config`

1. 以下を実行すると、この後AWS CLIを実行する際に --profileを指定する必要がなくなる  
	`export AWS_PROFILE=VSOC`

1. SSO 認証
   - Service_Prod のアカウントで SSO 認証を行う  
     `aws sso login`
     
1. gitの削除(※gitがある場合)
   - 以下のコマンドラインを実行する  
	```
	rm -rf reimei_RAG/terraform/environments_ray/prod
 	```
1. Gitのリポジトリを複製する
   - 以下のコマンドラインを実行する    
	```
	cd reimei_RAG/terraform/environments_ray/prod
	git clone https://github.com/sbintuitions/reimei_RAG.git
 	```

### モデルのアップロード
1. ファイル存在確認
   - モデルの確認  
   	`ls -l /store/share/~`  
	`ls -l /lustre/share/~`

1. コピー作業
    - 以下のコマンドラインを実行する  
   	`cp -a /store/share/sbint_models/finetuned/release/sbint-YYYY-MM-DD/sbint-YYYY-MM-DD-70b-sft/ /lustre/share/sbint_models/finetuned/release/`
1. checksum 確認
    - 事前に共有された checksum と比較し、同じであることを確認する  
      `find /lustre/share/sbint_models/finetuned/release/sbint-YYYY-MM-DD/sbint-YYYY-MM-DD-70b-sft/ -type f -exec md5sum {} \; | sort -k 2 | md5sum`

1. ログアウト  
   		`exit`
1. AWSからサインアウトし、ブラウザを閉じる(VSOC)
