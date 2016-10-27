#基本 basic


###enum
列挙型。Javaのenumはクラスなので、メンバやメソッドを持たせることができる。

```java
// デフォルトのenum
enum  Enum1 {
    HOGE1,
    HOGE2,
    HOGE3
}

// テスト
Enum1 e1 = Enum1.HOGE2;
System.out.println("e1=" + String.valueOf(e1));

// switchで値の確認
switch(e1) {
    case HOGE1:
        System.out.println("hoge1");
        break;
    case HOGE2:
        System.out.println("hoge2");
        break;
    case HOGE3:
        System.out.println("hoge3");
        break;
}

```

```java

```

###コメントアウト

```java
// コメントあうと!

hoge = 100;  // 行の途中からコメントアウト！

/*　グロックコメントアウト */

/*
  ブロックコメントは複数行もOK
 */
```

