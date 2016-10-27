#Synchronized Object によるブロック

オブジェクト(Stringとか)でブロックする方法。処理の中の一部分をブロックできる。

```java
synchronized (ブロックオブジェクト) {
  クリティカルセクションコード 
}；
```

```java
// 複数のスレッドから利用されるクラス
// 特定の処理を行っている間は他のスレッドから呼び出して欲しくないメソッドには synchronized をつけて、同時アクセスされないようにする
class SynchronizedClass {
    // synchronized にするオブジェクトは型は問わない
    public static String lockKey = "ロックキー";

    // synchronized オブジェクトでブロックするパターン
    public void synchronizedBlock() {
        synchronized (lockKey) {
            System.out.print("start 2!");
            try {
                Thread.sleep((int)(Math.random()*10));
            } catch (InterruptedException e) {

            }
            System.out.println("end 2!");
        }
    }
}

class SynchronizedThread1 extends Thread {
    SynchronizedClass sc = null;

    // コンストラクタで共有のSynchronizedClassオブジェクトを取得する
    SynchronizedThread1(SynchronizedClass sc) {
        this.sc = sc;
    }
    
    public void run() {
        for (int i=0; i<100; i++) {
            sc.synchronizedBlock();
        }
    }
}


class TestThread {
    public static void main(String[] args) {
        System.out.println("Thread Test");
        
        // スレッドを２つ実行
        SynchronizedClass sc = new SynchronizedClass();
        SynchronizedThread1 thread1 = new SynchronizedThread1(sc);
        SynchronizedThread1 thread2 = new SynchronizedThread1(sc);
        thread1.start();
        thread2.start();
    }
}
```

出力結果。最後まで start! end! が順に表示される。

```sh
start2! end2!
start2! end2!
start2! end2!
...
start2! end2!
```