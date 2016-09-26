#インクルード include

外部ファイルを読み込む。ファイルパスを指定しない場合は php.ini の include_path で指定したパスを検索する。
includeは致命的なエラーを発生させないが、requireは致命的なエラーが発生してスクリプトが停止する可能性がある。

```sh
include 'hoge.php';   //パスを指定しない php.ini の include_path を探す

include './hoge.php'; //パスを指定
```

###使い分け
[includeとrequireの使い分け](  http://furudate.hatenablog.com/entry/2013/10/17/141557)
  include  ページの内容を差し込みたいrequire PHPの処理を読み込みたい。同じ処理を何度も読み込む必要はないので、通常は require_once を使う。

require で呼び出したphpファイルに標準の引数を渡す方法(requireするphpはメソッドやクラスの定義だけなので普通はやらない！)

```sh
// hoge.php に引数を渡す
＄argv = array("1 2 3", "1", "2", "3");
＄argc = cont($argv);
require_once("hoge.php");

```

```sh
--- hoge.php ---

// 引数を表示する
foreach($argv as $arg) {
  print($arg . "\n");
}
```

