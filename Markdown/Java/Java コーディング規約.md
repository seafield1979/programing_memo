#Java コーディング規約 coding rule

[Java コーディング標準 - オブジェクト倶楽部](http://objectclub.jp/community/codingstandard/CodingStd.pdf)
を参考にした(ほとんどコピペ)

###ファイル名
publicクラスはそのクラス名

~~~
public class Point -> Point.java
~~~

###クラス名
先頭大文字．あとは区切りを大文字．

~~~java
class CapitalizedWithInternalWordsAlsoCapitalized 
{

}
~~~

####抽象クラス名
抽象クラス名に適当な名前が無いとき，Abstract から始まりサブクラス名を連想させる名
前を付ける．

~~~java
class AbstractBeforeSubClassName {}
~~~

####実装クラス名
特に interface と区別の必要があれば，最後に Impl を付ける．

~~~java
class ClassNameEndsWithImpl{}
~~~


###インターフェイス名
クラス名に同じ．ただし，class と区別の必要があれば，最初に I を付ける

~~~java
hoge
interface INameOfInterface {}
~~~

###変数名
<!-- variable:: -->

####基本の変数名(ローカル変数やクラスのプロパティ)
最初小文字で，あとは区切りを大文字

~~~java
count
startDate
errMsg
~~~

####boolean 変数
形容詞，is + 形容詞，can + 動詞，has + 過去分詞，三単元動詞，三単元動詞 + 名詞．

~~~java
boolean isEmpty
boolean dirty
boolean containsMoreElements 
~~~

###定数名
大文字を “_” でつないだもの．
~~~java
public static const int UPPER_CASE_WITH_UNDERSCORES = 100;
~~~

###メソッド名

####基本
最初小文字で，あとは区切りを大文字

~~~java
void firstWordLowerCaseButInternalWordsCapitalized();
~~~

####get/setメソッド
原則として public なプロパティは作らないで、privateで宣言し getX/setXメソッドでアクセスする。

~~~java
// get
X x()
X getX() // JavaBeans でプロパティとして扱える(推奨)
boolean isEnabled() // JavaBeans でプロパティとして扱える(推奨) 

// set
void setX(X value) // JavaBeans でプロパティとして扱える(推奨) 
~~~

####boolean値を返すメソッド

~~~java
boolean 変数を返すメソッド
is + 形容詞，can + 動詞，has + 過去分詞，三単元動詞，三単元動詞 + 名詞．
boolean isEmpty() // JavaBeans でプロパティとして扱える(推奨)
boolean empty() // だめ！’空にする’という動詞的な意味に取れるため良くない．
boolean canGet()
boolean hasChanged()
boolean contains(Object) 
boolean containsKey(Key) 
~~~


###パッケージ名
“.” で区切られた文字．

~~~
JP.co.your.domainname.projectname
junit.framework 
~~~

####名前の対称性

クラス名，メソッド名を付ける際は，以下の英語の対称性に気を付ける．

~~~java
add/remove
insert/delete
get/set
start/stop
begin/end
send/receive
first/last
get/release
put/get
up/down
show/hide
source/target
open/close 
source/destination
increment/decrement
lock/unlock
old/new
next/previous 
~~~

###メソッド

* ファイル先頭のコメントに COPYRIGHTを入れる
* package の定義とimport の定義の間に１行の空行を入れる
* クラス定義の前に /* */ のブロックコメント
* クラス開始の { の前で改行しない
* メソッドの先頭に /* */ のブロックコメント<br>メソッドの説明<br>引数を @param ...<br>戻り値を @return ...<br>必要に応じて@important ... 等の独自のフィールドを追加 
* インデントは1タブ = 4スペース
* if文の else,else ifの行はカッコを含めて１行に記述<br>}else{, } eles if(...) {

例

~~~java
/* COPYRIGHT ...
* ...
*/
package myProject.util;
import java.util.Stack;
import java.util.Vector;
/**
 * Stack を表現するクラス.
 * オブジェクトの push, pop が可能.
 *
 * @author Kenji Hiranabe
 */
public class Stack {
  /**
   * 要素を追加する.
   * @param item 追加する要素
  */
  public void push(Object item) {
    if (itemCapacity <= itemCount) {
      // ...
    } else {
      // ...
    }
  }
  
  /**
   * 先頭要素を取得する.先頭要素は取り除かれる.
   * @return 先頭要素
   */
  public Object pop() {
     // ...
     return top;
  }
} 
~~~

###ループカウンタ
スコープ（通用範囲）が狭いループカウンタ，イテレータに i, j, k という名前をこの順
に使う

~~~java
for(int i=0; i<10; i++) {
  for(int j=0; j<10; j++) {
    処理
  }
}
~~~