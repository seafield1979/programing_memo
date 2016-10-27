#Thread Runnableインターフェイス

Threadの処理をRunnableインターフェイスを実装して実現

必要な手順は以下

1. スレッド実行したいクラスにRunnableインターフェイスを実装(implements)
2. 1のクラスにスレドッジ実行時に呼ばれるrunメソッドを実装
3. 1のクラス(Runnableインターフェイス実装)のオブジェクトを生成
4. Threadオブジェクトのコンストラクタに3のオブジェクトを渡して生成
5. 4のThreadオブジェクトの startメソッドを呼び出す

```java
/**
 * Runnableインターフェースをimplementsしたクラスでrunメソッドに処理を記述する
 */
class MyRunnable implements Runnable {
    public void run() {
        for (int i=0; i<100; i++) {
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
            }
            System.out.println("Runnable run " + String.valueOf(i));
        }
    }
}


class TestThread {
    public static void main(String[] args) {
        System.out.println("Thread Test");
        // RunnableのオブジェクトをThreadクラスのコンストラクタに渡す
        MyRunnable runnable = new MyRunnable();
        Thread thread1 = new Thread(runnable);
        Thread thread2 = new Thread(runnable);
        
        // Threadオブジェクトのstartメソッドを呼ぶ
        thread1.start();
        thread2.start();
    }
}
```

出力

```sh
Runnable run 1
Runnable run 2
Runnable run 3
...
Runnable run 99
```