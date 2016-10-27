#Enum
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

enum と Int の相互変換

```java
// enum->数値
int order = Enum1.HOGE1.ordinal();

// 数値->enum
Enum1[] values = Enum1.values();
Enum1 align = values[order];

// 同じ
assertEqual(Enum1.HOGE1, align);
```

enumにいろいろな機能を持たせる

```java
// 整数値を返すことができるEnum。ちょっと面倒臭い
enum IntEnum { 
    Int1(1),
    Int2(2),
    Int3(3),
    ;

    private final int id;

    private IntEnum(final int id) {
        this.id = id;
    }

    public int getInt() {
        return this.id;
    }
    public String getMessage() {
        return String.valueOf(this.id);
    }
}

// 使用例
System.out.println("Int1=" + IntEnum.Int1.getMessage());
System.out.println("Int2=" + IntEnum.Int2.getMessage());
System.out.println("Int3=" + IntEnum.Int3.getMessage());
```