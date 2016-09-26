#ログを出力する logs

**エラーログ出力 error_log**  

```php
//　エラーログ出力
error_log("hogehoge");

// 任意のファイルにし出力 (第2引数に3を設定)
error_log("hoge","3","./error.log");

// 標準出力をしないprint_rを使う
error_log(print_r($array1, true));
~~~

<!-- log:: logs::  -->
**php.iniの設定**  
html上で実行されたphpプログラムのエラーログを出力するには php.ini で設定を行う必要がある。

~~~ini
;htmlにエラーを出力するかどうか
display_errors = On/Off
  
;ログの出力レベル
error_reporting = E_ALL | E_STRICT

;ファイルに出力
log_errors = On/Off

;エラーログのパス
error_log = /var/log/php/error.log

```

上記設定を行ってもエラーログがログファイルに出力されない場合は、エラーログファイルの出力ディレクトリ、出力ファイルに書き込み権限が設定されているか確認してみる。ログファイルのディレクトリを丸ごと chmod 777 で全ユーザに書き込み設定しておくのもあり。

**ログの出力(tailを使用して常に監視)**  
`$tail -50f /var/log/php/error.log`

**php.iniの変更を反映させるためにはApacheをリスタートする**  
`$sudo apachectl restart`