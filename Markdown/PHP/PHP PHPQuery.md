#PHP Query
<!-- phpquery:: -->
htmlをDOMのようなオブジェクトに変換してから、セレクタや便利メソッドを使ってあれこれできる。jQueryのPHP版。

|リンク|説明|
|---|---|
|[PHP Query Project](https://code.google.com/archive/p/phpquery/) | PHP Query 本家のドキュメント 
|[PHP Query Attributes](https://code.google.com/archive/p/phpquery/wikis/Attributes.wiki) | attr(),html(),text()等の属性系のメソッド 
|[DOMElement クラス](http://php.net/manual/ja/class.domelement.php) | PHP の DOM Elementクラス

```php
PHP Query DOMオブジェクト生成
require_once("../Library/phpQuery-onefile.php");

// Get Data Source
$html = file_get_contents("./_swift_memo.html");

// Get DOM Object
$dom = phpQuery::newDocument($html);

// Get 
$tag = $dom['div.hoge'];
print($tag);
```

###htmlからDOMオブジェクトを生成
いくつか方法がある

```php
// htmlファイルから生成
$dom = phpQuery::newDocumentFile("hoge1.html");

// テキストファイルから生成
$html = get_file_contents("hoge1.html");
$dom = phpQuery::newDocument($html);

// htmlテキストから生成 (newDocumentと同じ？)
$html = get_file_contents("hoge1.html");
$dom = phpQuery::newDocumentHTML($html);

// domオブジェクトをテキストに変換
$htmlStr = (string)$dom;
```



###DOM Element** オブジェクトと **PHP Query** オブジェクト
PHP Query オブジェクトは便利関数がいろいろ使えるが、PHP Query を 
PHP の DOM Element オブジェクトはこちら[DOMElementクラス](http://php.net/manual/ja/class.domelement.php) を参照。

```php
// セレクタで取得できるのは PHP Query オブジェクト
$pq1 = $dom['div.top'];

$pq2 = $dom['li'];    // $pq2は PHP Queryオブジェクト
foreach($pq2 as $element) {
  // $element は PHPの DOMElement オブジェクトなのでセレクタや文字列変換はできない
  // print($element);     エラー
  // $element['a'];       エラー
  // $element->attr("name")   エラー

  // pq()で PHP Query オブジェクトに変換してからあれこれする。
  pq($element)['a'];      // OK
  pq($element)->attr("name");   // OK
}
```

###セレクタ (Selector)
cssやjQueryのセレクタとほとんど同じセレクタが使用出来る。  
[Selectors.wiki](https://code.google.com/archive/p/phpquery/wikis/Selectors.wiki)

```php
// <div class="top"></div> を取得
$tag_top = $dom['div.top'];

// タグ
$tag_p = $tag_top['p'];

// id #
$tag_id = $tag_top['#hoge_id'];
print($tag_id . "\n");

// class .
$tag_class = $tag_top['.hoge'];
print($tag_class . "\n");

// attr
// have
$tag_attr = $tag_top['[href]'];
print($tag_attr . "\n");

// attr=value
$tag_attr = $tag_top['[name=shutaro]'];
print($tag_attr . "\n");

// attr!=value
$tag_attr = $tag_top['[name!=shutaro]'];
print($tag_attr . "\n");

// multiple selector
// selector1,selector2,...
$tag_mul = $tag_top['div.hoge,div.hoge2'];
print($tag_mul . "\n");

// only child (not grandchild)
$tag_child = $tag_top['.ul1 > li'];
print($tag_child . "\n");

// filter
// fist element
$tag_filter = $tag_top['.ul1 > li:first'];
print($tag_filter . "\n");

// last element
$tag_filter = $tag_top['.ul1 > li:last'];
print($tag_filter . "\n");

// first child (自分の親の最初の子要素)
$tag_filter = $tag_top['.ul1 > li:first-child'];
print($tag_filter . "\n");

// parent 親
$tag_filter = $tag_top['.ul2:parent'];
print($tag_filter . "\n");
```

###属性 (Attributes)
attr()メソッドで属性を取得、設定できる。
[PHP Query Attributes](https://code.google.com/archive/p/phpquery/wikis/Attributes.wiki)  

```php
$dom1 = $dom['div.hoge'];
// テキストノードを取得
$dom1->text();

// html要素を参照(自分のノードは含まない)
$dom1->html();

// html要素を設定
$dom1->html("<div>hoge</div>");

// 指定の名前の属性
$dom1->attr("name");

// 指定の名前の属性を書き換える
$dom1->attr("name", "new_name");

// 属性を削除
$dom1->removeAttr("name");

// クラスを追加
$dom1->addClass("hoge");
```