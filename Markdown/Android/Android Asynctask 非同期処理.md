#非同期処理 AsyncTask

Android で UIスレッドと協調動作する処理を行いたい場合はAsyncTaskが向いている。

**特徴**

* バックグラウンドで処理する
* UIスレッドと協調できる。UIスレッドの要素にアクセスしたい場合はrunOnUiThreadで生成したスレッド処理ないでする。
* １オブジェクトで１回しかexecuteできない。停止したら破棄して新しいものを作る。


AsyncTaskの主なクラス

|メソッド|説明|
|---|---|
|onPreExecute()|doInBackgroundメソッドの実行前にメインスレッドで実行されます。非同期処理前に何か処理を行いたい時などに使うことができます。
|doInBackground()|メインスレッドとは別のスレッドで実行されます。非同期で処理したい内容を記述します。 このメソッドだけは必ず実装する必要があります。
|onProgressUpdate()|メインスレッドで実行されます。非同期処理の進行状況をプログレスバーで 表示したい時などに使うことができます。
|onPostExecute()|doInBackgroundメソッドの実行後にメインスレッドで実行されます。doInBackgroundメソッドの戻り値をこのメソッドの引数として受け取り、その結果を画面に反映させることができます。|


AsyncTask<Params, Progress, Result>
の意味は
Params doInBackground()メソッドの引数の型
Progress onProgressUpdate()メソッドの引数
Result doInBackground()メソッドの戻り値の型 

```java
public class HogeAsync extends AsyncTask<Void, Void, String> {

    @Override
    protected void onPreExecute() {
        super.onPreExecute();
        // doInBackground前処理
    }

    @Override
    protected String doInBackground(Void... params) {
        return null;
    }

    @Override
    protected void onPostExecute(String result) {
        super.onPostExecute(result);
        // doInBackground後処理
    }
}
```

使い方

###無名オブジェクトを使う場合
AsyncTask オブジェクトを生成後、executeして処理を開始する

```java
public class MainActivity extends AppCompatActivity {
  @Override
  protected void onCreate(Bundle savedInstanceState) {
      ・
      ・
      mTextView = (TextView)findViewById(R.id.textView);
  }
  
 /**
   * UIスレッドのViewにテキストを表示する
   * @param text
   */
  private void sendText(final String text) {
      runOnUiThread(new Runnable() {
          @Override
          public void run() {
              mTextView.append(text);
          }
      });
  }
  private test1() {
    new AsyncTask<Void, Void, String>() {
        /**
         * 通信において発生したエラー
         */
        private Throwable mError = null;

        @Override
        protected String doInBackground(Void... params) {
            for (int i = 0; i < 5; i++) {
                try {
                    Thread.sleep(1000); //1000ミリ秒Sleepする
                } catch (InterruptedException e) {
                }
                sendText("" + (i + 1) + "\n");
                Log.d("myLog", "" + (i + 1));
            }
            return "finish!!\n";
        }

        @Override
        protected void onPreExecute() {
            super.onPreExecute();
            // doInBackground前処理
        }

        @Override
        protected void onPostExecute(String result) {
            super.onPostExecute(result);

            // doInBackground後処理
            mTextView.append(result);
        }
    }.execute();
  }
}
```

###オブジェクト変数を使用する
AsyncTask型の変数にオブジェクトを代入する方法。

```java
public class MainActivity extends AppCompatActivity {

  private AsyncTask<Void, Void, String> mTask2;

  private void test2() {
    if (mTask2 == null) {
        initTask2();
    }

    if (mTask2.getStatus() != AsyncTask.Status.RUNNING) {
        mTask2.execute();
    } else {
        Log.v("myLog", "実行中\n");
    }
  }
  private void initTask2() {
    mTask2 = new AsyncTask<Void, Void, String>() {
        /**
         * 通信において発生したエラー
         */
        private Throwable mError = null;

        @Override
        protected String doInBackground(Void... params) {
            for (int i = 0; i < 5; i++) {
                try {
                    Thread.sleep(SLEEP_TIME);
                } catch (InterruptedException e) {
                }
                sendText("" + (i + 1) + "\n");
                Log.d("myLog", "" + (i + 1));
            }
            return "finish!!\n";
        }

        @Override
        protected void onPreExecute() {
            super.onPreExecute();
            // doInBackground前処理
        }

        @Override
        protected void onPostExecute(String result) {
            super.onPostExecute(result);

            // doInBackground後処理
            mTextView.append(result);
            mTask2 = null;
        }
    };
  }
}
```

