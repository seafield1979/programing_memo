#PHP interactive
対話モード(インタラクティブシェルモード)  interactive 
<!-- interactive:: -->
**Windows**
~~~
php -a 
でインタラクティブモードを起動し、
  echo "hello world";
  と入力しても何も表示されない。

  インタラクティブモードで処理を実行させるには
  <?php
    echo "hello world";
  ?>
  などと <?php ?> で処理を囲ってから Ctrl + Z を入力する必要がある。
~~~
**Mac,Linux**  
-a オプションでphpを起動する
~~~sh
$ php -a 
php > echo "hello world!!";
hello world
php >
~~~