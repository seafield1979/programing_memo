#Java Iterator イテレータ

Iteratorインタフェースはコレクション内の要素に順番にアクセスする手段を提供します。コレクション・フレームワーク内のクラスは何らかの手段でこのIteratorインタフェースを使用できます。


List,Set,Mapの各コレクションはイテレータが使用出来る。要素を順番に全部処理したい場合などに利用しよう。

**Iteratorの特徴**  

* コレクションを継承したクラスで使える
* 要素を順にアクセスするのに使用できる
* 順に処理を行う途中で要素を削除することができる


**Iteratorインタフェースのメソッド**

|戻り型|	メソッド	説明|
|---|---|
|boolean hasNext() | 繰り返し処理において、次の要素がある場合にtrueを返します。
|Object	next()|繰り返し処理において、次の要素を返します。
|void remove()|繰り返し処理において呼び出された、最後の要素を削除します。

```java
public void testIterator() {
    Set<Integer> hs1 = new HashSet<Integer>();

    for (int i = 0; i < 10; i++) {
        hs1.add(new Integer(i));
    }
    System.out.println("削除前" + hs1);

    for (Iterator<Integer> i = hs1.iterator(); i.hasNext();) {
        Integer val = i.next();
        i.remove();
        // System.out.println("value:" + String.valueOf(val));
    }
    System.out.println("削除後" + hs1);
}
```

