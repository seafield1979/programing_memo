#Java Thread の実行

Threadを使って同時に２つの処理を実行する。

2つのThreadを同時に実行する

スレッドを動かすのに必要な処理は以下

1.  Thread クラスを継承したクラス
2. 1のクラスでrunメソッドを実装
3. 1のクラスのオブジェクトを生成
4. 3のオブジェクトからstartメソッドを実行

```java:ThreadTest1.java
/**
* Threadのテスト
* 2つのThreadを同時に実行する
* Thread クラスを継承したクラスのrunメソッドにスレットとして実行したい処理を記述し、
* オブジェクトを作成後 startメソッドで実行する
*/
class ThreadClass extends Thread {
    // スレッドとして実行したい処理
    public void run() {
        for (int i=0; i<100; i++) {
            try {
                sleep(10);
            } catch (InterruptedException e) {
            }
            System.out.println("test1 " + String.valueOf(i));
        }
    }
    // 通常の処理（メインスレッド）として実行する
    public void test1() {
        for (int i=0; i<100; i++) {
            try {
                sleep(10);
            } catch (InterruptedException e) {
            }
            System.out.println("test2 " + String.valueOf(i));
        }
    }
}


class TestThread {
    public static void main(String[] args) {
        System.out.println("Thread Test");
        // Threadを実行
        ThreadClass thread1 = new ThreadClass();
        
        // startメソッドでスレッド実行
        thread1.start();
        
        // スレッドは裏で動いていているのでメインの処理を実行
        thread1.test1();
    }
}
```