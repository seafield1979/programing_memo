#クラス class
<!-- class:: object:: -->
PHPのクラスは以下のような機能を持つ。
※オブジェクトはクラスから生成された実態。インスタンスともいう。
  
###基本

```php
//クラスの定義:
class Hoge {
  // クラス外から参照できるプロパティ
  public $publicStr = "公開";
  public $name = "";

  // クラス外から参照できないプロパティ
  private $privateStr = "極秘";

  // コンストラクタ
  function __construct($name) {
    $this->name = $name;
  }
  // デストラクタ (参照元が１つもなくなった時に呼ばれる)
  function __destruct() {      
  }

  public function hogeFunc(){
    // 処理
  }
}
```

##インスタンス(オブジェクト)生成

```php
// $instance = new クラス名();
$hoge1 = new Hoge("hohoho");

  もしくは

// $instance = new クラス名;
$hoge2 = new Hoge;

//メンバ参照
// $instance->プロパティ変数;
$hoge->name;

//メソッド呼び出し
$instance->メソッド();

//インスタンスの参照:  
//生成済みのインスタンスを別の変数に代入すると、代入先の変数は代入元の変数の参照になる。
//参照なので実態は１つだけ。よって参照元、参照先の変更はどちらにも反映される。

$hoge = new Hoge();
$hoge2 = $hoge;   // $hoge と $hoge2は同じ
~~~
```

###オブジェクトのクローン作成
PHPではインスタンス変数の代入は参照を作るだけなので、別のインスタンスを作りたい場合は clone を使用する

```php
$hoge = new CHoge();
$hoge2 = clone $hoge;
$hoge->name = "shutaro";
$hoge2->name = "naotaro";
// これで $hoge->name と $hoge2->name の値はそれぞれ別の値になる。

//クローン時に実行されるメソッドを定義できる

class Hoge{
  public $id = 0;
  function __clone(){
    ~クローン時の処理~
    個別のインスタンスで別々の値を持ちたいときなどに、ここで新たに確保する
  }
}
```

##アクセス制限
クラスのプロパティやメソッドの先頭にアクセス制限を指定するパラメータを指定できる。

|アクセス制限子|説明|
|---|---|
|public | クラスの外から参照可能  
|private | クラスの外から参照不可  
|protected | 定義したクラスと継承したクラスからのみ参照可能  