#Realm
RealmはiOSやAndroidで使用出来るデータベース。これからiOS版のアプリも開発する上でDBのインターフェースが共通のほうが便利そうなのでRealmを使用したい。

リンク
[Qiita Realm for Android](http://qiita.com/wasabeef_jp/items/92bb700e37a0a57fc765)
[【Android】Realm for Android](https://wasabeef.jp/realm-for-android/)


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
    private int id;
    private String name;
    private int age;
    
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