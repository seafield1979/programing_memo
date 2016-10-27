#staticメンバ、メソッド
クラスのプロパティやメソッドの先頭にstaticをつけると、オブジェクトの生成なしで使用出来るクラスプロパティ、クラスメソッドになる。


**staticの特徴**  

* オブジェクトの生成なしで使用出来る
* クラスメソッドの中で通常のプロパティやメソッドを使用することはできない。逆は可能で、インスタンスメソッドからstaticのプロパティやメソッドを呼び出すことは可能
* オブジェクトメソッドからクラスプロパティやメソドを参照する場合は先頭に this. をつける

```swift
// staticテスト
class TestStatic {
    int age;

    TestStatic(int age){
        this.age = age;
    }

    // static プロパティ
    // クラス外からオブジェクトを生成しないで呼び出すことができる
    public static final int HOGE = 256;

    // staticメソッド
    // オブジェクトを生成しないで呼び出すことができる
    public static void test1() {
        System.out.println("TestStatic test1()");
    }

    // 通常のメソッド
    public void disp(){
        // クラスメソッドを参照するには
        System.out.printf("TestStatic:disp() %d %d\n", age, this.HOGE);
        this.test1();
    }
}

// staticプロパティ、メソッドをテスト
System.out.printf("static HOGE:%d\n", TestStatic.HOGE);
TestStatic.test1();

TestStatic static1 = new TestStatic(36);
static1.disp();
```

