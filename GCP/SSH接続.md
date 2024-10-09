#### GCPのVMインスタンスにVSCodeでSSH接続

AWSと似ているが少々違っていた


1. GCPにログインし、VMインスタンスを作成する
2. 作成したインスタンスの接続欄の太字のSSHを押下するとブラウザでのSSH接続ができる

 
4. ブラウザでターミナル画面が開くので.sshに移動し、lsコマンドを叩くとauthorized_keys  id_rsa  id_rsa.pubの3つのファイルを確認できる
5. id_rsaとid_rsa.pubをローカルにダウンロードする
6. 右上の方にファイルをダウンロードとあるので押下する。するとパスを入力するポップアップが出てくるのでパスを入力する
![img](https://github.com/yuhei1012/GCP_Practice/blob/8ac2e0d13932a7207490c8c51d68b093e0298ee3/GCP/SSH%E7%94%BB%E5%83%8F/GCP_VM%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%83%B3%E3%82%B9_%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%83%80%E3%82%A6%E3%83%AD%E3%83%B3%E3%83%BC%E3%83%89%E7%94%BB%E9%9D%A2.png)
- ここで言うパスとはssh接続しているファイルのことである。ローカルで保存したい場所のことではない！今回で言えば/home/userneme/.ssh/id_rsaと入力することとなる

6. ダウンロードができたらローカルの.sshフォルダに id_rsa  id_rsa.pubを移動した。※そのままでも良いが自分は分かりやすくするため移動
7. VSCodeでcode ~/.ssh/configコマンドを叩き、下記のように記述する

````
    Host my-instance # インスタンス名
    HostName 12.34.56.789  # 外部 IP
    Port 22 # VMインスタンスのファイアウォール設定を確認
    IdentityFile "C:\Users\username\.ssh\id_rsa" # id_rsaがあるパス　今回はwindows使用
    PasswordAuthentication no
    IdentitiesOnly yes
````
 VMインスタンスのファイアウォール設定を確認

8. ダウンロードしたid_rsa.pub(認証鍵)を cat ~/.ssh/id_rsa.pubで確認し、全てコピーする
9. コピーしたらブラウザからssh接続しauthorized_keynsに貼り付ける
- #Added by Googleという行が2つあるが全て消すこと。デフォルトで記述されている認証鍵は時限付きですぐに切れる仕様となっている

googleのkey
![img](https://github.com/yuhei1012/GCP_Practice/blob/e6ecac830641fd6a062535c20732e23f4717be59/GCP/SSH%E7%94%BB%E5%83%8F/googlekey%E8%A8%98%E8%BF%B0%E7%94%BB%E9%9D%A2.png)
- 恐らくブラウザからsshすることを設計されているのでブラウザからssh接続するたびに毎回変わる登録されてしまうと思われる。
- Google Cloudが自動的に鍵を追加しているため、手動で削除しても再び登録されてしまう

接続エラー
![img](https://github.com/yuhei1012/GCP_Practice/blob/84cf4be8ccfe0f25ec95a22b2c14cf2361e7b32a/GCP/SSH%E7%94%BB%E5%83%8F/SSH%E6%8E%A5%E7%B6%9A%E3%82%A8%E3%83%A9%E3%83%BC.png)

  
  これは、Google CloudのOSログイン機能（OS Login）が有効になっている場合に発生します。対応策は下記の通りです。
- インスタンスを選択し、詳細ページで「編集」ボタンをクリックします。
- OSログインを無効化： 左側のCompute Engine欄「メタデータ」のセクションにある「追加メタデータ」から、キー enable-oslogin に対して値 FALSE を追加します。
- 変更を保存： 画面下部の「保存」ボタンをクリックして設定を反映します。

  これでVSCodeから接続できます

  


   







