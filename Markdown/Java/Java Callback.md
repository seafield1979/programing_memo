#コールバック Callback

Javaでコールバックを実現するにはInterfaceを使って実装するのが一般。
Cのコールバックに比べると非常に複雑。

```java
class TestCallback implements MyClassCallbacks{
    public static void main(String args[]) {
      TestCallback m = new TestCallback();
      
      // Test1クラスでコールバックメソッド(callbackMethod)を呼んでもらう
      Test1 test1 = new Test1();
      test1.setCallbacks(m);
      test1.method();
    }

    // MyClassCallbacks のメソッドを実装
    public void callbackMethod() {
        System.out.println("finished!!");
    }
}

// コールバック用のインターフェース
// コールバックを使用するクラスにimplementする
interface MyClassCallbacks {
    public void callbackMethod();
    public void callbackMethod2();
}

// コールバックを呼び出すクラス
// setCallbacks でインターフェースを実装したクラスを受け取る
class Test1 {
    private MyClassCallbacks _myClassCallbacks;

    public void setCallbacks(MyClassCallbacks myClassCallbacks){
        _myClassCallbacks = myClassCallbacks;
    }

    public void method(){
        System.out.println("1");
        System.out.println("2");
        System.out.println("3");
        System.out.println("4");
        System.out.println("5");
        System.out.println("6");
        // ここでコールバック呼び出し
        _myClassCallbacks.callbackMethod();
    }
}
```