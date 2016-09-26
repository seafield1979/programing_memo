#Java クラス定義

~~~java
class クラス名{  
  メンバ変数定義  
  コンストラクタ  // クラスメイト同じ名前のメソッド  
  ファイナライザ  // オブジェクト解放時に呼ばれるメソッド(なくてもいい)
  メソッド定義  
}  
~~~

~~~java
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