#コレクション Set

[Java道 Set](http://www.javaroad.jp/java_collection3.htm)

##Setの概要

Setインタフェースは重複要素を持たないオブジェクトの集合を実装します。Setインタフェースを実装するクラスとしてHashSet、TreeSet、LinkedSetが定義されています。

**HashSetクラス**
Setインタフェースを実装した基本となるクラスです。保持する要素内において重複要素を持ちません。保持する要素の順序を保証しません。Setインタフェースを実装したクラス内では最も高速に動作します。

**TreeSetクラス**
保持する要素内において重複要素を持ちません。また、保持する要素は要素の自然順序、もしくはコンストラクタに指定されたComparatorに従って昇順にソートされます。

**LinkedHashSetクラス**
保持する要素内において重複要素を持ちません。また、要素の挿入される挿入順を保持します。


|戻り値|メソッド|説明|
|---|---|---|
|boolean | add(Object) | 引数で指定された要素が、セットに存在しない場合追加されます。
|void | clear() | セットからすべての要素を削除します。
|boolean | contains (Object) | 引数で指定された要素が、セットに存在する場合trueを返します。
|boolean | remove  (Object) | 引数で指定された要素が、セットに存在する場合その要素を削除します。

```java
// HashSet をテスト
// Integer型の値を保持するSetを作成
public void testHashSet1() {
    try {
        HashSet<Integer> hs1 = new HashSet<Integer>();

        // 要素を追加
        for (int i=0; i<100; i++) {
            int rand = (int)(Math.random()*100);
            Integer val = new Integer(rand);
            if (hs1.contains(val)) {
                System.out.println(String.valueOf(val) + " is already exist.");
            } else {
                hs1.add(new Integer(rand));
            }
        }
        // 要素を表示
        Iterator<Integer> it = hs1.iterator();
        while (it.hasNext()) {
            System.out.println(String.valueOf(it.next()));
        }
        System.out.println("size:" + String.valueOf(hs1.size()));

    } catch (Exception e) {
        
    }
}
```

