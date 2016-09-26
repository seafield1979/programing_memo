###while文
C言語とほぼ同じ。

~~~java
int i=0;
while(i<10) {
    System.out.println("count:" + String.valuceeOf(i));
    i++;
}

i = 0;
while (true) {
    System.out.println("count:" + String.valueOf(i));
    if (i >= 10) {
        break;
    }
    i++;
}
~~~