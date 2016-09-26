##Java 配列
Javaの配列の特徴

* new で配列の実体を確保
* 多次元配列の要素数は揃っていない。要素毎に個別に一次元配列を持っている感じ。
* 配列の要素数を変更するメソッドや仕組みはない。要素数を変更したい場合は新しい要素数の配列を用意して、そちらに古い配列の値をコピーする。

###配列の宣言＆初期化
~~~java
//配列の宣言方法
//型 変数名[] = new 型[要素数];
//型[] 変数名 = new 型[要素数];

int intArray[] = new int[10];
int[] intArray = new int[10];

// 初期値あり配列
int array[] = {1,2,3,4,5};
  
// 配列長を取得
// 配列変数名.length
int array[] = new int[5];
System.out.println(array.length);

// 配列の要素をコピー
// cloneを使う
int[] cloneSrc = {1,2,3,4,5};
int[] cloneDst = cloneSrc.clone();
dispIntArray("cloneSrc", cloneSrc);
dispIntArray("cloneDst", cloneDst);

// arrayCopyを使う
// コピー本、コピー先の開始位置、コピー要素数を指定可能
int[] arrayCopySrc = {1,2,3,4,5,6,7,8,9,10};
int[] arrayCopyDst = new int[5];
System.arraycopy(arrayCopySrc, 3, arrayCopyDst, 0, 5);
dispIntArray("arrayCopyDst", arrayCopyDst);

// 多次元配列
// 要素の数は揃っていなくてもOK
// 型名 配列変数名[][];
// 配列変数名 = new 型名[要素数][];

int seiseki[][] = new int[2][];
int array1[] = new int[10];
int array2[] = new int[20];
seiseki[0] = array1;
seiseki[1] = array2;
 
// 多次元配列の初期値あり初期化
int num[][] = {{10, 8, 5}, {9, 16, 4, 11}, {3, 7, 5}};

~~~