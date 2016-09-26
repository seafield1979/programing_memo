#データベース操作  database
既にphpとMySQLが使用できる状態にしてある前提で進める
phpで以下のメソッドを使用する

##MySQLを使用する MySQL
phpにはMySQL接続のためのAPIが３種類用意されてある。  
mysql と mysqli そして PDO  
mysqlは古いので使用しない方がよいらしい。  
[どの API を使うか](http://php.net/manual/ja/mysqlinfo.api.choosing.php)
ここではmysqli を使ってMySQLのテーブルを操作してみる。

###接続〜切断の流れ

```php
//データベースに接続
$mysqli = new mysqli("localhost", "root", "poco", "testdb");
if ($mysqli->connect_error) {
    die('Db Connect Error: '.$dbh->connect_errno.':'.$dbh->connect_error);
}
//文字コードをセット(UTF-8)
$mysqli->set_charset('utf8');

//クエリを発行
// query は
//SELECT, SHOW, DESCRIBE や EXPLAIN 文、その他結果セットを返す文では、 mysql_query() は成功した場合に resource を返します。エラー時には FALSE を返します。
//それ以外の SQL 文 INSERT, UPDATE, DELETE, DROP などでは、 mysql_query() は成功した場合に TRUE 、エラー時に FALSE を返します。
if($result = $mysqli->query("SELECT * FROM table1")){
  if ($result->num_rows > 0){
    //1件づつ取得する
    while($row = $result->fetch_assoc()){
      printf( "%d %s %d %d<br>", $row["id"], $row["column1"], $row["column2"], $row["column3"]);
    }
  }
  // 結果セットを閉じる
  $result->close();
}
// DB接続を閉じる
$mysqli->close();
~~~
```

###select文
レコードを選択して、取得する

```php
//select文の結果はオブジェクトに返る
if($result = $mysqli->query("SELECT * FROM table1")){
  // 取得したレコードの件数は $result->num_rows に入る
  echo "rows=$result->num_rows";
  // レコードを取得するには $result->fetch_assoc() で１件づつとり出す
  while ($row = $result->fetch_assoc()) {
    // $row["カラム名"] でレコードのカラムを参照できる。
    echo "id=$result->row['name']"
  }
}
else {
  echo "失敗";
}
```

###insert文
テーブルにレコードを挿入（追加）する

```php
INSERT, UPDATE, DELETE, DROP などでは、 mysql_query() は成功した場合に TRUE 、エラー時に FALSE を返します
if($result = $mysqli->query("INSERT INTO users (name) VALUES ('hoge')")) {
  if ($result == 0) {
    echo "失敗";
  }
  else {
    echo "成功";
  }
}
```

