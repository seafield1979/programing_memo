#ライブラリ(jar)作成

別のプロジェクトで自作のクラスを使えるようにする。

jarの作り方を解説。手順の詳細、設定ファイルの各パラメータの説明。
[【Android】AndroidStudioでjarを出力する](http://tech.admax.ninja/2014/09/10/export-jar-by-android-studio/)


##jarの作り方(Android Studio)

###プロジェクトにモジュールを追加
Android Studio メニューの
`File - New - New Module` でNew Moduleダイアログを表示、`Android Library`を選択する。
適当な Module name をつけて`Finish`ボタンを押す。

これでプロジェクトの `Gradle Scripts` 以下にbuild.gradle(Module: ライブラリ名) のような項目が追加される。

![](http://sunsunsoft.com/image/android/make_jar.png)

また、ライブラリ用のパッケージ(mylibrary)も追加される。

![](http://sunsunsoft.com/image/android/make_jar2.png)

このパッケージに自作のクラスファイルを追加する(以下の例ではLogListView.javaを追加)

![](http://sunsunsoft.com/image/android/make_jar3.png)


###build.gradleファイルを編集
`build.gradle(Module: ライブラリ名)`を開いて以下のように編集する。

```xml
- build.gradle -

apply plugin: 'com.android.library'

android {
    compileSdkVersion 24
    buildToolsVersion "24.0.2"

    defaultConfig {
        minSdkVersion 21
        targetSdkVersion 24
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

<!--ここ以下を追加-->
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:24.2.0'
}

task clearJar(type: Delete) {
    delete 'build/libs/mylibrary.jar'
}
task makeJar(type: Copy) {
    from('build/intermediates/bundles/release/')
    into('release/')
    include('classes.jar')
    rename('classes.jar', 'mylibrary.jar')
}
makeJar.dependsOn(clearJar, build)
```

###jarをビルドする
コマンドラインからプロジェクトファイルに移動して以下のコマンドを実行

```sh
$ ./gradlew ${Moduel name}:clean ${Module name}:assembleDebug ${Module name}:makeJar
```

不思議なくらい長々とビルド処理が実行される。
ビルドが成功すると `<プロジェクト>/app/libs/` 以下にjarファイルが出力される。

##jarファイルを使う
作成されたjarをプロジェクトに組み込むには以下の手順

### jarファイルをコピー
.jar を組み込みたいプロジェクト以下 `<プロジェクト>/app/libs` にコピー

### build.gradle を編集
プロジェクトの `Gradle Scripts - build.gradle(Module: App)` を編集
dependencies ブロック以下に
`compile files('libs/作成したJar.jar')`
を追加する

![](http://sunsunsoft.com/image/android/make_jar4.png)

### jarの中のクラスをimportする
com.hogehoge.myclass パッケージの Hoge1 クラスをインポートする場合は
jarを作成する際にjavaファイルの先頭で書いたパッケージ名をつける。

```java
- LogListView.java -
package com.example.mylibrary;
```

```java
- MainActivity.java -
import com.example.mylibrary.LogListView;
```