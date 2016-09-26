 
#hello world
<!-- hello:   -->
###コンソールから実行
コンソールから実行するには拡張子.php のファイルを作成し
~~~php
- helloworld.php -
<?php
  echo "hello world!!\n";
?>
~~~
~~~sh
$ php helloworld.php
~~~
のようにしてphpコマンドの引数としてphpファイルを実行する

###HTMLファイル内のphpプログラムを実行
htmlファイル内でPHPプログラムを動作させてみる
~~~html
- php_test.html -

<html>
 <head>
  <title>PHP Test</title>
 </head>
 <body>
 <!-- <?php ~ ?> 内にPHPプログラムプログラムを記述する -->
 <?php
    echo '<p>Hello World</p>';
  ?>
 </body>
</html>
~~~