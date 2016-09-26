#PHPのファイル file paths

phpファイルの置き場所  

###Mac

|ファイルパス|説明|
|!--|!--|
/private/etc/php.ini         | php設定ファイル
/Library/WebServer/Documents | Webドキュメントルート

###Linux

|ファイルパス|説明|
|!--|!--|
/etc/php.ini             | PHP設定ファイル
/var/log/php/errors.log  | エラーログ<br>ファイルパスはphp.ini の error_log プロパティで設定する
/Applications/MAMP/htdocs  | MAMPのWebドキュメントルート