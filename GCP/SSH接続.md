#### GCPのVMインスタンスにVSCodeでSSH接続

AWSと似ているが少々違っていた


1. GCPにログインし、VMインスタンスを作成する
2. 作成したインスタンスの接続欄の太字のSSHを押下するとブラウザでのSSH接続ができる
3. ブラウザでターミナル画面が開くので.sshに移動し、lsコマンドを叩くとauthorized_keys  id_rsa  id_rsa.pubの3つのファイルを確認できる
4. id_rsaとid_rsa.pubをローカルにダウンロードする
5. 右上の方にファイルをダウンロードとあるので押下する。するとパスを入力するポップアップが出てくるのでパスを入力する
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
恐らくブラウザからsshすることを設計されているので毎回変わるのかもしれない
11. 接続ができるようになる
    


   







