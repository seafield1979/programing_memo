Java Collection Map

Mapの概要

Mapインタフェースはキーと値が対になった要素を持ちます。キーの重複は許可されず各キーは1つの値のみに対応付けられます。キーから値を参照するデータ構造を持ったデータの利用に適しています。Mapインタフェースを実装するクラスとして、HashMap、TreeMap、LinkedHashMapが定義されています。

HashMapクラス

Mapインタフェースを実装した基本となるクラスです。キーの並び順を保持しないという特性をもっています。Mapインタフェースを実装したクラス内では最も高速に動作します。

HashMapクラスはHashTableクラスの後継として定義されたものです。違いは要素の操作を行う際、HashTableクラスが同期化されているのに対して、HashMapクラスが同期化されていない点です。HashMapクラスは同期化されていない分、高速に動作します。同期化された処理をHashMapクラスで行う場合は、HashMapクラスに同期処理を実装します。

TreeMapクラス

キーが自然順序、もしくはコンストラクタに指定されたComparatorに従って昇順にソートされた要素を持ちます。

LinkedHashMapクラス

キーが挿入される挿入順を保持します。コンストラクタの引数の指定により、挿入順ではなくアクセス順を保持することもできます。デフォルトの状態では挿入順です。

Mapのメソッド

戻り値	メソッド	説明
void	clear()	Mapからすべての要素を削除します。
boolean	containsKey(Object)	引数で指定されたキーが、Mapに存在する場合trueを返します。
boolean	containsValue(Object)	引数で指定された値が、Mapに存在する場合trueを返します。
Object	get(Object)	引数に指定されたキーに紐付けられた値を返します。
Object	put(Object, Object)	指定された引数(キー, 値)を、Mapに挿入します。
void	putAll(Map)	引数に指定されたMapの要素すべてを、Mapに挿入します。
Object	remove (Object)	引数に指定されたキーがMapに存在する場合、そのキーと紐付けられた値を削除します。
Set	keySet()	Mapに格納されているキーを持つ、セットビューを返します。
Collection	values()	Mapに格納されている値を持つ、コレクションビューを返します。
HashMapのサンプル

java

// HashMapのテスト
public void testHashMap() {
    try {
        HashMap<String,String> map1 = new HashMap<String,String>();

        // いろいろ追加
        String[][] names = {{"hoge1", "111"},
                {"hoge2", "222"},
                {"hoge3", "333"}, 
                {"hoge4", "444"},
                {"hoge5", "555"}};
        for (String[] array : names) {
            map1.put(array[0], array[1]);
        }

        // 全表示
        for (String key : map1.keySet()) {
            System.out.println(key + "=" + map1.get(key));
        }

        // Mapの結合
        System.out.println("\nconnect map");
        HashMap<String,String> map21 = this.getHashMap("hoge", 10);
        HashMap<String,String> map22 = this.getHashMap("mage", 10);
        map21.putAll(map22);

        for (String key : map21.keySet()) {
            System.out.println(key + "=" + map21.get(key));
        }

        // 要素の削除
        // keyのオブジェクトに一致したものを削除する
        System.out.println("\nremove object");
        map21.remove("hoge3");
        map21.remove("hoge5");
        map21.remove("hoge7");
        map21.remove("hoge9");

        for (String key : map21.keySet())  {
            System.out.println(key + "=" + map21.get(key));
        }

        // valueのセットを表示する
        System.out.println("\nvalues");;
        for (String value : map21.values()) {
            System.out.println("value=" + value);
        }
    } catch (Exception e) {
        
    }
}

// 適当なHashMapを作って返す
static HashMap<String,String> getHashMap(String keyName, int size) {
    HashMap<String,String> map = new HashMap<String,String>();
    for (int i=0; i<size; i++) {
        String key = keyName + String.valueOf(i);
        String value = String.valueOf((int)(Math.random()*100));
        map.put(key, value);
    }
    return map;
}