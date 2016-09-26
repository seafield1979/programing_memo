#Java パッケージ package

パッケージはファイルをフォルダ階層に配置してまとめる仕組み。パッケージを使用するとクラス名やファイル名の衝突が回避される。ただし、異なるパッケージに含まれる同じクラス名がかぶっている場合、それらのパッケージを同時にimportするとクラス名がかぶるのでNG。

パッケージの特徴

**パッケージ名と同じフォルダ階層を作り、クラスを配置する**  
`com.sunsunsoft.Hoge.Calc`というパッケージを作る場合は、フォルダ構成は  
`/com/sunsunsoft/Hoge/Calc.java`  
となる。この com 以下フォルダを jar でパックして SunSunHoge.jar のようなjarファイルを作成する。

**複数のパッケージに含まれる同名のクラスをインポートした場合、古いクラスは後から読み込まれたパッケージのクラスに上書きされる**  
MyPacakge1/Hoge というパッケージと  
MyPackage2/Hoge というパッケージがあって、これらを 

~~~
import MyPackage1/Hoge;
import MyPackage2/Hoge;
~~~
のようにインポートした場合、MyPackage1/Hogeクラスは MyPackage2/Hoge に上書きされimport先のクラスでHogeクラスを使用した場合、MyPackage2のHogeが呼び出される。


**パッケージはパッケージのトップフォルダをjarファイルにパックできる** 
パッケージをパックしたjarを特定のフォルダに配置すると、importして使用できるようになる

jarコマンドで

~~~sh
# jar cf <jarファイル名> <パッケージのトップフォルダ>
$ jar cf MyPackage.jar MyPackage
~~~
のようにフォルダjarファイルに圧縮すると、このjarに含まれるパッケージやクラスを外部から使用出来るようになる。


####パッケージの利用方法

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

~~~java:Hoge1.java
// パッケージ名を指定する。パッケージのトップからのパス
// /MyPackage 以下なので　MyPackage
package MyPackage;

public class Hoge1 {
  public void test1() {
    System.out.println("Hoge1 test1()");
  }
}

~~~

~~~java:Hoge2.java
// パッケージ名を指定する。パッケージのトップからのパス
// /MyPackage/AAA 以下なので　MyPackage.BBB
package MyPackage.AAA;

public class Hoge2 {
  public void test1() {
    System.out.println("Hoge2 test1()");
  }
}

~~~java:Hoge3.java
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

###実行可能なjarファイルを作成する
Javaから実行可能なjarを作成する方法。実行可能なjarを作成するにはjarの中にpublicなmainメソッドを持ったクラスをパックして、そのクラス名をマニフェストファイルに記述する。

以下のようなファイルを作成

~~~
Test1.java    jarが実行された時に呼ばれるクラス
MANIFEST.MF   マニフェストファイル。jar実行時に呼ばれるクラス名を記述する
MyPackage/Hoge.java   パッケージファイル
~~~

~~~java:Test1.java
import MyPackage.Hoge;

public class Test1 {
    public static void main(String[] args) {
        System.out.println("Hello Runable Package!!");

        Hoge hoge1 = new Hoge();
        hoge1.test1();
    }
}
~~~


~~~java:Hoge.java

// Test1で使用されるパッケージクラス
package MyPackage;

public class Hoge{
    public void test1(){
        System.out.println("MyPackage.Hoge1.test1()");
    }
}
~~~

~~~MANIFEST.MF
# マニフェストファイル
# Main-Class に実行されるクラス名(Test1)を指定する
Manifest-Version: 1.0
Created-By: 1.8.0_101 (Oracle Corporation)
Main-Class: Test1
~~~

コンパイル＆jar圧縮

~~~sh
#!/bin/bash
# コンパイル
$ javac Test1.java
$ javac MyPackage/Hoge.java

# jar に圧縮
# f オプションで出力するjarファイルを指定
# m オプションでマニフェストファイル(MANIFEST.MF)を指定
$ jar cvfm Runable.jar MANIFEST.MF Test1.class MyPackage
~~~

これで Runable.jar が生成されるので  
jar ファイルを実行する

~~~sh
# java コマンドを -jar オプションで実行
$ java -jar Runable.jar
  Hello Runable Package!!
  MyPackage.Hoge1.test1()
~~~