# manual

1	L3ルームでVSOC 接続																																																												
	1	作業端末の電源 ON										-				-			-		L3 ルーム入室し、作業端末の電源を入れる																					エラーなき事																			
																																																													
																																																													
	2	作業端末にログイン										L3端末				-			-		GUIで作業端末にログインする																					デスクトップ画面が表示されること																			
																																																													
																						user			SBINT																																				
																						passwd			SBINT123																																				
																																																													
	3	VSOC環境にログイン										L3端末				SBINT			-		デスクトップ左上の「リモートデスクトップ接続」のアイコンをクリックし、																					デスクトップ画面が表示されること																			
																					VCOSにログインする																																								
																																																													
																						接続先			10.0.50.101																																				
																						user			prod.XXXX.XXXX@l4vsoc.sbint.local																																				
																						passwd																																							
																																																													
																																																													
2	モデルのアップロード																																																												
	1	login-node にログイン										L3端末				SBINT			-		Putty などのターミナルソフトを起動し、ログインノードに接続する																					プロンプトが表示されること																			
																																																													
																						接続先			10.120.169.2																																				
																						user			userXXXXX																																				
																						passwd																																							
																																																													
	2	PROXY 設定										login-node				個人					以下のコマンドラインを実行する																																								
																				$	export HTTPS_PROXY=http://10.0.20.11:8080																																								
																				$	export https_proxy=http://10.0.20.11:8080																																								
																				$	export AWS_PROFILE=AWSAdministratorAccess-050451382758																																								
																																																													
	3	aws コマンド存在確認										login-node				個人					aws コマンドの存在確認																					バージョンが表示されること																			
																				$	aws --version																																								
																																										存在しなかった場合には、インストールする																			
																																										curl -x ${HTTPS_PROXY} -L "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"																			
																																										unzip awscliv2.zip																			
																																										rm awscliv2.zip																			
																																										./aws/install -i ${HOME}/.local/aws-cli -b ${HOME}/.local/bin																			
																																										source .profile																			
																																										aws --version 																			
																																																													
																																																													
	4	ブラウザでAWSにログイン										L3端末				SBINT			-		VSOC デスクトップ上の chrome のアイコンをクリックし、ブラウザを起動し																					RD_Dev にログインできること																			
		(VSOC)																			以下のURL にアクセスする																																								
																																																													
																						https://d-95675fea1b.awsapps.com/start/#																																							
																																																													
																						prod でないアカウントでログインする																																							
																						prod アカウントでログインすると、次の aws configure sso で																																							
																						RD_Dev ( 730335521439 ) アカウントが選択できなくなる																																							
																																																													
																																																													
	5	SSO設定 (RD_Devアカウント)										login-node				個人					RD_Dev アカウントについて SSO 設定を行う																																								
																				$	aws configure sso --no-browser --use-device-code																					以下のように入力する																			
																																																													
																																										SSO session name (Recommended): upload_task													( ※ 任意の文字列 )						
																																										SSO start URL [None]: https://d-95675fea1b.awsapps.com/start/#																			
																																										SSO region [None]: ap-northeast-1																			
																																										SSO registration scopes [sso:account:access]:														(何も入力しない)					
																																										Browser will not be automatically opened.																			
																																										Please visit the following URL:																			
																																																													
																																										https://d-95675fea1b.awsapps.com/start/#/device																			
																																																													
																																										Then enter the code:																			
																																																													
																																										BZRW-BSPH																			
																																																													
																																										Alternatively, you may visit the following URL which will autofill the code upon loading:																			
																																										https://d-95675fea1b.awsapps.com/start/#/device?user_code=BZRW-BSPH																			
																																										There are 5 AWS accounts available to you.																			
																																										Using the account ID 730335521439											730335521439 を選択すること！								
																																										The only role available to you is: AWSAdministratorAccess																			
																																										Using the role name "AWSAdministratorAccess"																			
																																										Default client Region [None]: ap-northeast-1																			
																																										CLI default output format (json if not specified) [None]: json																			
																																										Profile name [AWSAdministratorAccess-730335521439]:																			
																																										To use this profile, specify the profile name using --profile, as shown:																			
																																																													
																																										aws sts get-caller-identity --profile AWSAdministratorAccess-730335521439																			
																																																													
																																																													
																																																													
	6	config 確認										login-node				個人					~/.aws/config を確認する																					以下の様に表示されること																			
																				$	cat ~/.aws/config																																								
																																										[profile AWSAdministratorAccess-730335521439]																			
																																										sso_session = upload_task																			
																																										sso_account_id = 730335521439																			
																																										sso_role_name = AWSAdministratorAccess																			
																																										region = ap-northeast-1																			
																																										output = json																			
																																										[sso-session upload_task]																			
																																										sso_start_url = https://d-95675fea1b.awsapps.com/start/#																			
																																										sso_region = ap-northeast-1																			
																																										sso_registration_scopes = sso:account:access																			
																																																													
																																																													
	7	SSO 認証										login-node				個人					RD_Devアカウントで SSO 認証を行う																																								
																				$	aws sso login --no-browser --use-device-code --profile AWSAdministratorAccess-730335521439																																								
																																																													
																																																													
	8	ダウンロード										login-node				個人					モデルのダウンロードを行う																					正常終了すること																			
																				$	OBJ_KEY="share/sbint_models/reranker/sarashina2.2-0.5b-reranker-toy-0523"																																								
																				$	aws s3 sync --profile AWSAdministratorAccess-730335521439 "s3://arn:aws:s3:ap-northeast-1:730335521439:accesspoint/reimei-engine-dev-hf-models-ap/$OBJ_KEY"  "/lustre/$OBJ_KEY"																																								
																																																													
																																																													
	9	checksum 確認										login-node				個人					checksum を取得し、事前に開発から入手した checksum と比較し、同じで																					checksum の値が同じであること																			
																					あることを確認する																																								
																				$	OBJ_KEY="share/sbint_models/reranker/sarashina2.2-0.5b-reranker-toy-0523"																									（前項目が有効な場合には、省略可)															
																				$	find /lustre/$OBJ_KEY -type f -exec md5sum {} \; | sort -k 2 | md5sum																																								
																																																													
																						結果を以下に貼り付ける																																							
																						判定		実行結果																																					
																						❌																						672043bc52837fd366ad64d8a989f98e  -																	
																																																													
																																																													
	10	ログアウト										login-node				個人					login-node からログアウトする																					エラー無きこと																			
																				$	exit																																								
																																																													
																																																													
	11	AWSからサインアウトし、										L3端末				SBINT			-		AWSからサインアウトし、ブラウザを閉じる																					エラー無きこと																			
		ブラウザを閉じる																																																											
		(VSOC)																																																											
																																																													
