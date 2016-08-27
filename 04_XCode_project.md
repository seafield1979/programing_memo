<!-- Stylesheet -->
<link href="http://kevinburke.bitbucket.org/markdowncss/markdown.css" rel="stylesheet"></link>
<link href="http://sunsunsoft.com/css/hoge.css" rel="stylesheet"></link>

<span class="title">Xcode プロジェクトファイル(.xcodeproj)</span>

<!-- もくじ -->
[TOC]

プロジェクトファイルを開くにはプロジェクトナビゲータにあるプロジェクトのアイコンをクリックする。
![open](http://sunsunsoft.com/image/xcode/xcode_project.jpg)

プロジェクトファイル(.xcodeproj)の設定

右側のプロジェクトファイルをクリックするとプロジェクトの設定が開く。重要な設定をまとめてみた。
#マクロ
<!-- macro:: -->
##プロジェクトファイル中で使用されるマクロ
$(マクロ名)でプロジェクトの各項目の設定値にあらかじめ定義された or 自分で作成した定義名を参照することができる。
  
  |マクロ名|内容|
  |!--|!--|
  inherited | 上位プロジェクトの設定を引き継ぐ
  SRCROOT |   プロジェクトファイルのあるパス<br>"$(SRCROOT)/../../
  Documents/FacebookSDK"<br>みたいに使用する
  BUILD_ROOT | ビルド結果が出力されるパス
  PRODUCT_NAME | アプリの名前
  BUILT_PRODUCTS_DIR | ビルド後のファイル出力先のディレクトリ
  HEADER_SEARCH_PATHS | ヘッダーファイルの検索パス
  PRODUCT_BUNDLE_IDENTIFIER | デフォルトのBundle ID。plistの Bundle Identifier には $(PRODUCT_BUNDLE_IDENTIFIER)が設定されている。

##マクロの一覧を取得
$(SRCROOT)等のマクロ名の一覧を取得する方法。
プロジェクトファイルの [Build Phases] の [Run Script] に以下のスクリプトを追加する  

    env > env.txt

このあとプロジェクトをビルドするとプロジェクトの直下にマクロの一覧が env.txt に出力される。  
※ [Run Script]がない場合は 左上の [+]ボタンを押してから "New Run Script Phases" を押す。
![マクロを出力](http://sunsunsoft.com/image/xcode/output_macros.png)

#プロジェクトの項目の説明
![プロジェクトのメニュー](http://sunsunsoft.com/image/xcode/xcode_project_menu.png)


##Generalタブ
###Identity
  |項目|説明|
  |!--|!--|
  Bundle Identifier | Bundle ID、バンドル名、ともいう。App Storeで公開されるアプリの中で名前がかぶらないように、ベンダーのドメインを逆順にしたもの + アプリ名にするのが通例（開発者のドメインが hogehoge.co.jp、アプリ名が MyApp の場合は jp.co.hogehoge.MyApp のようになる)。
  Version | アプリのバージョン。1.2.3のように３つの数字を.で区切って設定する。プログラムからは<br> [[[NSBundle mainBundle] infoDictionary] objectForKey:@"CFBundleVersion"];<br> で参照できる。<br>詳しくは[iOSアプリバージョンについて](http://qiita.com/mo12ino/items/667f9f3fd3829440a20b)
  Build | ビルドバージョン。1.2.3のように３つの数字を.で区切ってセッッ呈する。ユーザーには見えない内部のバージョン。アプリを申請する時にこのバージョンが同じだと以前のものと同じだと見なされてリジェクトされる。

###Deployment Info
  |項目|説明|
  |!--|!--|
  Deployment Target      | これ以上のiOSバージョンで動作するという情報。7.0ならiOS7.0以上の端末にのみインストール可能。
  Devices                | どの端末(iPhone / iPad)で動くようにするか。iPhoneと設定するとApp StoreでiPhoneからしかインストール出来なくなる。Universal と設定すると iPhoneでもiPadでもインストールできる。
  Main Interface         |       使用するStoryboardを設定する。空白だとストーリーボードを使用しない。
  Device Orientation     |   画面の向き。全てチェックを入れると現在の端末の向きによって自動でアプリの画面の向きも変わる。縦画面だけ対応する場合は Portrait だけにチェックする。
  Status Bar Style       | ステータスバーの見た目を設定する。ためしにDefaultとLightを見てみたが違いがわからなかった。
  Hide status bar        | チェックするとステータスバーが消える。と思ったけど、ここの設定では消えないようだ。ステータスバーを消すには info.plist に <br>View controller-based status bar appearance を「NO」<br>を追加する
  Requires full screen | iOS9でマルチタスキング機能が追加になったことで増えた項目。マルチタスキングで画面を２つのアプリで共有している場合はそれぞれのアプリのサイズを変更できるが、この項目をチェックすることでサイズの変更ができなくなる。

###App Icons and Launch Images
  |項目|説明|
  |!--|!--|
  App Icons Source | アプリのアイコンをどこから持ってくるか。デフォルトだと"AppIcon"になっている。これはプロジェクトに含まれるAssets.xcassets に含まれるデフォルトのアイコンセットの名前。
  Launch Images Source | アイコンが含まれる.xcassetsファイルの名前。デフォルトだとプロジェクト作成時に自動生成されるAssets.xcassetsが選択されている。
  Launch Screen File | アプリ起動時にズームされながら表示される画面。

 

##Infoタブ
info.plistの内容。ここで設定した値は General タブの項目で使用されたりする。
###Custom iOS Target Properties

|項目|説明|
|!--|!--|
Bundle Identifier | Bundle ID、バンドル名、ともいう。App Storeで公開されるアプリの中で名前がかぶらないように、ベンダーのドメインを逆順にしたもの(リバースドメイン) + アプリ名にするのが通例（開発者のドメインが hogehoge.co.jp、アプリ名が MyApp の場合は jp.co.hogehoge.MyApp のようになる)。
Bundle name | デフォルトは${PRODUCT_NAME}、PRODUCT\_NAMEは Build Settings の Packagingの下にある。
Bundle version | 1.0.0みたいな形式のビルド番号。Bundle version string, shortと合わせること 
Bundle version string,short | 1.0.0みたいなバージョン番号
Application requires iPhone environment | これがチェックされていると、bundle(実行ファイル) は iOSのみ動く。YESにしておく(デフォルト)。iPhoneだけという意味ではないので、iPadを対象外にするためにNOにするのは間違い
Localization native development region | アプリのデフォルト言語
日本語にするためにJapanを設定
Icon already includes gloss effects | NOにすると光沢なしの画像と判定され、iOSが光沢をつけてくれる。

##Build Settingタブ

###Code Signing
|項目|説明|
|!--|!--|
Code Signing Entitlements | 
Code Signing Identity | コード作成者の署名ファイル。端末にアプリを転送するのに必要
Provisioning Profile | プロビジョニングプロファイルの項目を参照

###Packaging
|項目|説明|
|!--|!--|
info.plist File | プログラムで使用するplistファイルのパス。デフォルトは[プロジェクト名]/info.plist
Private Headers Folder Path |
Product Bundle Identifier |
Product Name | デフォルトは $(TARGET_NAME)。ここの名前を変更すると別アプリとしてインストールされる。デバッグ版ととリリース版のアプリを同時にインストールしたい場合は、ここの名前を別々にするのがいいかも
Public Headers Folder Path | 静的ライブラリを作って後悔する場合に設定する？

###Seach Paths
Framework Search Paths | フレームワーク(.framework)の検索パス。読み込みたいフレームワークのあるフォルダを相対 or 絶対パスで指定する。パスは""で囲むこと
Header Search Paths | ヘッダー(.h)の検索パス
Library Search Paths | ライブラリファイル(.a)の検索パス

###User-Defined
ここに自前のマクロ名を追加できる。  
追加したマクロは __$(マクロ名)__ で参照できる。  

追加方法は プロジェクト設定ウィンドウの上の方にある [+] を押してから、"Add User-Defined Setting" を押す。マクロは各ビルド構成毎に個別の値を設定できる。デフォルトではDebugとReleaseの２つ。

![Add User-Defined](http://sunsunsoft.com/image/xcode/add_user_defined1.jpg)  
<br>
![Add User-Defined](http://sunsunsoft.com/image/xcode/add_user_defined2.jpg)

##Build Phasesタブ
ビルドの順番。上から順に処理される。Target Dependencies以外はドラッグ＆ドロップで自由に順番を入れ替えることができる。Run Scriptを追加して、それを先頭に持ってくることで毎回ビルド前に自前の処理を行うことも可能。

###Target Dependencies
利用したいスタティックライブラリ(.a)を追加する

###Compile Sources
コンパイルするソースを追加する。通常はプロジェクトに追加したソースファイルが自動的に追加される。プロジェクトには追加したけどコンパイルはしたくない場合などに、このリストから削除するなどする。

###Link Binary With Libraries
使用する外部ライブラリ(.framework .o .tbd)を追加する。標準で使えるフレームワークはここで

###Copy Bundle Resources
プロジェクトにパッケージするリソースファイル一覧。リソースファイル(画像ファイル、xib、Assetファイル等)はプロジェクトに登録すると自動で追加されるが、何かしらの理由でここのリストから消えていたりする場合があるので、リソースが読み込まれないときなどはちゃんとリストにあるかどうか確認してみるとよい。

###Run Script
自前のスクリプトを追加できる。
ビルド前に最新の外部ファイルをコピーしたり、ビルド後に出力されたアプリをコピーしたりできる。

__Input Files__ はスクリプトへの入力用のファイル。スクリプト内で参照するには以下。
~~~sh
${SCRIPT_INPUT_FILE_COUNT}  入力ファイルの数
${SCRIPT_INPUT_FILE_0}  1つめの入力ファイルパス
${SCRIPT_INPUT_FILE_1}  2つめの入力ファイルパス
~~~
__Output Files__ はスクリプトへの出力用のファイル。スクリプト内で参照するには以下。
~~~sh
${SCRIPT_OUTPUT_FILE_COUNT}
${SCRIPT_OUTPUT_FILE_0}  1つ目の出力ファイル
${SCRIPT_OUTPUT_FILE_1}  2つ目の出力ファイル
~~~
サンプル
~~~sh
echo ${HOGE} > hoge.txt;
cat "${SCRIPT_INPUT_FILE_0}" >> hoge.txt
echo "hogehoge" >> "${SCRIPT_OUTPUT_FILE_0}"
~~~

![Run Script](http://sunsunsoft.com/image/xcode/xcode_run_script.jpg)
