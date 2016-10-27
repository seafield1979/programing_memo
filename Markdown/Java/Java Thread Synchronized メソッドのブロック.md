#Synchronized Method でブロック

複数のスレッドで１つのオブジェクトに何かしらの処理を行う場合に、タイミングによってはおかしな状況になることがある。  
例えば複数のスレッドから使用される MyData というオブジェクトがあり、このMyDataは途中で

~~~java
int val = this.total;  
val += 10;  
this.total = val;
~~~
のような計算をしている場合、想定ではこのような値になってほしい

|Thread1|Thread2|コメント|
|---|---|---|
|int val = this.total; | --- | val = 0
|val += 10; | --- | val = 10
|this.total = val; | --- | this.total = 10
|---|int val = this.total; | val = 10
|---|val += 10; | val = 20
|---|this.total = val; | this.total = 20

だがしかし、途中でスレッドが変わると以下のようになる可能性がある

|Thread1|Thread2|コメント|
|---|---|---|
|int val = this.total | --- | val = 0
|           val += 10 | --- | val = 10
|                 --- | int val = this.total | val = 0
|                 --- | val += 10 | val = 10
|                 --- | this.total = val | this.total = 10
|    this.total = val | --- | this.total = 10 

本来は計算処理が２回走ったのでtotalは２０になっていて欲しかったのが10にしかならない。

synchronized を使用すると複数のスレッドが１つのブロックやメソッドの処理を同時に行うことができないようにして、上記のような問題が起きないようにする。

使用例
1.関数単位で複数のスレッドからのアクセスを制限する。

```java
// このメソッドを処理できるのは一度に１つのスレッドだけ
synchronized　void　calcTotal（）{
  
}；


// 複数のスレッドから利用されるクラス
// 特定の処理を行っている間は他のスレッドから呼び出して欲しくないメソッドには synchronized をつけて、同時アクセスされないようにする
class SynchronizedClass {
    // synchronized をつけることでこのメソッドにアクセスできるのは常に１つのスレッド
    // public void block1() {
    synchronized public void block1() {
        System.out.print("start!");
        try {
            Thread.sleep(10);
        } catch (InterruptedException e) {

        }
        System.out.println("end!");
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
            sc.block1();
        }
    }
}

class TestThread {
    public static void main(String[] args) {
        System.out.println("Thread Test");
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
start! end!
start! end!
start! end!
...
start! end!
```

