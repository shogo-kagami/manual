## L3ルームでVSOC 接続
1. 作業端末の電源 ON
   - L3 ルーム入室し、作業端末の電源を入れる
1. 作業端末にログイン
   - GUIで作業端末にログインする
1. VSOC環境にログイン
   - デスクトップ左上の「リモートデスクトップ接続」のアイコンをクリックし、VSOCにログインする

## モデルのアップロード
1. login-node にログイン
	- Putty などのターミナルソフトを起動し、ログインノードに接続する

1. ファイル存在確認
   - モデルの確認  
   	`ls -l /store/share/~`  

1. コピー作業
    - 以下のコマンドラインを実行する  
   	`cp -a [コピー元] [コピー先]`

1. checksum 確認
    - 事前に共有された checksum と比較し、同じであることを確認する  
      `find /lustre/share/sbint_models/finetuned/release/sbint-YYYY-MM-DD/sbint-YYYY-MM-DD-70b-sft/ -type f -exec md5sum {} \; | sort -k 2 | md5sum`

1. ログアウト  
   		`exit`
   
1. AWSからサインアウトし、ブラウザを閉じる(VSOC)
