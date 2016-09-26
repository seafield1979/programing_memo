#セットアップ setup
###Linux
ルート(管理者権限のあるユーザー)で作業する  
~~~
$su -

##PHPをインストール

```sh
# yum -y install php php-mbstring php-mysql php-gd
```

"complete!" と表示されたら成功
~~~

PHPの設定 (/etc/php.iniを編集)

設定ファイル php.ini

~~~ini
/etc/php.ini
  #エラーログの出力パス
  error_log = [パス]  
  ※ error_log = /var/log/php_errors.log のように設定する

  #文字コード変更
  ;mbstring.internal_encoding = EUC-JP ->
  mbstring.internal_encoding = UTF-8

  # 変更前
  ; extension_dir = “ext”
  date.timezone =
  ;extension=php_mbstring.dll
  ;mbstring.language = Japanese
  ;extension=php_mysql.dll

  # 変更後
  extension_dir = "(ドライブ文字):\php\ext"
  date.timezone = "Asia/Tokyo"
  extension=php_mbstring.dll
  mbstring.language = Japanese
  extension=php_mysql.dll

~~~

###Windows
  ダウンロード＆インストール(Windows10 & PHP7)
  phpのzipファイルをダウンロードする。(ファイル名はphp-7.0.9-Win32-VC14-x64.zipとか)
  zipを展開する
  展開したフォルダを c:\php あたりに移動＆リネームする。
  展開したフォルダ内にあるbinフォルダにパスを通す。

  ※コマンドラインからphpを実行して「VCRUNTIME140.dllがない」というエラーが出たら 
    Visual C++ 2015 ランタイム  
    http://www.microsoft.com/ja-jp/download/details.aspx?id=48145  
  をインストールするとよい  