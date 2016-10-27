#コレクション List

Listインタフェースは重複要素を許可し、要素において順番を持ちます。インデックス番号により要素にアクセスするメソッドや、要素のインデックス番号を調べるメソッドが用意されています。Listインタフェースを実装するクラスとして、ArrayList、LinkedListが定義されています。

**ArrayListクラス**
[クラス ArrayList](https://docs.oracle.com/javase/jp/7/api/java/util/ArrayList.html)  
Listインタフェースを実装する基本となるクラスです。ランダムアクセスをサポートしており、要素の検索において高速に動作します。要素の追加、削除においては追加、削除された要素以降の要素を再配置する必要があるため低速です。

ArrayListクラスはVectorクラスの後継として定義されたものです。違いは要素の操作を行う際、Vectorクラスが同期化されているのに対して、ArrayListクラスが同期化されていない点です。ArrayListクラスは同期化されていない分、高速に動作します。同期化された処理をArrayListクラスで行う場合は、ArrayListクラスに同期処理を実装します。

**LinkedListクラス**
[クラス ArrayList](https://docs.oracle.com/javase/jp/7/api/java/util/LinkedList.html)  

要素の順番を各要素の前後のリンク情報を持つことによって保持します。要素の追加、削除を行う場合、リンク情報を変更することによって処理するため高速に動作します。 要素の検索においては、要素に順番にアクセスするため低速です。


###コンストラクタ

####ArrayListのコンストラクタ
|コンストラクタ | 説明|
|---|---|
|ArrayList() | 初期容量10の空のArrayListを作成します。
|ArrayList(Collection) | 引数に指定されたコレクションの要素を保持したArrayListを作成します。
|ArrayList(int) | 引数に指定した容量の空のArrayListを作成します。

####LinkedListクラスのコンストラクタ
|コンストラクタ | 説明|
|---|---|
|LinkedList()|	空のLinkedListを作成します。
|LinkedList(Collection)|	引数に指定されたコレクションの要素を保持したLinkedListを作成します。


###Listのメソッド

|戻り型|	メソッド|	説明|
|---|---|---|
|void|add(int, Object)|引数intで指定された位置に、要素を追加します。
|boolean|add(Object)|リストの最後に要素を追加します。
|boolean|addAll(Collection)|リストの最後に、引数Collectionで指定された要素すべてを追加します。
|boolean|addAll(int, Collection)|	引数intで指定された位置に、引数Collectionで指定された要素すべてを追加します。
|void|clear()|リストからすべての要素を削除します。
|Object|get(int)|リスト内のインデックス番号で指定された位置にある要素を返します。
|Object	|remove(int)|リスト内のインデックス番号で指定された位置にある要素を削除します。返り値として、削除された要素を返します。
|Object|set(int, Object)|	リスト内のインデックス番号で指定された位置にある要素を引数Objectで指定された要素に置き換えます。返り値として、置き換えられた古い要素を返します。
|int|size()|リスト内にある要素の数を返します。
|Array<T>


コレクションを逆順で参照したい場合は Collections.reverse(リスト)を使用する。

```java
// lists を逆運で参照する
Collections.reverse(lists);
for (MyData data : lists) {
}
```

###サンプル
####ArrayList

```java
// ArrayListのテスト
// リストに要素を追加、要素の読み出し
public void testArray2() {
    try {
        // 初期容量が100の配列
        ArrayList<String> list1 = new ArrayList<String>(100);

        // 要素の追加
        for (int i=0; i<10; i++) {
            list1.add(i, "hoge" + String.valueOf(i));
        }

        // 要素の削除
        list1.remove(5);

        // 要素数(インデックスで参照できる)
        System.out.println("size:" + String.valueOf(list1.size()));
        
        // 値の読み出し
        for (int i=0; i<list1.size(); i++) {
            String str = (String)list1.get(i);
            System.out.println(str);
        }

        // 値の設定
        for (int i=0; i<list1.size(); i++) {
            list1.set(i, "oge" + String.valueOf(i));
        }

        // 値の読み出し
        for (int i=0; i<list1.size(); i++) {
            String str = (String)list1.get(i);
            System.out.println(str);
        }
    } catch (Exception e) {
        System.out.println(e);
    }       
}
```

####LinkedList
LinkedListでスタックを実現するサンプル。
pushでスタックの一番上に要素を追加、popで一番上から要素を取り出す

|戻り型|	メソッド|	説明|
|---|---|---|
|void|add(`<T>`)|リストの末尾に要素を追加する
|void|push(`<T>`)|リストの先頭に要素を追加する<br>0番目に挿入する|
|`<T>`|pop()/poll()|先頭要素を取り出す。取り出した要素はリストから削除<br>要素がない場合はnullを返す|
|void|removeFirst()|リストの先頭要素を削除する|
|void|removeLast()|リストの末尾要素を削除する|

push/pop は先入、先出し（スタック）を実現する
~~~
pushはリストの先頭に要素を追加する
push 1
 ↓
[1]

push 2
 ↓
[2][1]

push 3
 ↓
[3][2][1]


pop/pollはリストの先頭を取り出す。取り出した要素はリストから削除
pop/pol
[3]
 ↑[2][1] 
 
pop/poll
[2]
 ↑ [1]

pop
[3]
 ↑
~~~

```java
// LinkedListのテスト
// popやpush等のキュー用のメソッドが用意されているので待ち行列として使える
// スタックとして使用する (先入先出し)
public void testLinkedList1() {
    try {
        // private ArrayList<Rule> list = new ArrayList<Rule>();
        LinkedList<String> list1 = new LinkedList<String>();

        // 要素の先頭に要素追加
        for (int i=0; i<10; i++) {
            list1.push("hoge" + String.valueOf(i));
        }
        
        // 要素の取り出し
        for (int i=0; i<5; i++) {
            String str = (String)list1.pop();
            System.out.println("pop:" + str);
        }

        // 要素を最後から順に取り出し
        for (int i=0; i<list1.size(); i++) {
            System.out.println(list1.get(i));
        }

        // 破棄
        list1.clear();
        System.out.println("size:" + String.valueOf(list1.size()));
    } catch (Exception e) {
        System.out.println(e);
    }       
}
```

