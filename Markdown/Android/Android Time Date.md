#Time 日時のあれこれ

##システムの時間(ms)
System.currentTimeMillis() で1970年1月1日からのミリ秒を取得できる。

```java
long currentTimeMillis = System.currentTimeMillis();
```

## Calendar 
DateクラスはAPI22でdeprecatedになってしまったのでCalendarを使う。
Dateは1970/1/1/ からの経過時間をmsで保持する。タイムゾーンの概念はない。
Calendarはタイムゾーンの指定ができる、日時の計算ができる。
たぶんCalendarがあれば日付や時間の処理は事足りる。

|戻り値|メソッド|説明|
|---|---|---|
|int|get(Calendar.YEAR)|年を取得|
|int|get(Calendar.MONTH)|月を取得  // 0が1月に相当するので +1する|
|int|get(Calendar.DAY_OF_MONTH)| 日を取得|
|int|get(Calendar.HOUR_OF_DAY) | 時間を取得(0~23)|
|int|get(Calendar.MINUTE) | 分を取得|
|int|get(Calendar.SECOND) | 秒を取得|
|void|add(Calendar.YEAR, 加算する年数)|年数を加算する。月、日等も可能|
|void|set(int field, int value)|指定フィールドに値を設定<br>例: cal.set(Calendar.YEAR, 2020)|
|void|set(int year, int month, int date)|年月日を設定する
|void|set(int year, int month, int date, int hourOfDay, int minute, int second)|年月日と時刻を設定|

```java
// Calendarオブジェクトに設定された日時を表示する
public static void showTime(Calendar cal) {
    System.out.println(String.format("%04d/%02d/%02d %02d:%02d:%02d",
            cal.get(Calendar.YEAR),
            cal.get(Calendar.MONTH)+1,  // 0が1月に相当するので +1する
            cal.get(Calendar.DAY_OF_MONTH),
            cal.get(Calendar.HOUR_OF_DAY),
            cal.get(Calendar.MINUTE),
            cal.get(Calendar.SECOND)));
}


public void test1() {
  Calendar calendar = Calendar.getInstance(TimeZone.getTimeZone("Asia/Tokyo"), Locale.JAPAN);
  
  // 1年後
  calendar.add(Calendar.YEAR, 1);
  showTime(calendar);
  
  // 1ヶ月後
  calendar.add(Calendar.MONTH, 1);
  showTime(calendar);
  
  // 指定日時
  Calendar calendar = Calendar.getInstance();
          calendar.set(2020, // Year
              8,  // Month
              1,  // Day of Month
              10, // Hour of Day
              0,  // Minute
              0); // Second
  showTime(calendar);
}
```