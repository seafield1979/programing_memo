#Math 数学関数

三角関数
double cos(double a);  aはラジアン角
double sin(double a);  

```java
// ラジアン角度
public static final double RAD = 3.1415 / 180.0;

double sin90 = Math.sin(90 * RAD);
double cos90 = Math.cos(90 * RAD);

// sin
public static void test1() {
    for (int i=0; i<360; i+=10) {
        System.out.println("value:" + i + " sin:" + Math.sin((double)i * RAD));
    }
}

// cos
public static void test2() {
    for (int i=0; i<360; i+=10) {
        System.out.println("value:" + i + " cos:" + Math.cos((double)i * RAD));
    }
}

// 0->100 の値を 0->1.0->0に変換する
public static void test3() {
    for (int i=0; i<=100; i++) {
        double v1 = ((double)i / (double)100) * 180;
        System.out.println("value:" + i + " sin:" + Math.sin(v1 * RAD));
    }
}
```