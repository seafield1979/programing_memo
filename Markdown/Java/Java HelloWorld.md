#Java Hello World
コンソール画面に"Hello World!!"とだけ表示するプログラムを作ってみる。

```java:helloworld.java
class Test1{
  public static void main(String args[]){
    System.out.println("hello Test1");
  }
}
```

helloworld.javaを作成したら `javac` でコンパイル後`java`でclassを実行する

```sh:shell
#javacでコンパイルすると .classファイルが作成される(HelloWorld.class)
$javac helloworld.java

#クラス名で実行
$java HelloWorld
```