#データベース Database
SQLiteを使ったデータベース

テーブルを作成する
データベースの作成、テーブルの作成はSQLiteOpenHelperクラスを継承した専用のヘルパークラスで行う。

```java
- ContactDbOpenHelper.java  -

import android.content.Context;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;
import android.util.Log;

public class ContactDbOpenHelper extends SQLiteOpenHelper {
    private static final String TAG = "ContactDbOpenHelper";

    static final String DATABASE_NAME = "contact.db";
    static final int DATABASE_VERSION = 1;

    public ContactDbOpenHelper(Context context) {
        // データベースファイル名とバージョンを指定しSQLiteOpenHelperクラスを初期化
        super(context, DATABASE_NAME, null, DATABASE_VERSION);
        Log.d(TAG, "ContactDbOpenHelperのコンストラクタが呼ばれました");
    }

    @Override
    public void onCreate(SQLiteDatabase database) {
        Log.d(TAG, "ContactDbOpenHelper.onCreateが呼ばれました");
        // Contactテーブルを生成
        // @formatter:off
        database.execSQL("CREATE TABLE "
                + Contact.TBNAME + "("
                + Contact._ID + " INTEGER PRIMARY KEY AUTOINCREMENT, "
                + Contact.NAME + " TEXT NOT NULL, "
                + Contact.AGE + " INTEGER " + ");");
        // @formatter:on
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        Log.d(TAG, "ContactDbOpenHelper.onUpgradeが呼ばれました");
        // Contactテーブルを再定義するため現在のテーブルを削除
        db.execSQL("DROP TABLE IF EXISTS " + Contact.TBNAME);
        onCreate(db);
    }
}

- Cotanct.java - 
テーブルのカラム名
import android.provider.BaseColumns;

public interface Contact extends BaseColumns {
    String TBNAME = "t_contact";
    String NAME = "f_name";
    String AGE = "f_age";
}

```

使用方法

```java
ContactDbOpenHelper helper = null;
SQLiteDatabase db = null;
try {
    // ContactDbOpenHelperを生成
    // 書き込み可能なSQLiteDatabaseインスタンスを取得
    helper = new ContactDbOpenHelper(this);
    db = helper.getWritableDatabase();
    
    // ここでデータベース処理処理
    
} finally {
    // 後片付け
    if (db != null) {
        db.close();
    }
    if (helper != null) {
        helper.close();
    }
}
```

##テーブル操作処理
上記の使用方法の
// ここでデータベース処理
の部分でテーブル操作処理を行う

###SELECT文
条件をつけてレコードを選択する

```java
Cursor cursor = null;
try {
    // Commentsテーブルのすべてのデータを取得
//  Cursor query(String table, String[] columns, String selection,
//  String[] selectionArgs, String groupBy, String having,
//  String orderBy) {
    cursor = db.query(Contact.TBNAME,   // テーブル名
            null,                       // カラム指定
            Contact.AGE + " > ?",       // SELECT文
            new String[]{Integer.toString(10)},  // SELECT文パラメータ(1つでもString[]で渡す)
            null,       // GROUP BY (グループ化したい時に使用)
            null,       // HAVING (グループ化された項目に対する条件)
            Contact.NAME);  // ORDER BY 
    // Cursorにデータが１件以上ある場合処理を行う
    if (cursor != null) {
        while (cursor.moveToNext()) {
            // 名前を取得
            String name = cursor.getString(cursor
                    .getColumnIndex(Contact.NAME));
            // 年齢を取得
            int age = cursor.getInt(cursor.getColumnIndex(Contact.AGE));
            textView.append(name + ":" + age + "\n");
        }

    }
} finally {
    if (cursor != null) {
        cursor.close();
    }
}
```

###INSERT文
レコードを追加する

```java
// 生成するデータを格納するContentValuesを生成
ContentValues values = new ContentValues();
values.put(Contact.NAME, "Shutaro");
values.put(Contact.AGE, 10);

// 戻り値は生成されたデータの_IDが返却される
long id = db.insert(Contact.TBNAME, null, values);
```

###UPDATE文
レコードを更新する
設定する値は事前に ContentValuesに設定しておく。

```java
// 更新データを格納するContentValuesを生成
ContentValues values = new ContentValues();
// Contact.NAMEにaが含まれるデータの年齢を25に変更
values.put(Contact.AGE, mRand.nextInt(100));
// 戻り値は更新した数が返却される
int n = db.update(Contact.TBNAME,   // テーブル名
      values,                       // 設定値(ContentValuesに設定しておく)
      Contact.NAME + " like ?",     // SELECT文
      new String[]{"%a%"});         // SELECT文パラメータ
```

###DELETE文
レコードを削除する

```java
// Contact.NAMEがJokerのデータを削除
// 戻り値は削除した数が返却される
int n = db.delete(Contact.TBNAME,   // テーブル名
        Contact.NAME + " = ?",      // SELECT文(削除するレコードを選択)
        new String[]{"Joker"});     // SELECT文パラメータ
        
// 全て削除する
int n2 = db.delete(Contact.TBNAME, null, null);
```