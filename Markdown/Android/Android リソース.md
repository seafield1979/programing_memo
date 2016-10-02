#リソース resources
リソースはプロジェクトツリーの
~~~
app
  + res 
~~~
以下に配置されている


##基本

###xmlから他のリソースを参照する
`@リソース種類/リソース名`
例:
@string/hoge_text

![](http://sunsunsoft.com/image/android/set_resource.png)

android:text="@string/hoge_text"


###リソースクラスを使ってリソースを参照
ActivityクラスのgetResourcesメソッドでResourceクラスのオブジェクトを取得し、これ経由でアプリのリソースを参照する。

```java
Resources res = getResources();
String titleString = res.getString(R.string.hoge_text);
Log.v("myLog", titleString);
```

Resourcesには以下のようなメソッドがある

~~~
boolean getBoolean(int id)
int getColor(int id)
float getDimension(int id)
Drawable getDrawable(int id)
int getInteger(int id)
Movie getMovie(int id)
String[] getStringArray(int id)
CharSequence getText(int id)
~~~

##タイプ別リソース
###色
ファイル color.xml

~~~
res
 + values
   + colors.xml
~~~
参照方法

```java
R.color.color名
R.color.colorPrimary

※color名は colors.xml で定義された名前
<color name="colorPrimary">#3F51B5</color>
```

###レイアウト
~~~
res
 + layout
   + .xml
~~~

###文字列
~~~
res
  + values
    + strings.xml
~~~

参照方法

```java
R.string.string名
```