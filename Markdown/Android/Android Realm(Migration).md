#Realm でマイグレーション
マイグレーションはデータベースのテーブルをそのままに、カラムを追加したり変更したりすること。

古いバージョンになかったテーブルやカラムを追加した場合や削除された場合などに、現在のデータベースに対して変更を加える処理を行う。


リンク
[Realm Migration Example](https://github.com/realm/realm-java/blob/master/examples/migrationExample/src/main/java/io/realm/examples/realmmigrationexample/model/Migration.java)

Realmのマイグレーションの方法

##仕組みとか
Realmのスキーマオブジェクトは現在のバージョンを持っている。
このバージョンはRealmを初期化する時に渡している。

↓こんな感じ

```swift
RealmConfiguration realmConfig = new RealmConfiguration.Builder(context)
                .schemaVersion(1)   // <- 1が現在のバージョンだYO
                .build();
Realm.setDefaultConfiguration(realmConfig);
```

このバージョン管理は完全にユーザーが行う。新しくモデル(RealmObjectのサブクラス)を追加したり、カラムを追加、削除した場合などに自分でバージョンをあげる。

例えば、上記のバージョン(1)でアプリを実行したあとに、モデルに"address"カラムを追加したとする。

```swift
public class Person extends RealmObject{
    @PrimaryKey
    private int id;
    private String name;    
    private String address;   // NEW!!
}
```

これでアプリを実行すると、以前のモデルにないaddressが追加されたのにマイグレーションされてないよ、みたいなエラーが出る。Realmの内部では以前実行した時のモデルのカラムを保持していて、この内容と現在のモデルのカラムが異なっているとエラーにするようだ。
Realm側で自動でうまいことカラムを追加して整合性をとってくれたりはしない。

この場合、RealmのMigrationの仕組みを使って Realmに address カラムが追加されたことを知らせてあげないといけない。

で、どうやってMigrationを行うか。Realmの初期化時に RealmConfigurationオブジェクトの migration()メソッドでMigrationを行うためのクラスを渡せるので、

```swift
RealmConfiguration realmConfig = new RealmConfiguration.Builder(context)
                .schemaVersion(2)
                .migration(new MyMigration())  // マイグレーションを行うクラス指定
                .build();
Realm.setDefaultConfiguration(realmConfig);
```

のようにしてマイグレーションを行うクラス MyMigration(名前は自由)を指定してあげる。
マイグレーションクラスの本体は以下のように書く。

```swift
- MyMigration.java -

public class MyMigration implements RealmMigration{
  
  /**
   * oldVersion がマイグレーション前のバージョン
   * newVersion がマイグレーション後のバージョン
   */
  @Override
  public void migrate(DynamicRealm realm, long oldVersion, long newVersion) {
      RealmSchema schema = realm.getSchema();

      // Migrate from version 1 to version 2
      if (oldVersion == 1) {
          // マイグレーション処理 
          // String address を追加する
          RealmObjectSchema migraSchema = schema.get("Person");
          migraSchema.addField("address", String.class, FieldAttribute.REQUIRED);
          
          oldVersion = 2;
      }
  }
}
```

このクラスはRealmオブジェクトのバージョンが変わった時（schemaVersion()で指定したバージョンが変わった場合)に自動で呼ばれる。
このクラスの migrate() メソッドに必要な変更の処理を書く。
一度にまとめてバージョンアップがされる場合もあるので、古いバージョンから最新のバージョンへ１つづつバージョンをアップする処理を書く必要がある。

例えば初期バージョンの1から 2 -> 3 -> 4とバージョンアップした場合は、以下のようになる。

```swift
public class MyMigration implements RealmMigration{
  /**
   * oldVersion がマイグレーション前のバージョン
   * newVersion がマイグレーション後のバージョン
   */
  @Override
  public void migrate(DynamicRealm realm, long oldVersion, long newVersion) {
      RealmSchema schema = realm.getSchema();

      // Migrate from version 1 to version 2
      if (oldVersion == 1) {
          // マイグレーション処理 
          // String address を追加する
          RealmObjectSchema migraSchema = schema.get("Person");
          migraSchema.addField("address", String.class, FieldAttribute.REQUIRED);
          
          oldVersion = 2;
      }
      
      // Migrate from version 2 to version 3
      if (oldVersion == 2) {
          ・・・
          oldVersion = 3;
      }
      // Migrate from version 3 to version 4
      if (oldVersion == 3) {
          ・・・
          oldVersion = 4;
      }
  }
}
```

マイグレーションの処理

###テーブルを追加した
TangoBoxテーブルを追加。

```swift
RealmObjectSchema boxSchema = schema.create("TangoBox")
                    .addField("id", Integer.class, FieldAttribute.PRIMARY_KEY)
                    .addField("name", String.class, FieldAttribute.REQUIRED)
                    .addField("comment", String.class, FieldAttribute.REQUIRED)
                    .addField("color", Integer.class, FieldAttribute.REQUIRED)
                    .addField("createTime", Date.class, FieldAttribute.REQUIRED)
                    .addField("updateTime", Date.class, FieldAttribute.REQUIRED);
```

###カラムを追加した
TangoBox に history を追加

```swift
RealmObjectSchema boxSchema = schema.get("TangoBox")
        .addField("history", Integer.class, FieldAttribute.REQUIRED)
```

###カラムを削除した
TangoBox から firstName を削除

```swift
RealmObjectSchema boxSchema = schema.get("TangoBox")
        .removeField("firstName")

```

###データを変更