#Thread の実行
Threadを使ってバックグラウンドで処理を実行する。

2つのThreadを同時に実行する

Threadクラスのメソッド

|メソッド|説明|
|---|---|


Threadで処理を行いたい場合は以下の方法がある

1.Threadクラスのサブクラスを作る
2.Runnableインターフェイスを持つクラスを元にThreadオブジェクトを作成

スレッドを動かすのに必要な処理は以下

###Threadのサブクラス
1.  Thread クラスを継承したクラス
2. 1のクラスでrunメソッドを実装
3. 1のクラスのオブジェクトを生成
4. 3のオブジェクトからstartメソッドを実行

```java
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

###Runnableインターフェース
Runnableインターフェースを持つクラスを元にThreadを立ち上げる

1. Runnableクラスを継承したクラスを作成
2. 1のクラスでrun()メソッドを実装
3. メインスレッドでMyRunnableオブジェクトを作成、これを元にThreadオブジェクトを作成
4. Threadオブジェクト のstart()でスレッド開始

```java
class MyRunnable implements Runnable {
    final int INTERVAL_PERIOD = 1000;
    private int mCount = 0;
    private boolean running = true;

    // Threadの処理
    public void run() {
        System.out.println("thread1 start");
        while(running) {
            System.out.println("count " + mCount);
            mCount++;
            try {
                Thread.sleep(INTERVAL_PERIOD);
            } catch (InterruptedException e) {
            }
        }
    }

    // 停止要求
    public void stopRequest() {
        running = false;
    }
}

public class TestThread {
  public static void main() {
    MyRunnable runnable = new MyRunnable();
    Thread thread1 = new Thread(runnable);
    Thread thread2 = new Thread(runnable);
    thread1.start();
    thread2.start();
  }
}
```

