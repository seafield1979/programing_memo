#Realm
RealmはiOSやAndroidで使用出来るデータベース。これからiOS版のアプリも開発する上でDBのインターフェースが共通のほうが便利そうなのでRealmを使用したい。

リンク
[Qiita Realm for Android](http://qiita.com/wasabeef_jp/items/92bb700e37a0a57fc765)
[【Android】Realm for Android](https://wasabeef.jp/realm-for-android/)

モデルクラスで使用可能なデータ一覧

Realm supports the following field types:
  boolean,
  byte,
  short,
  int,
  long,
  float,
  double,
  String,
  Date and byte[].
The integer types byte, short, int, and long are all mapped to the same type (long actually) within Realm. Moreover, subclasses of RealmObject and RealmList<? extends RealmObject> are supported to model relationships.

##使用方法
Android Studioの場合

Gradleに以下を追加

```xml
- build.gradle(Project:)- 

repositories {
        jcenter()
    }
    dependencies {
        classpath "io.realm:realm-gradle-plugin:1.1.0"  <-- これ
    }
```

```xml
- build.gradle(Module:app) - 

apply plugin: 'com.android.application'
apply plugin: 'realm-android'  <- これ

dependencies {
    compile 'io.realm:android-adapters:1.3.0' <- これ
}
```

RealmObjectを継承したモデルクラスを作成

```java
public class User extends RealmObject {
    @PrimaryKey
    private int id;   // <- こいつがプライマリーキーになる
    private String name;
    private int age;
    
    @Ignore   // データベースに保存しないメンバ変数
    
    
    
    // Get/Set
    ...
}
```

DAO(Data Access Object)クラスを作成

Realm.setDefaultConfiguration() を実行後に
Realm.getDefaultInstance() を使用すること。

```java
public class UserDAO {
    public UserDAO(Context context) {
        // Realm.getDefaultInstance() の前に Realm.setDefaultConfiguration をコールしておかないとエラーになる
        RealmConfiguration realmConfig = new RealmConfiguration.Builder(context)
        .schemaVersion(1)
        .migration(new UserMigration())
        .build();
        
        Realm.setDefaultConfiguration(realmConfig);
    }

    /**
     * Select
     * @return
     */
    public String[] select1() {
        Realm realm = Realm.getDefaultInstance();

        LinkedList<String> strs = new LinkedList();
        RealmResults<User> results =
                realm.where(User.class)
                        .greaterThan("age", 2).
                        findAll();
        for (User user : results) {
            Log.d("select", "name:" + user.getName());
            strs.add(user.getName());
        }

        return null;
    }

    /**
     * 全要素取得
     * @return nameのString[]
     */
    public String[] selectAll() {
        Realm realm = Realm.getDefaultInstance();

        LinkedList<String> strs = new LinkedList();
        try {
            RealmResults<User> results = realm.where(User.class).findAll();
            for (User user : results) {
                Log.d("select", "name:" + user.getName());
                strs.add(user.getName());
            }

        } catch (Exception e) {
            Log.d("error", e.getMessage());
        }

        return strs.toArray(new String[0]);
    }

    /**
     * 要素を追加
     * @param name
     * @param age
     */
    public void add1(String name, int age) {
        Realm realm = Realm.getDefaultInstance();

        int newId = getNextUserId(realm);

        User user = new User();
        user.setId(newId);
        user.setName(name);
        user.setAge(age);

        realm.beginTransaction();
        realm.copyToRealm(user);
        realm.commitTransaction();
    }

    /**
     * かぶらないプライマリIDを取得する
     * @param realm
     * @return
     */
    public int getNextUserId(Realm realm) {
        // 初期化
        int nextId = 1;
        // userIdの最大値を取得
        Number maxUserId = realm.where(User.class).max("id");
        // 1度もデータが作成されていない場合はNULLが返ってくるため、NULLチェックをする
        if(maxUserId != null) {
            nextId = maxUserId.intValue() + 1;
        }
        return nextId;
    }
}
```

Migration
データベースのバージョンが変わった場合に、バージョンアップの処理を行う必要がある。その時に使用されるのがMigrationクラス。
Migrationクラスの処理としては、必要におい時て migrateメソッドが呼ばれる。引数は古い(現在の)バージョンがoldVersion、移行先のバージョンがnewVersion。

公式のサンプルがわかりやすかったのでこちらを参考にする。
[Realm公式 Migration](https://realm.io/docs/java/latest/#migrations)

```java
import io.realm.DynamicRealm;
import io.realm.RealmMigration;

public class UserMigration implements RealmMigration {
    @Override
    public void migrate(DynamicRealm realm, long oldVersion, long newVersion) {

        // Migrate from version 0 to version 1
        if (oldVersion == 0) {
            // マイグレーション処理
            oldVersion = 1;
        }

        // Migrate from version 1 to version 2
        if (oldVersion == 1) {
            // マイグレーション処理
            oldVersion = 2;
        }
    }
}
```

###SELECT
データを取得する。条件に一致するデータだけを取得できたりもする。

<Realmオブジェクト>.where(<モデルクラス>.class).<条件指定メソッド>.<取得メソッド>;

例:
mRealm.where(User.class).greaterThan("age", 10).findAll();

find系メソッド
public RealmResults<E> findAll()
public RealmResults<E> findAllAsync()
public RealmResults<E> findAllSorted(String fieldName, Sort sortOrder)
public RealmResults<E> findAllSortedAsync(final String fieldName, final Sort sortOrder)
public RealmResults<E> findAllSorted(String fieldName)
public RealmResults<E> findAllSortedAsync(String fieldName)
public RealmResults<E> findAllSorted(String fieldNames[], Sort sortOrders[])
public RealmResults<E> findAllSortedAsync(String fieldNames[], final Sort[] sortOrders)
public RealmResults<E> findAllSorted(String fieldName1, Sort sortOrder1,
                                         String fieldName2, Sort sortOrder2)
public RealmResults<E> findAllSortedAsync(String fieldName1, Sort sortOrder1,
                                              String fieldName2, Sort sortOrder2)
                                              
public E findFirst()
public E findFirstAsync()

```java
// 全件取得
RealmResults<User> results = mRealm.where(User.class).findAll();
for (User user : results) {
    Log.d("select", "id:" + user.getId() + " name:" + user.getName() + " age:" + user.getAge());
}

// 条件指定で取得
RealmResults<User> results =
                mRealm.where(User.class)
                        .greaterThan("age", age)
                        .findAll();
        for (User user : results) {
            Log.d("select", "name:" + user.getName() + " age:" + user.getAge());
            strs.add(user.getName());
        }

// １件だけ取得
User user = mRealm.where(User.class).greaterThan("age", 20).findFirst();
if (user != null) {
    Log.d("select", "name:" + user.getName());
}
```

###ADD
データを追加する
モデルクラスのオブジェクトに値をセットしてから
mRealm.beginTransaction();
mRealm.copyToRealm(モデルオブジェクト);
mRealm.commitTransaction();
のようにする。

```java
// Userデータを１件追加
User user = new User();
user.setId(100);
user.setName("hoge");
user.setAge(37);

mRealm.beginTransaction();
mRealm.copyToRealm(user);
mRealm.commitTransaction();

// たくさん追加
// 最初と最後にトランザクション処理を行う
mRealm.beginTransaction();
for (User user : list) {
    int newId = getNextUserId(mRealm);
    user.setId(newId);
    Log.d("userdao", user.getMessage());

    mRealm.copyToRealm(user);
}
mRealm.commitTransaction();
```

###INSERT
データの追加はcopyToRealm()メソッドを使うが、大量にデータを追加するとcopyToRealmが追加したオブジェクトを返す仕様のせいで速度が低下するらしい。これを回避するにはcopyToRealmの代わりにinsertメソッドを使用する。

[Realm Java 1.1.0 — Insert APIを追加！](https://realm.io/jp/news/realm-java-1.1.0/)

Insertメソッド
void Realm.insert(RealmModel obj)
void Realm.insert(Collection collection)
void Realm.insertOrUpdate(RealmModel obj)
void Realm.insertOrUpdate(Collection collection)

```java
User user = new User();
user.setName(name);
user.setAge(age);

mRealm.beginTransaction();
mRealm.insert(user);
mRealm.commitTransaction();
```

###UPDATE
データを更新するにはfind系のメソッドで取得したRealmResultのオブジェとに対して値を設定するだけ。

```java
// １件だけ更新
mRealm.beginTransaction();
User user = mRealm.where(User.class).equalTo("id", id).findFirst();

user.setName(name);
user.setAge(age);
mRealm.commitTransaction();

// 複数件更新
mRealm.beginTransaction();
RealmResults<User> results = mRealm.where(User.class).equalTo("age", 10).findAll();
for (int i=0; i<results.size(); i++){
    User user = results.get(i);
    user.setName(name);
    user.setAge(age);
}
mRealm.commitTransaction();
```

###DELETE
データを削除するにはRealmResultsオブジェクトに対してdelete系のメソッドを使用する。
public boolean deleteFirstFromRealm()
public boolean deleteLastFromRealm()
public boolean deleteAllFromRealm()
public void deleteFromRealm(int location) 

```java
// １件削除
mRealm.beginTransaction();
RealmResults<User> results = mRealm.where(User.class).equalTo("id", id).findAll();
results.deleteFirstFromRealm();
mRealm.commitTransaction();

// 全件削除
mRealm.beginTransaction();
RealmResults<User> results = mRealm.where(User.class).findAll();
results.deleteAllFromRealm();
mRealm.commitTransaction();
```

###条件指定
whereでデータを絞り込むのに使用する。
使用例

```java
mRealm.where(User.class)
    .greaterThan("age", age)
    .findAll();
```

between(), greaterThan(), lessThan(), greaterThanOrEqualTo() & lessThanOrEqualTo()
equalTo() & notEqualTo()
contains(), beginsWith() & endsWith()
isNull() & isNotNull()
isEmpty() & isNotEmpty()

複数の条件を指定する
使用例 IDのリストで絞り込む

```java
// 基本
RealmQuery<User> query = realm.where(User.class);

// Add query conditions:
query.equalTo("name", "Wasabeef");
query.or().equalTo("name", "Chip");
query.or().equalTo("name", "Hoge");

// Execute the query:
RealmResults<User> resultAll = query.findAll();

```

```java
// 配列の要素をforで回す 
// int[] ids のIDに一致する要素を全て取得

// Build the query looking at all users:
RealmQuery<TangoCard> query = mRealm.where(TangoCard.class);

// Add query conditions:
boolean isFirst = true;
for (int id : ids) {
    if (isFirst) {
        isFirst = false;
        query.equalTo("id", id);
    } else {
        query.or().equalTo("id", id);
    }
}
// Execute the query:
RealmResults<TangoCard> results = query.findAll();
```