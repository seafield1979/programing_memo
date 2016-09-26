#Java クラスのプロパティ（メンバ変数）
[アクセス修飾子] [static] 型 変数名;

~~~java
例
int val1;
int val12 = 100;
static int val2;  // インスタンスなしでアクセス可能
public static int val3;  // クラス外部からインスタンスなしでアクセス可能
~~~

**アクセス修飾子**  

[Javaのアクセス修飾子―public、protected、private、デフォルトなど](https://www.zealseeds.com/Lang/LangJava/BasicGrammar/AccessQualifier/index.html)

|アクセス修飾子|説明|
|---|---|
省略 | 修飾子を省略した場合は、同じパッケージ内のクラスからアクセスできる。<br>パッケージ限定のpublic。
public | クラス外からアクセス可能。省略時に
protected | 自クラスと継承したクラスからアクセス可能
private | 自クラスからアクセス可能

アクセスレベル表

|アクセス修飾子|クラス|サブクラス|パッケージ|その他すべて|
|---|---|---|---|---|
private | ○ | × | × | ×
デフォルト | ○ | × | ○ | ×
protected | ○ | ○ | ○ | ×
public | ○ | ○ | ○ | ○ | ○


**型**  
型にはデータ型（プリミティブ型）とクラスが指定可能。

|データ型|説明|
|---|---|
boolean | true or false
char | 16ビットUnicode文字 ¥u0000～¥uFFFF
byte | 8ビット整数 -128～127
short | 16ビット整数 -32768～32767
int | 32ビット整数 -2147483648～2147483647
long | 64ビット整数 -9223372036854775808～9223372036854775807
float | 32ビット単精度浮動小数点数
double | 64ビット倍精度浮動小数点数