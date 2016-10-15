#SeekBar
値をスライドして設定値を変更できるWidget
![](http://sunsunsoft.com/image/android/widget_seekbar.png)


レイアウト
max がバーの最大値
progress がバーの現在値

```xml
<SeekBar
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/seekBar"
        android:max="100"
        android:progress="50"/>
```

メソッド

|戻り値|メソッド|説明|
|---|---|---|
|void|setOnSeekBarChangeListener<br>(OnSeekBarChangeListener l)|リスナーを設定する<br>リスナーではシークバーの値が変更されたイベントの処理を設定できる|
|void|incrementProgressBy(int diff)|シーク値を加算/減算する|
|void|setProgress(int progress)|シーク値を設定する|


スライドで値を変更された時のイベントリスナーを設定する

```java
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    
    seekBar.setOnSeekBarChangeListener(new OnSeekBarChangeListener() {
        // トラッキング開始時
        @Override
        public void onStartTrackingTouch(SeekBar seekBar) {
            positionText.setText(String.valueOf(seekBar.getProgress()));
        }
        // トラッキング中
        @Override
        public void onProgressChanged(SeekBar seekBar, int progress, boolean fromTouch) {
            positionText.setText(String.valueOf(progress) + ", " + String.valueOf(fromTouch));
        }
        // トラッキング終了時
        @Override
        public void onStopTrackingTouch(SeekBar seekBar) {
            positionText.setText(String.valueOf(seekBar.getProgress()));
        }
    });
}
```