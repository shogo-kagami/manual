## モデルのアップロード（Prd環境）(暫定版) 

### L3ルームでVSOC 接続　
1. 作業端末の電源 ON
   - L3 ルーム入室し、作業端末の電源を入れる
2. 作業端末にログイン
   - GUIで作業端末にログインする
3. VSOC環境にログイン
   - デスクトップ左上の「リモートデスクトップ接続」のアイコンをクリックし、VCOSにログインする
		    
### モデルのアップロード
1. login-node にログイン
   - Git Bash を起動
2. PROXY 設定
   - 以下のコマンドラインを実行する  
	```
 	export HTTPS_PROXY=http://10.0.20.11:8080
	export https_proxy=http://10.0.20.11:8080
	export AWS_PROFILE=AWSAdministratorAccess-050451382758
 	```
3. gitの削除(※ある場合)
   - 以下のコマンドラインを実行する  
	`rm -rf reimei_RAG/terraform/environments_ray/prod`
5. gitCloneを実施
   - 以下のコマンドラインを実行する    
   	`cd reimei_RAG/terraform/environments_ray/prod`  
   	`git clone https://github.com/sbintuitions/reimei_RAG.git`
4. SSO設定
    - 以下のコマンドラインを実行する  
      `$aws configure sso`
1. Service_Prod のアカウントを選択する

7. モデルの確認
    - 以下のコマンドラインを実行する
   	`ls -l`
9. コピー作業
    - 以下のコマンドラインを実行する  
      `$cp -a /store/share/sbint_models/finetuned/release/sbint-2025-07-14/sbint-2025-07-14-70b-sft/ /lustre/share/sbint_models/finetuned/release/`
10. checksum 確認
    - 事前に共有された checksum と比較し、同じであることを確認する
      `$find /lustre/share/sbint_models/finetuned/release/sbint-2025-07-14/sbint-2025-07-14-70b-sft/ -type f -exec md5sum {} \; | sort -k 2 | md5sum`
11. ログアウト
12. AWSからサインアウトし、ブラウザを閉じる(VSOC)
