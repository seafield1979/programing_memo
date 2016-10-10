#アプリ Appli

###アプリバージョンを取得
getPackageManager().getPackageInfo(getPackageName(), 0);
を使用する。

```java
PackageManager pm = getPackageManager();
int versionCode = 0;
String versionName = "";
try {
    PackageInfo packageInfo = pm.getPackageInfo(getPackageName(), 0);
    versionCode = packageInfo.versionCode;
    versionName = packageInfo.versionName;
} catch (PackageManager.NameNotFoundException e) {
    e.printStackTrace();
}

String msg = String.format("versionCode:%1$s, versionName:%2$s",
        versionCode, versionName);
```

###アクションに対応するアプリの一覧取得
指定したアクションを持つアプリをResolveInfoリストで取得する。

```java
PackageManager pm = getPackageManager();
Intent intent = new Intent();

// アクションを指定(ACTION_SENDTOとか、ACTION_VIEWとか)
intent.setAction(Intent.ACTION_SEARCH);

List<ResolveInfo> activities = pm.queryIntentActivities(intent, PackageManager
        .MATCH_ALL);

ArrayList<String> nameList = new ArrayList<String>();
for (ResolveInfo info : activities) {
    Log.v("myLog", (String)info.loadLabel(pm));
}
```