# モデルのアップロード（Prd環境）(暫定版) 
### データ持ち込み
- 事前に申請してAWS CLIで以下のコマンドラインを実行する  
`aws configure sso`
```
SSO start URL [None]: https://d-95675fea1b.awsapps.com/start
SSO region [None]: ap-northeast-1
SSO registration scopes [None]: sso:account:access
Default client Region [None]: ap-northeast-1
CLI default output format (json if not specified) [None]: json
Profile name [123456789011_ReadOnly]: {任意のprofile名を入力}
```
- 以下を実行すると、この後aws cli を実行する際に --profileを指定する必要がなくなる  
	`export AWS_PROFILE={任意のprofile名を入力}`

- 以下コマンドラインで中身を確認できる  
	`aws s3 ls s3://arn:aws:s3:ap-northeast-1:060795933614:accesspoint/l2-upload-9coxiz/250723_shogo_kagami_9coxiz/`

- cp コマンドでコピーする
  	`aws s3 cp [コピー元(~/Downloads/)] [コピー先(s3://arn:aws:s3:ap-northeast-1:060795933614:accesspoint/l2-upload...)]`

### L3ルームでVSOC 接続
1. 作業端末の電源 ON
   - L3 ルーム入室し、作業端末の電源を入れる
1. 作業端末にログイン
   - GUIで作業端末にログインする
1. VSOC環境にログイン
   - デスクトップ左上の「リモートデスクトップ接続」のアイコンをクリックし、VCOSにログインする

### モデルのアップロード
1. login-node にログイン
	- Git Bash を起動する
1. PROXY 設定
	- 以下のコマンドラインを実行する  
	```
 	export HTTPS_PROXY=http://10.0.20.11:8080
	export https_proxy=http://10.0.20.11:8080
	export AWS_PROFILE=AWSAdministratorAccess-050451382758
 	```
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

 	> s3://arn:aws:s3:ap-northeast-1:060795933614:accesspoint/l4-download-9coxiz/250723_shogo_kagami_9coxiz/

1. Service_Prod のアカウントを選択する  

1. SSO 認証
   - Service_Prod のアカウントで SSO 認証を行う  
     `aws sso login`
1. モデルの確認
    - 以下のコマンドラインを実行する  
   	`ls -l`
1. コピー作業
    - 以下のコマンドラインを実行する  
   	`cp -a /store/share/sbint_models/finetuned/release/sbint-****-**-**/sbint-****-**-**-70b-sft/ /lustre/share/sbint_models/finetuned/release/`
1. checksum 確認
    - 事前に共有された checksum と比較し、同じであることを確認する  
      `find /lustre/share/sbint_models/finetuned/release/sbint-2025-07-14/sbint-2025-07-14-70b-sft/ -type f -exec md5sum {} \; | sort -k 2 | md5sum`
1. terraform 初期化
   - 以下のコマンドラインを実行する  
     `terraform init`
1. 変更内容のプレビュー
   - 以下のコマンドラインを実行する  
     `terraform plan`
1. ログアウト
1. AWSからサインアウトし、ブラウザを閉じる(VSOC)
