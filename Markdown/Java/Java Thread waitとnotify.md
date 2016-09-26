#waitとnotifyAll

複数のスレッドの同期方法でwait()で待機状態、notifyAll()で待機状態を解除するものがある。

|スレッド1|スレッド2|説明|
|---|---|---|
|wait()| ---|スレッド１が待ち状態になる|
|---|notifyAll()|スレッド１の待ち状態が解除される|
|処理中|処理中||
|---|wait()|スレッド2が待ち状態になる|
|nitifyAll()|---|スレッド2の待ち状態が解除される


~~~java
class WaitClass {
    // 動作モード 0: block1も2も実行中でない
    //          1: block1が実行中
    //          2: block2が実行中
    public int mode = 0;

    public synchronized void block1(String threadName) {
        // mode = 1ならblock2が実行中なので待機
        if (mode == 1) {
            try {
                System.out.println(threadName + " wait");
                wait();
                System.out.println(threadName + " awake");
            } catch (InterruptedException e) {

            }
        }
        try {
            System.out.println("block1");
            Thread.sleep(200);
        } catch (InterruptedException e) {

        }
        mode = 1;
        notifyAll();
    }

    public synchronized void block2(String threadName) {
        if (mode != 1) {
            try {
                System.out.println(threadName + " wait");
                wait();
                System.out.println(threadName + " awake");
            } catch (InterruptedException e) {

            }
        }
        try {
            System.out.println("block2");
            Thread.sleep(200);
        } catch (InterruptedException e) {

        }
        mode = 2;
        notifyAll();
    }
}

class WaitThread1 extends Thread {
    WaitClass wc;
    String name;
    int type;

    WaitThread1(WaitClass wc, int type) {
        this.wc = wc;
        this.name = "Thread" + String.valueOf(type+1);
        this.type = type;
    }

    public void run() {
        for (int i=0; i<10; i++) {
            if (type == 0) {
                wc.block1(this.name);
            } else if (type == 1) {
                wc.block2(this.name);
            }
        }
    }
}
~~~