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

###各値の文字列を返すEnum

```java
enum EnumStr {
    TYPE1("AAA"),
    TYPE2("BBB"),
    TYPE3("CCC"),
    ;

    private final String text;

    private EnumStr(final String text) {
        this.text = text;
    }

    public String getString() {
        return this.text;
    }
}

// 使用例
EnumStr es1 = EnumStr.TYPE1;
EnumStr es2 = EnumStr.TYPE2;
EnumStr es3 = EnumStr.TYPE3;

System.out.println("es1=" + es1.getString());  // es1=AAA 
System.out.println("es2=" + es2.getString());  // es2=BBB
System.out.println("es3=" + es3.getString());  // es3=CCC
```

###全要素をforで回す

```java
// その１ ただのEnum
enum DAY {
    MON,
    TUE,
    WED,
    THU,
    FRI,
    SAT,
    SUN
}
for (DAY day : DAY.values()) {
    System.out.println("num:" + day.ordinal() + " " + day.toString());   
    // num:0 MON
    // num:1 TUE
    // num:2 WED
    // num:3 THU
    // num:4 FRI
    // num:5 SAT
    // num:6 SUN
}

// その２ 値を持つEnum
enum EnumStr {
    TYPE1("AAA"),
    TYPE2("BBB"),
    TYPE3("CCC"),
    ;

    private final String text;

    private EnumStr(final String text) {
        this.text = text;
    }

    public String getString() {
        return this.text;
    }
}

// forループで全要素にアクセス
for (EnumStr es : EnumStr.values()) {
    System.out.println("num:" + es.ordinal() + " " + es.getString());
}
```