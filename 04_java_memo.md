<!-- Stylesheet -->
<link href="http://sunsunsoft.com/css/hoge.css" rel="stylesheet"></link>

<span class="title">Javaプログラミング</span>

<!-- もくじ -->
[TOC]

Javaのメモ








#環境構築
##windows
  jdkをインストールして、インストールしたjdk以下のbinフォルダにパスを通す
  　環境変数のPathに "C:\Program Files\Java\jdk1.8.0_25\bin"　を追加する。
  これでコマンドラインから java や javac が実行できるようになる

##bash on Windows
※ [Qiitaの記事](http://qiita.com/sndr/items/f080fb4141c45c51bdb9)を参考にjavaをインストールしてみたがjavacが動かない。Windows10 bash では javaは使えないようだ。


#Hello World
コンソール画面に"Hello World!!"とだけ表示するプログラムを作ってみる。

~~~java
- helloworld.java -
class Test1{
  public static void main(String args[]){
    System.out.println("hello Test1");
  }
}
~~~~

helloworld.javaを作成したら `javac` でコンパイル
~~~sh
#javacでコンパイルすると .classファイルが作成される(HelloWorld.class)
$javac helloworld.java

#クラス名で実行
$java HelloWorld
~~~


#文字列出力 print
標準出力(コンソール)に文字列を出力する。

###print
print 文字列出力(改行なし)
println 文字列出力(改行あり)
printf 指定したフォーマットで文字列出力(改行なし)

~~~java
System.out.print("hoge1");  // hoge1
System.out.println("hoge2");  // hoge2\n
String str1 = "abc";
System.out.printf("%s %d hoge", str1, 100); // abc 100 hoge 
// フォーマットした文字列を変数に代入するには String.format を使用する
String str2 = String.format("%s %d hoge2", str1, 100);

~~~

#基本 basic
<!-- basic:: -->
###コメント
C言語と同じで  
単一行のコメントは //  
ブロックのコメントは /* ~ */  
~~~java
// コメントあうと!
hoge = 100;  // 行の途中からコメントアウト！
/*　グロックコメントアウト */
/*
  ブロックコメントは複数行もOK
 */
~~~

###予約語

|予約語|内容|
|---|---|
true | true(真)を表す。boolean型の変数やif文等の条件式に使用できる
false | false(偽)を表す。boolean型の変数やif文等の条件式に使用できる
null | NULL(値なし)を表す。オブジェクト変数等に代入できる


###エンコーディング
<!-- encoding -->
ソース中に日本語を使用している場合、その日本語がどんな文字コードでエンコードされているかを認識しておく必要がある。もしコンパイルしてできたjavaの文字コードとそれを実行する環境の文字コードがずれていると、正しく日本語が表示されない。

####javaファイルのエンコーディングを確認
~~~java
System.out.println(System.getProperty("file.encoding"));  // UTF-8 など
~~~

####今コーディングを指定してコンパイル
~~~sh
javac -encoding エンコーディング名 ソースファイル

java -encoding UTF-8 hoge.java
java -encoding SJIS hoge2.java   // Mac環境だと日本語の行で"この文字は、エンコーディングSJISにマップできません"というコンパイルエラー
~~~

主なエンコーディング

|エンコーディング|説明|
|---|---|
UTF-8 UTF8 | 8 ビット Unicode (UCS) Transformation Format
EUC-JP  EUC_JP | JISX 0201、0208、0212、EUC エンコーディング、日本語
Shift_JIS SJIS | Shift-JIS、日本語

#文字列 string
<!-- string:: -->
Javaでは文字列変数はString型で管理される。文字列リテラルはC言語と同じchar型(16ビットのUnicode)が使われる。

~~~java
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

~~~

###Stringとchar配列の相互変換
[[Java] String → char[], char[] → String への変換](http://kadoppe.com/archives/2011/03/java-string-char-conversio.html)

~~~java
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
~~~

###StringとIntegerの相互変換
~~~java
// 文字列 -> 整数値
int val1 = Integer.parseInt("123");

// 整数値 -> 文字列
String str1 = String.valueOf(100);

System.out.printf("%d %s\n",val1, str1);
~~~

###エスケープシーケンス

|エスケープシーケンス|意味|
|---|---|
¥t | Tab
¥n | 改行
¥r | 復帰(カーソルが行の先頭に移動)
¥f | 改ページ
¥' | シングルクオーテーション
¥" | ダブルクオーテーション
¥¥ | ¥文字

#クラス class
クラスはプロパティ（変数）とメソッド（関数）をセットにしたもの。

Javaのクラスの特徴

* デストラクタがない。代わりにファイナライザがある。が、いつ呼ばれるかわからない(GCにお任せ)
* デストラクタがなくファイナライザも期待できないため、デストラクタ的なメソッドを自前で用意して呼ばなくてはならない。

###クラス定義

~~~java
class クラス名{  
  メンバ変数定義  
  コンストラクタ  // クラスメイト同じ名前のメソッド  
  ファイナライザ  // オブジェクト解放時に呼ばれるメソッド(なくてもいい)
  メソッド定義  
}  

例:
class Calc{
    // プロパティ
    int result;
    private float mult;

    // コンストラクタ
    Calc() {
      mult = 1.0;
    }
    Calc(float _mult) {
      mult = _mult;
    }

    // ファイナライザ
    @Override
    protected void finalize() throws Throwable {
      try {
        super.finalize();
      } finally {
        destruction();
      }
    }

    // デストラクタ
    private void destruction() {
      System.out.println("destruction");
    }

    // メソッド定義
    void plus(int val1, int val2){
      result = val1 + val2 * self.mult;
    }
    void disp(){
      System.out.println("result=" + result);
    }
  }
  class Test1{
    public static void main(String args[]){
      System.out.println("hello Test1");
    }
  }
~~~

###オブジェクト生成
クラスの定義を元にオブジェクトを生成する。

**変数宣言**  
クラス変数の宣言方法。普通の変数の型の部分がクラス名になる
~~~java
// クラス変数を定義(オブジェクト生成はまだ)
クラス名 変数名;

例:
HogeClass hoge1;
public static HogeClass hoge2;
~~~

**オブジェクト生成**
~~~java
// クラスオブジェクトを生成し、変数に代入
クラス名 変数名 = new クラス名();  

例:
HogeClass hoge1 = new HogeClass();
public static HogeClass hoge2 = HogeClass();
~~~

###プロパティ
[アクセス修飾子] [static] 型 変数名;

~~~
例
int val1;
int val12 = 100;
static int val2;  // インスタンスなしでアクセス可能
public static int val3;  // クラス外部からインスタンスなしでアクセス可能
~~~

**アクセス修飾子**  

[Javaのアクセス修飾子―public、protected、private、デフォルトなど](https://www.zealseeds.com/Lang/LangJava/BasicGrammar/AccessQualifier/index.html)

|アクセス修飾子|説明|
|---|---|
省略 | 修飾子を省略した場合は、同じパッケージ内のクラスからアクセスできる。<br>パッケージ限定のpublic。
public | クラス外からアクセス可能。省略時に
protected | 自クラスと継承したクラスからアクセス可能
private | 自クラスからアクセス可能

アクセスレベル表

|アクセス修飾子|クラス|サブクラス|パッケージ|その他すべて|
|---|---|---|---|---|
private | ○ | × | × | ×
デフォルト | ○ | × | ○ | ×
protected | ○ | ○ | ○ | ×
public | ○ | ○ | ○ | ○ | ○


**型**  
型にはデータ型（プリミティブ型）とクラスが指定可能。

|データ型|説明|
|---|---|
boolean | true or false
char | 16ビットUnicode文字 ¥u0000～¥uFFFF
byte | 8ビット整数 -128～127
short | 16ビット整数 -32768～32767
int | 32ビット整数 -2147483648～2147483647
long | 64ビット整数 -9223372036854775808～9223372036854775807
float | 32ビット単精度浮動小数点数
double | 64ビット倍精度浮動小数点数


###メソッド
[アクセス修飾子] [static] 戻り値  名前 (型 引数1,型 引数2,...){  
  処理  
  [return 戻り値];
}  

アクセス修飾子はプロパティと同じ  
staticをつけるとクラスメソッドとなって Hoge.func1 のようにクラス名.メソッド名でアクセスできる  

~~~java
//例
public int calc1(int a, int b) {
  System.out.println("%d + %d", a, b);
  return a+b;
}

// 呼び出し
int ret1 = calc1(100,200);
~~~

###コンストラクタ
<!-- construct:: constructor:: -->
new等でオブジェクトが生成されるときに呼ばれるメソッド。プロパティの初期化やクラスで使用するリソースの読み込み等を行う。
コンストラクタの名前はクラス名と同じ。引数なしのコンストラクタをデフォルトコンストラクタと呼ぶ。

####Javaコンストラクタの特徴

* コンストラクタを自前で定義しない場合、プロパティを自動で初期化してくれる。<br>値変数は0、オブジェクト変数はnull。
* 実際に使用する引数のコンストラクタを用意すればOK。引数ありコンストラクタを定義してそれだけ使用する限りは、引数なしコンストラクタを定義しなくてもOK。
* コンストラクタの中で別のコンストラクタを呼べる。this("hoge",100) のようにthisをつかう。
* 子クラスの先頭で親クラスのコンストラクタが自動で呼ばれる。が、super()やthis()のよう明示的にコンストラクタを呼んでいる場合は呼ばれない。
* 親クラスのコンストラクタは自動で呼ばれるので、自分で呼ぶ必要がない

####コンストラクタサンプル
サンプル
~~~java
class Hoge{
  private String name;
  private int age;

  // 引数なしコンストラクタ
  Hoge(){
    // プロパティを初期化
    this.name = null;
    this.age = 0;

    // 引数ありのコンストラクタを呼ぶ場合
    // this("hoge", 100);
  }
  // 引数ありコンストラクタ
  Hoge(String name, int age) {
    this.name = name;
    this.age = age;
  }
}

Hoge hoge1 = new Hoge();
hoge hoge2 = new Hoge("taro", 14);

class HogeSub extends Hoge {
  HogeSub(){
    // ここで自動で super() が呼ばれている。ただし、super()やthis(引数)を自前で呼んでいる場合は呼ばれない
  }

  HogeSub(String name) {
    this.name = "sub:" + name;
  }

  // dispをオーバーライド
  public void disp() {
    // super で親クラスのメソッドを呼び出せる
    super.disp();
  }
}

~~~

###ファイナライザとデストラクタ
ファイナライザはGC(ガベージコレクション)がオブジェクトを解放する時に呼ばれるメソッド。通常はJavaがデフォルトのファイナライザを自動に呼んでくれるが、自前の処理を行いたい場合はオーバーライドする。  
ただ、ファイナライザはGCが呼ばれた時に自動で呼ばれるものなのでいつ呼ばれるかわからない。オブジェクトが不要になった場合にリソースを解放したい場合などは自前のデストラクタを用意してそれを呼ぶようにする。

~~~java
public class Test1{
  @Override
  protected void finalize() throws Throwable {
    try {
      super.finalize();
    } finally {
      destruction();
    }
  }
  private void destruction() {
    System.out.println("destruction");
  }
}
~~~

####オーバーライド override
<!-- override:: -->
overrideの元々の意味。
~~~
1〈命令・権利などを〉無視する，受け付けない; 〈決定などを〉くつがえす，無効にする.
~~~
  
Javaでは親クラスで定義されたメソッドを子クラスでオーバーライド(override)することができる。このとき、処理が変わるのはオーバーライドした子クラスのメソッドのみ。親クラスのメソッドはそのまま。

オーバーライドするには子クラスで親クラスと同名のメソッドを定義するだけで良いが、オーバーライドされたメソッドされたということを強調するために `@Override` をつけることもできる。
~~~java
@Override public void hoge();
~~~

サンプル
~~~java
class Hoge {
  public void hoge() {
    System.out.println("hoge");
  }
} 

class HogeSub extends Hoge {
  @Override
  public void hoge() {
    System.out.println("hoge hoge");
  }
}

HogeSub sub1 = HogeSub();
sub1.hoge();    // hoge hoge

~~~

###継承
親クラスを継承した子クラスを定義できる。子クラスは親クラスのプロパティ、メソッドをそのまま引き継ぐ。ただしprivate修飾子がついたプロパティやメソッドは子クラスから使用できない。

~~~java
// extends で継承元のクラスを指定する
class HogeSub extends Hoge {

  int calc2(int a, int b) {
    // 親クラスのメソッドを呼び出すには super.メソッド名
    int val = super.calc(a, b);
    return val + 10000;
  }
}
~~~

####Javaの継承の特徴

* ほぼC++と同じ
* 継承できる親クラスは一つだけ（単一継承）、多重継承はできない。
* 多重継承できない代わりに Interface で複数の機能セットを引き継いだクラスは作れる。

###抽象クラス
<!-- abstract:: -->
抽象クラスはオブジェクトが作れないクラス。抽象クラスを継承した先で抽象メソッド(abstractがついたメソッド)を定義して使用する。abstract メソッドを１つでも持っていたら抽象クラスになる。

~~~java
// 抽象クラス classの前に abstract をつける
abstract class AbstractHoge {
  private String name;

  abstract public void hoge();
  abstract public void hoge2(String name);
}
// 実装クラス
class HogeImpl extends AbstractHoge{ 

    // 親クラスの abstract メソッドの実態を定義する
    public void hoge() {
        System.out.println("impl hoge");
    }
    public void hoge2(String name) {
        this.name = name;
        System.out.println(this.name);
    }
}
~~~

###インターフェース
<!-- interface:: -->
抽象クラスしか持たないクラス（のようなもの）をインターフェースと呼ぶ。インターフェースからオブジェクトは作れない。

**Java Interfaceの特徴**

* インターフェースはプロパティを持てない
* 定数は持てる。
* 多重継承がない代わりに、インターフェースを使って複数の機能のセットを持つクラスを作成することができる。
* インターフェースを取り込んだ先のクラスでインターフェースのメソッドを全て定義しないとエラーになる

~~~java
// インターフェースの定義例
public interface InterfaceHoge {
  // 定数 (static)
  int HOGE_MAX = 256;

  void hoge();
  int calc();
}

// Interfaceを実装したクラス
class InterfaceHogeImpl implements InterfaceHoge{
    public void hoge() {
        System.out.printf("inpl hoge %d\n", InterfaceHoge.HOGE_MAX);
    }
    public int calc(int a, int b) {
        System.out.printf("calc: %d\n", a + b);
        return a + b;
    }
}




#インポート import
<!-- import -->

###同じ階層にあるclassファイルを使用する方法
同じ階層にあるclassファイルとそのclassファイルで定義されたクラスを利用する方法。

~~~java
- Test1.java -
class Test1 {
    public static void main(String args[]) {
        // 別のjavaファイルから生成したImported1クラスを使用する
        // Imported1.class の test1()を実行
        Imported1 import1 = new Imported1();
        import1.test1();
    }
}
~~~

呼び出されるクラス
~~~java
- Imported1.java - 
class Imported1 {
    public void test1() {
        System.out.println("Imported1 test1!");
    }
}
~~~

Imported1.java と Test1.java をコンパイル後、Test1 を実行すると、Imported1 のオブジェクト生成され、コンソールに "Imported1 test1!" と表示される。


#パッケージ package
<!-- package:: -->

パッケージはファイルをフォルダ階層に配置してまとめる仕組み。パッケージを使用するとファイル名の衝突が回避される。

パッケージの利用方法。


試しにパッケージを作ってみる。どこでもいいので以下のようなフォルダ構成でファイルを配置する。
~~~

- TestPackage.java   パッケージをテストするクラス
+ MyPackage  パッケージのトップフォルダ
  - Hoge1.java      Hoge1クラス
  + AAAフォルダ
    - Hoge2.java    Hoge2クラス
  + BBB フォルダ
    - Hoge3.java    Hoge3クラス
~~~

各javaファイルの中身
~~~java
- Hoge1.java
// パッケージ名を指定する。パッケージのトップからのパス
// /MyPackage 以下なので　MyPackage
package MyPackage;

public class Hoge1 {
  public void test1() {
    System.out.println("Hoge1 test1()");
  }
}

- Hoge2.java -
// パッケージ名を指定する。パッケージのトップからのパス
// /MyPackage/AAA 以下なので　MyPackage.BBB
package MyPackage.AAA;

public class Hoge2 {
  public void test1() {
    System.out.println("Hoge2 test1()");
  }
}

- Hoge3.java -
// パッケージ名を指定する。パッケージのトップからのパス
// /MyPackage/BBB 以下なので　MyPackage.BBB
package MyPackage.BBB;

public class Hoge3 {
  public void test1() {
    System.out.println("Hoge3 test1()");
  }
}

- TestPackage.java -
import MyPackage.Hoge1;
import MyPackage.AAA.Hoge2;
import MyPackage.BBB.Hoge3;

class TestPackage {
  public static void main(String args[]) {
    // 各パッケージのメソッドを呼び出してみる
    // ※importされたパッケージはパッケージのパスを指定しないで呼び出せるようになる
    Hoge1 hoge1 = new Hoge1();
    hoge1.test1();

    Hoge2 hoge2 = new Hoge2();
    hoge2.test1();

    Hoge3 hoge1 = new Hoge3();
    hoge3.test1();
  }
}

~~~

ファイルを作成したら、コンパイル後、TestPackageクラスを実行する
~~~sh
cd <MyPackageフォルダがある階層>
$ javac TestPackage.java
$ javac MyPackage/Hoge1.java
$ javac MyPackage/AAA/Hoge2.java
$ javac MyPackage/BBB/Hoge3.java

$ java TestPackage
  Hoge1 test1()
  Hoge2 test2()
  Hoge3 test3()
~~~

###パッケージをjarファイルにパックする
パッケージを使うためにパッケージのフォルダごとコピーするのは面倒臭いので、jarファイルにパッケージをパックして１ファイルで扱えるようにする。このjarファイルをクラス検索パス(Macの場合は ~/Library/Java/Extension/以下)に配置すると、java実行時にimportのパッケージを検索してくれる。

自前のjarをimportするための手順は以下の通り

1. パッケージをjarファイルにパックする
2. 1で作成したjarファイルを検索パスに配置する
3. javaで1のjarに含まれるクラスをimportする

**1.jarに圧縮する**  
jarコマンドを使用する
~~~sh
$ jar cf MyPackage.jar MyPackage
~~~
これで `MyPackage.jar`が作成される

**2.検索パスに配置する**  
Macの場合  
~/Library/Java/Extension 以下に1で作成したMyPacakge.jar をコピーする。  
※~/Library/Java/Extension は最初は存在しないので、フォルダを作成する。

**3.javaでjarに含まれるクラス(パッケージ)をimportする**  
~~~java
// ここでjarに含まれるパッケージのクラスをimport 
import MyPackage.Hoge1;

class TestPackage {
  public static void main(String[] args) {
    // MyPackage.Hoge1 を Hoge1 という名前で使用出来る
    Hoge1 hoge1 = new Hoge1();
    hoge1.test1();
  }
}
~~~

~~~sh
# MyPackage以下のファイルを圧縮して MyPackage.jar を出力する
$ jar cf MyPackage.jar MyPackage 
~~~

#jarファイル
classファイルをjarにパッケージすることで、プログラムを１ファイルにまとめることができる。

jarファイルに圧縮
~~~sh
$ jar cvf pack.jar pack
~~~

jarファイルを解凍
~~~sh
$ jar xvf pack.jar
~~~

jarファイルにパッケージされたファイルを確認する
~~~sh
$ jar tf pack.jar
~~~

詳細  
jar {ctxu}[vfm0Mi] [jar-file] [manifest-file] [-C dir] files ...  

|オプション|内容|
|:--:|---|
-c | アーカイブを新規作成する
-t | アーカイブの内容を一覧表示する
-x | 指定の (またはすべての) ファイルをアーカイブから抽出する
-u | 既存アーカイブを更新する
-v | 標準出力に詳細な出力を生成する
-f | アーカイブファイル名を指定する
-m | 指定のマニフェストファイルからマニフェスト情報を取り込む
-0 | 格納のみ。ZIP 圧縮を使用しない
-M | エントリのマニフェストファイルを作成しない
-i | 指定の jar ファイルのインデックス情報を生成する
-C | 指定のディレクトリに変更し、以下のファイルを取り込む

#コーディング規約 coding rule
<!-- coding rule:: -->
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
<!-- abstract:: -->
抽象クラス名に適当な名前が無いとき，Abstract から始まりサブクラス名を連想させる名
前を付ける．
~~~java
class AbstractBeforeSubClassName {}
~~~

####実装クラス名
<!-- implimentation:: -->
特に interface と区別の必要があれば，最後に Impl を付ける．
~~~java
class ClassNameEndsWithImpl{}
~~~


###インターフェイス名
クラス名に同じ．ただし，class と区別の必要があれば，最初に I を付ける．
~~~java
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


#その他 others

###プログラム引数
プログラムに渡された引数は、mainメソッドに渡される引数はargsで参照できる。

~~~sh
# javaの呼び出し部分
java Hoge 引数1 引数2 引数3 の場合
~~~
の場合
Hoge.main() の args は ["引数1", "引数2", "引数3"] 配列になる。
※args[0]が全引数文字列のC言語と異なり、args[0]から一つ目の引数が代入される。


●クラス使用
　クラスのインスタンスを生成、メソッドを呼び出す方法
  クラス名 変数名 = new クラス名();  // コンストラクタが呼ばれるよ
  　例: Cacl calc = new Calc();
        calc.plus(1,2);
        calc.disp();

■InterfaceとImplements
  Javaは多重継承が出来ないため、複数の機能のセットを実装したい場合 Interfaceにメソッドの定義だけをしておいて、それらをClassにImplementsとして取り込む。
  Implementsしたインターフェイスは実装は含まないので、Implements先のクラスで実装しなくてはならない。

  ●interfaceの定義
    [修飾子] interface <インタフェース名> {
      データ型 変数名 = 値;
      修飾子 戻り値のデータ型 メソッド名(引数の型宣言);
    }
    例: 
    public interface Interface01 {
      public static int hoge = 100;
      public void test1();
      public boolean test2();
    }

  ●implements 
    例: 
    class InterfaceImpl implements Interface1, interface2, interface3 {
      // インターフェイスを実装する
    }

●配列
　配列の宣言方法
　　型 変数名[] = new 型[要素数];
    型[] 変数名 = new 型[要素数];

    int intArray[] = new int[10];
    int[] intArray = new int[10];

  初期値あり配列
    int array[] = {1,2,3,4,5};
  
  配列長を取得
  　配列変数名.length
    int array[] = new int[5];
    System.out.println(array.length);

  多次元配列
  　型名 配列変数名[][];
    配列変数名 = new 型名[要素数][];

    int seiseki[][] = new int[2][];
    int array1[] = new int[10];
    int array2[] = new int[20];
    seiseki[0] = array1;
    seiseki[1] = array2;
 
    多次元配列の初期値あり初期化
    int num[][] = {{10, 8, 5}, {9, 16, 4}, {3, 7, 5}};


#ループ loop
JavaのループはC言語と同じで for,while,do~whileしかない。foreach とか便利なものはない。

###for文
  配列の要素数でループを回す
  for (index = 0; index < args.length; ++index) {
      System.out.println("args[" + index + "]: " + args[index]);
  }

  もしくは

  for (String s : args) {
      System.out.println(s);
  }

###while文











■日記
2012/12/01
　Androidでjavaを使用するのでjavaも一緒に勉強することにした。