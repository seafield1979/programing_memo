#Java 乱数

###Random nextInt
RandomクラスのオブジェクトのnextIntメソッドで最大値を指定して整数の乱数を取得する

```java
import java.util.Random;
public class Ran{
    public static void main(String[] args){

        //Randomクラスのインスタンス化
        Random rnd = new Random();

        int ran = rnd.nextInt(10);
        System.out.println(ran);
    }
}
```

###Math.random
Math.random()  
0~1.0の値を返す

```java
int val = Math.random();
```

