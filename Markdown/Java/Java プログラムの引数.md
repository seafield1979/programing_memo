###Java プログラム引数
プログラムに渡された引数は、mainメソッドに渡される引数はargsで参照できる。

~~~sh
# javaの呼び出し部分
java Hoge 引数1 引数2 引数3 の場合
~~~

の場合  

main() の args は` ["引数1", "引数2", "引数3"]` 配列になる。
※args[0]が全引数文字列のC言語と異なり、args[0]から一つ目の引数が代入される。

```swift
class Hoge {
    public static void main(String args[]) {
		 // 引数を全て表示する
        for (int i=0; i<args.length; i++) {
            System.out.printf("%d %s\n", i, args[i]);
        }
        
        // 第１引数を整数に変換する
        int mode = Integer.parseInt(args[0]);
    }
}
```

