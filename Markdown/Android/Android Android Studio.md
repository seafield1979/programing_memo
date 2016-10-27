#Android Studio

###ユーザーインターフェース

![](https://developer.android.com/studio/images/intro/main-window_2-1_2x.png?hl=ja)

1. ツールバー
    アプリの実行、Android ツールの起動といった幅広い操作を実行できます。
2. ナビゲーション バー  
    プロジェクト内を移動したり編集するファイルを開くときに役立ちます。 [Project] ウィンドウよりもコンパクトに構成が表示されます。
3. エディタ ウィンドウ
    コードを作成、編集します。 作業中のファイル形式に応じてエディタが変化します。 たとえば、レイアウト ファイルを表示するとエディタはレイアウト エディタになります。
4. ツール ウィンドウ
    プロジェクト管理、検索、バージョン管理など、特定のタスクにアクセスでき、 各領域の展開または折りたたみが可能です。
5. ステータスバー
    プロジェクトと IDE 自体のステータスのほか、警告やメッセージが表示されます。

###設定

フォントサイズ
`Cmd + ,`からPreferencesウィンドウを開き、`Editor - Colors & Defaults`以下の各項目から目的のフォントを設定する。



###必須の機能

xmlの表示切り替え (テキスト <-> Layout)
レイアウトファイルを開いた状態で、エディタウィンドウの左下に`Design/Text`タブがあるのでこれを切り替えるか、`Ctrl + Shift + ← or →`を押す。

![](http://sunsunsoft.com/image/android/layout_switch.png)
↑切り替えタブ

###ショートカット


基本

|コマンド|オススメ度|Mac|Windows|
|---|---|---|---|
|環境設定|★★★|Cmd + ,|


表示系

|コマンド|オススメ度|Mac|Windows|
|---|---|---|---|
|メッセージ        |★★|Cmd + 0|
|プロジェクト管理  |★★|Cmd + 1|
|お気に入り        |★|Cmd + 2|
|Runログ           |★|Cmd + 4|
|Debugログ         |★|Cmd + 5|
|Android Monitor   |★★|Cmd + 6|
|Structure<br>クラスのメソッド一覧|★★★|Cmd + 8|
|Terminal表示      |★★|Opt + F12|
|クラスのメンバ、メソッド一覧表示|★★★|Cmd + F12|
|ツールウィンドウを全表示/非表示|★|Cmd + Shift + F12|
|TextとLayout切り替え|★★★| Ctrl + Shift + ← or →| |
|クラスとレイアウトを切り替え|★★|Cmd + Ctrl + ↑|
|エディタタブの切り替え(←)|★★|Cmd + Shift + @|
|エディタタブの切り替え(→)|★★|Cmd + Shift + [|
|クラスの階層を表示|★★★|Ctrl + H|


実行・デバッグ

|コマンド|オススメ度|Mac|Windows|
|---|---|---|---|
|ビルド|★★|Cmd + F9||
|実行|★★★|Ctrl + R| |
|停止|★★|Cmd + F2||
|再開|★★★|Cmd + Opt + R||
|デバッグ実行|★★|Ctrl + D| |
|再起動|★|Ctrl + Cmd + R| |
|ステップオーバー|★★★|F8||
|ステップイン|★★★|F7||
|ステップアウト|★|Shift + F8||
|カーソル位置まで実行|★|Opt + F9||


編集

|コマンド|オススメ度|Mac|Windows|
|---|---|---|---|
|エディタの選択テキストを上下に移動<br>メソッドブロックを選択するとメソッドの移動が可能|★★★|Cmd + Shift + ↑ or ↓|
|1行コメントアウト|★★★|Cmd + /|
|ブロックコメントアウト|★★★|Cmd + Shift + /|
|Javadocコメントアウト|★★|/** + return|
|シンボルジャンプ|★★★|Cmd + B<br>Cmd+クリック|
|戻る|★★★|Cmd + Opt + ←|
|参照位置表示|★|Opt + F7|

検索

|コマンド|オススメ度|Mac|Windows|
|---|---|---|---|
|エディタ内検索|★★|Cmd + F|
|Grep|★★★|Cmd + Shift + F|
|全て検索|★★★|Shift ２回押し|
|使われている箇所をハイライト<br>解除するにはPreferences の <br>｀Editor - General - Highlight usages of element at caret<br>のチェックをはずす|★|Cmd + F7|

##その他

###コードエディタ中の自動整形をON/OFF

```java
// @formatter:off
自動整形しないコード
// @formatter:on
```