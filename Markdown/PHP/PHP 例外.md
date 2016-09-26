#例外処理 exception

PHPもC＋＋と同じ感じで例外をキャッチできる。何かしらのエラーが起きた時に、そのエラーを受け取るブロックまで自動で処理が遷移してエラー処理を行うことができる。

```php
try {
  // 例外が発生するかもしれない処理を try スコープに入れる
  throw new Exception("error1");
} catch (Exception $e){
  // 例外をキャッチ。例外に対応した処理を行う
  echo $e->getMessage . "\n";
} finnaly {
  // 必ず実行される処理
} 
```

Exceptionを継承した自前の例外クラスを使ったエラーハンドリング処理

```php
class MyException extends Exception { }
class MyException2 extends Exception { }

class Test {
    public function testing() {
        try {
            try {
                // ここでエラーを投げる
                throw new MyException('foo!');
                throw new MyException2('woo!');
            } catch (MyException $e) {
                // MyException の throwはこちらでキャッチされる
                // 改めてスロー
                throw $e;
            } catch (MyException2 $e) {
                // MyException2 の throwはこちらでキャッチされる
                throw $e;
            }
        } catch (Exception $e) {
            var_dump($e->getMessage());
        } finally {
            echo "finnaly!\n";
        }
    }
}

$foo = new Test;
$foo->testing();
```