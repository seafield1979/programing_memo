#文字列 string

[Android Developer String](https://developer.android.com/reference/java/lang/String.html)

###String
Javaでは文字列変数はString型で管理される。文字列リテラルはC言語と同じchar型(16ビットのUnicode)が使われる。

```swift
"ほげほげ"  // 文字列リテラル

// 初期値の代入と、その値を書き換える
String str1 = "hoge";
str1 = "hoge1";

// 初期値を後で代入する
String str2;
str2 = "hoge2";

// 文字列は + で連結できる
String str3 = str1 + " " + str2;

System.out.println("str1:" + str1 + "\nstr2:" + str2 + "\nstr3:" + str3);  // hoge1

// 指定のフォーマットで文字列を作成
// Androidだと Localeを指定しないと警告になる
String str4 = String.format(Locale.US, "%d:%s", 100, "hoge");
```

###Stringのメソッド

|戻り値|メソッド|説明|
|---|---|---|
|String|format(Locale l, String format, Object... args)|"%d"のようなフォーマット指定で文字列を作成する|
|boolean|equals(Object)| 文字列が等しいかどうかチェック|
|String|substring(int start, int end)|文字列の切り出し|
|int|indexOf(String str)|引数の文字列が見つかった位置を返す<br>文字列が見つからなかった場合は-1を返す|
|String|concat(String str)|文字列を結合する|
|boolean|startsWith(String prefix)|文字列の先頭が一致するかをチェックする|
|boolean|endsWith(String suffix)|文字列の末尾が一致するかをチェックする|

```swift
String str = "01234567890";
String str2 = "this is an apple!";

// 文字列が同じかどうかチェックする equals(Object)
System.out.println("- equals");
boolean ret = "hoge".equals("hoge");
System.out.println(ret);

// 指定位置の文字を抜き出す  charAt(int:pos)
System.out.println("- charAt");
for (int i=0; i<str.length(); i++) {
    System.out.println(String.valueOf(str.charAt(i)));
}

// 文字列の切り出し substring(int:start,int:end)
System.out.println("- substring");
String substr = str.substring(3,6);
System.out.println(substr);

// 検索文字列が見つかった位置を返す indexOf(String:searchStr)
System.out.println("- indexOf");
int pos = str.indexOf("67");
System.out.println(str.substring(pos));

pos = str.indexOf("hoge");  // 見つからない文字列
System.out.println(String.valueOf(pos));  // -1

// 文字列の結合 concat(String)
System.out.println("- concat");
String concat1 = str.concat("_concat!");
System.out.println(concat1);

// 先頭、末尾の文字列が一致しているかをチェック endsWith(String) startWith(String)
System.out.println("- startsWith, endsWith");
System.out.println(String.valueOf(str.startsWith("012")));
System.out.println(String.valueOf(str.endsWith("890")));
```

###Stringとchar配列の相互変換
[[Java] String → char[], char[] → String への変換](http://kadoppe.com/archives/2011/03/java-string-char-conversio.html)

```swift
// String -> char
// String.toCharArray()を使用する
String str = "Hello";
char[] charArray = str.toCharArray();
for (char ch : charArray) {
  System.out.println(ch);
}

// char -> String
// String.valueOf()を使用する
charArray[1] = 'a';
String newStr = String.valueOf(charArray);
System.out.println(newStr);
```

###StringとIntegerの相互変換

```swift
// 文字列 -> 整数値
int val1 = Integer.parseInt("123");

// 整数値 -> 文字列
String str1 = String.valueOf(100);

System.out.printf("%d %s\n",val1, str1);
```

### StringBuffer
<!-- stringbuffer:: -->
簡単に言うと値(文字列)の変更が可能なString。
Stringは文字を１つ追加したり削除したりした場合でも、別にString型のオブジェクトを生成する必要がある。StingBufferはある程度はメモリを使い回すので頻繁に編集するような文字列を管理するのに向いている。
  
詳細はこちら([StringBufferのメソッド](http://www.javaroad.jp/java_character5.htm))  

```swift
// 文字列を追加する append
StringBuffer strbuf3 = new StringBuffer("hoge");
strbuf3.append(" hoge!");
System.out.println(strbuf3);

strbuf3.append(100);
System.out.println(strbuf3);

// 指定位置を削除する delete(int:start, int:end);
strbuf3.delete(1,3);
System.out.println(strbuf3);

// 文字列（その他）を挿入する insert(int:start, String:addStr);
StringBuffer strbuf4 = new StringBuffer("01234567890");
strbuf4.insert(0, "hoge_");
System.out.println(strbuf4);

// 指定範囲の文字列を置き換える replace(int:start, int:end, String:replaceStr)
StringBuffer strbuf5 = new StringBuffer("01234567890");
strbuf5.replace(3,5, "_hoge_");
System.out.println(strbuf5);

// バッファ数を返す capacity()
System.out.println(String.valueOf(strbuf2.capacity()));
```

###エスケープシーケンス

|エスケープシーケンス|意味|
|---|---|
|¥t | Tab
|¥n | 改行
|¥r | 復帰(カーソルが行の先頭に移動)
|¥f | 改ページ
|¥' | シングルクオーテーション
|¥" | ダブルクオーテーション
|¥¥ | ¥文字