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

## powershellの場合
https://www.vwnet.jp/windows/PowerShell/2017103101/AES256byPowerShell.htm

### まず以下のpowershellファイルをフォルダに配置する
AES256.ps1

Make256Key.ps1

### ①共通鍵の作成
--# Make256Key.ps1 共通鍵のフルパス

上記のpowershellファイルが配置されているディレクトリで以下コマンドを実行

PowerShell -ExecutionPolicy RemoteSigned .\Make256Key.ps1 .\share.key

※「**PowerShell -ExecutionPolicy RemoteSigned**」でPowerShell のスクリプトの実行ポリシーを許可

### ②暗号化
--# AES256.ps1 -Encrypto -KeyPath 共通鍵のフルパス -OutPath 出力先フォルダ(省略可) -Path 暗号化するファイル

PowerShell -ExecutionPolicy RemoteSigned .\AES256.ps1 -Encrypto -KeyPath .\share.key -OutPath .\encrypto -Path .\plain.txt

### ③復号化
--# AES256.ps1 -Decrypto -KeyPath 共通鍵のフルパス -OutPath 出力先フォルダ(省略可) -Path 復号化するファイル

PowerShell -ExecutionPolicy RemoteSigned .\AES256.ps1 -Decrypto -KeyPath .\share.key -OutPath .\decrypto -Path .\encrypto\plain.txt.enc


