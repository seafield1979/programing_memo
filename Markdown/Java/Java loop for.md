#Loop for

C言語のfor分と同じ。配列の要素数分のループを回すのに for(変数 : 配列) の構文が使える

```swift
  // 配列の要素数でループを回す
  for (int i=0; i<10; i++) {
      System.out.println("count: " + String.valueOf(i));
  }

  // 配列の要素数分のループを回す
  int[] array = {1,2,3,4,5,6,7,8,9,10};
  for (int i=0; i<array.length; i++) {
      System.out.printf("array[%d]=%d\n", i, array[i]);
  }
  
  もしくは
  
  for (int val : array) {
  	  System.out.printf("value=%d\n", val);
  }

  // プログラムの引数(argv[])を表示する
  for (index = 0; index < args.length; ++index) {
      System.out.println("args[" + index + "]: " + args[index]);
  }

  もしくは

  for (String s : args) {
      System.out.println(s);
  }
```

