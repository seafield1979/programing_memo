#ダイアログ Dialog

本家のマニュアル（日本語）
[Android Developers ダイアログ](https://developer.android.com/guide/topics/ui/dialogs.html)

ダイアログのサンプルでよく見る以下のようなダイアログは画面回転に対応していないため使わないほうが良いらしい。

```java
new AlertDialog.Builder(this)
  .setTitle("title")
  .setMessage("message")
  .setPositiveButton("OK", null)
  .show();
```

公式に推奨されているのは DialogFragment クラスを継承した自前のダイアログクラスを作成し、それの onCreateDialog で生成したダイアログを返す方法。これだとダイアログがActivityのライフサイクルに乗るので端末の画面を回転させた時も問題なく動作する。

```java
public class MainActivity extends FragmentActivity {
   .
   .
   .
   private void showAlert() {
      FragmentManager fm = getSupportFragmentManager();
      AlertFragment af = new AlertFragment();
      af.show(fm, "alert_dialog");
   }
}
public class AlertFragment extends DialogFragment {
   @Override
   public Dialog onCreateDialog(Bundle savedInstanceState) {
      AlertDialog.Builder builder = new AlertDialog.Builder(getActivity());
      builder.setTitle("Title")
             .setMessage("Message");
      return builder.create();
   }
}
```

###呼び出し元のActivityに戻り値を返す
戻り値を直接返す仕組みはない。
呼び出し先のDialogFragmentで呼び出し元のActivityを参照できるので、ここから呼び出し元で定義した戻り値を返す用のメソッドを呼び出す。

```java
public class MainActivity extends FragmentActivity implements OnClickListener{
  ・
  ・
  ・
  // 名前や引数はなんでもOK
  public void onReturnValue(String value) {
      Log.v("myLog", value);
  }
}
```

```java
public class Dialog1Fragment extends DialogFragment {
    @Override
    public Dialog onCreateDialog(Bundle savedInstanceState) {
    ・
    ・
    ・
      builder.setPositiveButton("OK", new DialogInterface.OnClickListener() {
          @Override
          public void onClick(DialogInterface dialog, int id) {
              // 呼び出し元のActivityはgetActivity()で取得
              // それ経由でメソッドを呼び出す
              MainActivity callingActivity = (MainActivity) getActivity();
              callingActivity.onReturnValue("Dialog1Fragment OK");
          }
        }
    }
});
```

###ダイアログ１
タイトル、メッセージ、OK、キャンセルボタンが表示される。
![](http://sunsunsoft.com/image/android/dialog_simple.png)

```java

/**
 * Created by shutaro on 2016/10/04.
 * タイトル、メッセージ、OKボタン、キャンセルボタンを表示するダイアログ
 */
public class Dialog1Fragment extends DialogFragment {
    @Override
    public Dialog onCreateDialog(Bundle savedInstanceState) {
        AlertDialog.Builder builder = new AlertDialog.Builder(getActivity());

        // 渡された引数を取得する
        Bundle bundle = getArguments();
        String title = bundle.getString("title");
        String message = bundle.getString("message");
        boolean isCancel = bundle.getBoolean("isCancel");

        // Add action buttons
        // Title
        if (title != null) {
            builder.setTitle(title);
        }
        if (message != null) {
            builder.setMessage(message);
        }
        // OK
        builder.setPositiveButton("OK", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int id) {
                // 呼び出し元のActivityはgetActivity()で取得
                MainActivity callingActivity = (MainActivity) getActivity();
                callingActivity.onReturnValue("Dialog1Fragment OK");
            }
        });

        // Cancel
        if (isCancel) {
            builder.setNegativeButton("Cancel", new DialogInterface.OnClickListener() {
                public void onClick(DialogInterface dialog, int id) {
                    Dialog1Fragment.this.getDialog().cancel();
                }
            });
        }
        builder.setNeutralButton("Later", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                // Later button pressed
            }
        });

        return builder.create();
    }
}

呼び出し (MainActivityクラス等)

FragmentManager fm = getSupportFragmentManager();
Dialog1Fragment dialog1 = new Dialog1Fragment();

// フラグメントにBundle経由で引数を渡す
Bundle args = new Bundle();
args.putString("title", "たいとる");
args.putString("message", "めっせーじ");
dialog1.setArguments(args);

dialog1.show(fm, "alert_dialog");
```

###独自レイアウトダイアログ
ダイアログに自前のlayoutを適用したダイアログ。例ではEditTextをもつlayoutを表示している。
![](http://sunsunsoft.com/image/android/dialog_custom.png)

```java
public class AlertFragment extends DialogFragment {
    @Override
    public Dialog onCreateDialog(Bundle savedInstanceState) {
        AlertDialog.Builder builder = new AlertDialog.Builder(getActivity());
        LayoutInflater inflater = getActivity().getLayoutInflater();

        // 渡された引数を取得する
        Bundle bundle = getArguments();
        String title = bundle.getString("title");
        String message = bundle.getString("message");

        // layoutファイルから自前のViewを取得
        final View layout = inflater.inflate(R.layout.dialog_contents, null);

        TextView textView = (TextView)layout.findViewById(R.id.messageTextView);
        if (message != null) {
            textView.setText(message);
        }

        builder.setView(layout);

        // Add action buttons
        // Title
        if (title != null) {
            builder.setTitle(title);
        }
        // OK
        builder.setPositiveButton("OK", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int id) {
                        // エディットの入力文字列を取得する
                        EditText text = (EditText) layout.findViewById(R.id.edit_text);
                        String string = text.getText().toString();

                        // 呼び出し元のActivityはgetActivity()で取得
                        MainActivity callingActivity = (MainActivity) getActivity();
                        callingActivity.onReturnValue(string);
                    }
                });
        // Cancel
        builder.setNegativeButton("Cancel", new DialogInterface.OnClickListener() {
            public void onClick(DialogInterface dialog, int id) {
                AlertFragment.this.getDialog().cancel();
            }
        });

        return builder.create();
    }
}

呼び出し MainActivity

FragmentManager fm = getSupportFragmentManager();
// ダイアログ用のフラグメントを生成
AlertFragment af = new AlertFragment();

// フラグメントにBundle経由で引数を渡す
Bundle args = new Bundle();
args.putString("title", "たいとる");
args.putString("message", "めっせーじ");
af.setArguments(args);

// ダイアログを表示
af.show(fm, "alert_dialog");
```

###複数の選択項目があるダイアログ
押せるタイプの選択項目があるダイアログ
![](http://sunsunsoft.com/image/android/dialog_items.png)

```java
public class Dialog2Fragment extends DialogFragment {

    final String[] items = {"item_0", "item_1", "item_2"};

    @Override
    public Dialog onCreateDialog(Bundle savedInstanceState) {
        AlertDialog.Builder builder = new AlertDialog.Builder(getActivity());

        // 渡された引数を取得する
        Bundle bundle = getArguments();
        String title = bundle.getString("title");

        // Add action buttons
        // Title
        if (title != null) {
            builder.setTitle(title);
        }
        builder.setItems(items, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                // item_which pressed
                Log.v("myLog", "Dialog2 " + String.valueOf(which));
            }
        });

        return builder.create();
    }
}

呼び出し

// 選択項目が３つあるダイアログ
FragmentManager fm = getSupportFragmentManager();
Dialog2Fragment dialog2 = new Dialog2Fragment();

// フラグメントにBundle経由で引数を渡す
Bundle args = new Bundle();
args.putString("title", "たいとる");
dialog2.setArguments(args);

// ダイアログを表示
dialog2.show(fm, "dialog2");
```

###チェックボックスつきダイアログ
チェックボックスあり、それぞれをチェックできるダイアログ
![](http://sunsunsoft.com/image/android/dialog_check.png)

```java
public class Dialog3Fragment extends DialogFragment {

    final String[] items = {"item_0", "item_1", "item_2"};

    @Override
    public Dialog onCreateDialog(Bundle savedInstanceState) {
        AlertDialog.Builder builder = new AlertDialog.Builder(getActivity());

        final ArrayList<Integer> checkedItems = new ArrayList<Integer>();

        // 渡された引数を取得する
        Bundle bundle = getArguments();
        String title = bundle.getString("title");

        // Add action buttons
        // Title
        if (title != null) {
            builder.setTitle(title);
        }

        // OK
        builder.setPositiveButton("OK", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int id) {
                // 呼び出し元のActivityはgetActivity()で取得
                MainActivity callingActivity = (MainActivity) getActivity();
                callingActivity.onReturnValue(checkedItems);
            }
        });

        // Checked list
        builder.setMultiChoiceItems(items, null, new DialogInterface.OnMultiChoiceClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which, boolean isChecked) {
                if (isChecked) {
                    checkedItems.add(which);
                }
                else {
                    checkedItems.remove((Integer) which);
                }
            }
        });

        return builder.create();
    }
}

呼び出し

// 選択項目が３つあるダイアログ
FragmentManager fm = getSupportFragmentManager();
Dialog3Fragment dialog3 = new Dialog3Fragment();

// フラグメントにBundle経由で引数を渡す
Bundle args = new Bundle();
args.putString("title", "たいとる");
dialog3.setArguments(args);

// ダイアログを表示
dialog3.show(fm, "dialog2");
```