# noipddnsclient
No-IPで取得したドメインをダイナミックDNS(DDNS)に登録して利用するためのクライアントスクリプトです。
Linuxで動作します。公式のツールがサポートしていないRHEL系でも使用できます。
グローバルIPアドレスが固定されていない自宅サーバでの利用を想定しています。

## 特徴
* Linuxで動作します。(Python3スクリプトです）
* 複数ドメインに対応しています。
* 定期強制更新が可能です。
* 現在のグローバルIPアドレスが前回実行と異なるときに、DNSレコードを現在のグローバルIPアドレスに更新します。このことによりサーバ上で高頻度で実行しても、DDNSサービスには負担をかけません。(グローバルIPを調べるサービスには負担を書けるのでご注意ください)

## システム要件
* python3以上
* requestsモジュール

## インストール方法
1. requestsモジュールをインストールします。
```
# pip install requests
```
2. スクリプトを任意の場所に配置します。
```
# cp noipddnsclient.py /usr/local/noipddnsclient/bin/noipddnsclient.py
```
3. パーミッションを700に設定します(認証情報を記載するため）
```
# chmod 700 /usr/local/noipddnsclient/bin/noipddnsclient.py
```
4. noipddnsclient.pyにNo-IPの認証情報と、DDNSに登録するドメインを記載します
```
# vi /usr/local/noipddnsclient/bin/noipddnsclient.py
```
```
# No-IPのユーザ名、パスワード
USERID="1234567890"
PASSWORD="Passworddayo!"

# ドメインリスト
# -書式-
#   FQDN,FQDN,...
#    (カンマ区切り)
# -例-
#   wwww.example.com
#   wwww.example.com,blog.example.com,wwww.xxxx.com
DOMAINLIST="www.example.com,himitsu.example2.com"

# 強制的にDDNSを更新する日数間隔（無効にする場合は大きい数字にしてください）
DAYS_FORCE_UPDATE=1

```
5.crontabに登録します。
```
# cat <<EOF > /etc/cron.d/noipddnsclient
0,15,30,45 * * * * root /usr/local/noipddnsclient/bin/noipddnsclient.py
EOF
```

## 使い方
* 上記通り、cronに登録して実行します。
* ログは/var/log/noipddnsclient.logに出力します。(何かあった時しか出力しません)
* IPアドレスの記録と前回更新日時は/var/lib/noipddnsclient.txtに保存しています。
* LOG_VERBOSE=Trueにすると冗長ログを出力します。（実行ごとの開始終了を出力します）

## 注意
* 非公式かつ無償ですので自己責任でご使用ください。いかなる損害が発生しても責任は負いません。
* 必要に応じてログはログローテーションしてください。
* 利用者は日本人しか想定していません。（なので日本語で書いています）
* ソースはcoolな書き方をしていませんので参考になりません。

## Autor
* 作成者：ちぢぴー(chidipy)
* E-mail：webmaster@chidipy.jpn.com

## ライセンス
MITライセンスです。

