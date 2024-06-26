# パスワード暗号化復号化検証

## コマンドプロンプトの場合
### ①opensslインストール
https://web.save-editor.com/crypt/crypt_aes_command_openssl_install.html
### ②暗号化
#### 参考URL
https://qiita.com/noraworld/items/4b5d902332e1f557804a
#### ・鍵生成
openssl genrsa -aes256 -out key.pem 4096

1234test

1234test

#### ・X.509 証明書生成 (自己署名証明書)
openssl req -new -x509 -key key.pem -out cert.pem -days 36500 -subj /CN="Yamamoto"

1234test

#### ・暗号化
openssl smime -aes256 -binary -encrypt -in plain.txt -out encrypted.txt cert.pem

#### ・復号
openssl smime -decrypt -in encrypted.txt -out decrypted.txt -inkey key.pem pass:1234test

openssl smime -decrypt -in encrypted.txt -inkey key.pem -passin pass:1234test

## powershellの場合（**powershellコマンドで実行する方法**）
https://www.vwnet.jp/windows/PowerShell/2017103101/AES256byPowerShell.htm

### まず以下のpowershellファイルをフォルダに配置する
AES256.ps1

Make256Key.ps1

### ①共通鍵の作成
#### 共通鍵の作成コマンド：Make256Key.ps1 共通鍵のフルパス
PowerShell -ExecutionPolicy RemoteSigned .\Make256Key.ps1 .\share.key

　　※「**PowerShell -ExecutionPolicy RemoteSigned**」でPowerShell のスクリプトの実行ポリシーを許可
 
　　※上記のpowershellファイルが配置されているディレクトリで以下コマンドを実行
  
　　→共通鍵が「カレントディレクトリ\share.key」に作成される。
### ②暗号化
#### 暗号化コマンド：AES256.ps1 -Encrypto -KeyPath 共通鍵のフルパス -OutPath 出力先フォルダ(省略可) -Path 暗号化するファイル

PowerShell -ExecutionPolicy RemoteSigned .\AES256.ps1 -Encrypto -KeyPath .\share.key -OutPath .\encrypto -Path .\plain.txt

　　→暗号化された文字が「カレントディレクトリ\encrypto\plain.txt.enc」に作成される。
### ③復号
#### 復号コマンド：AES256.ps1 -Decrypto -KeyPath 共通鍵のフルパス -Path 復号化するファイル

PowerShell -ExecutionPolicy RemoteSigned .\AES256.ps1 -Decrypto -KeyPath .\share.key -Path .\encrypto\plain.txt.enc

　　→復号された文字がコマンドラインに出力される
  
  　※参考のソースは復号された文字をファイル出力させていたが、コマンドラインに出力するように**AES256.ps1**を改修
