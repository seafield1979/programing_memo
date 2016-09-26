###Java コンストラクタ
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