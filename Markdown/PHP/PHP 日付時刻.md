#日時時刻取得 date

```php
date("Y-m-d H:i:s");

string date ( string $format [, int $timestamp = time() ] )

date_default_timezone_set('Asia/Tokyo');    // タイムゾーンを設定しないとグリニッジ標準時間になる
$date_str = date("Y-m-d H:i:s");
echo $date_str . "\n";

$date_str = date("Y-m-d H:i:d");
$next_day = date("Y-m-d", strtotime("+1 day"));

```

format

|フォーマット|説明|値|
|---|---|---|
|d | 日。二桁の数字（先頭にゼロがつく場合も） | 01 から 31
|D | 曜日。3文字のテキスト形式。 | Mon から Sun
|l | (小文字の 'L') | 曜日。フルスペル形式。 Sunday から Saturday
|w | 曜日。数値。 | 0 (日曜)から 6 (土曜)
|月 | --- |  ---
|F | 月。フルスペルの文字。 |  January から December
|m | 月。数字。先頭にゼロをつける。 |  01 から 12
|M | 月。3 文字形式。 |  Jan から Dec
|t | 指定した月の日数。 |  28 から 31
|年 | --- |  ---
|Y | 年。4 桁の数字。 |  例: 1999または2003
|y | 年。2 桁の数字。 |  例: 99 または 03
|時 | --- |  ---
|a | 午前または午後（小文字） | am または pm
|A | 午前または午後（大文字） | AM または PM
|h | 時。数字。12 時間単位。  | 01 から 12
|H | 時。数字。24 時間単位。  | 00 から 23
|i | 分。先頭にゼロをつける。 | 00 から 59
|s | 秒。先頭にゼロをつける。 | 00 から 59