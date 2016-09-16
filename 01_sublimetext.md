<!-- Stylesheet -->
<link href="http://sunsunsoft.com/css/hoge.css" rel="stylesheet"></link>

<span class="title">テキストエディタ Sublime Text</span>

<!-- もくじ -->
[TOC]

#セットアップ setup
Sublime Textをインストール後にまずやっておきたいこと。そのままでも文章の作成ぐらいならできるが、プログラミングに使用するなら開発効率が大きく変わってくるので絶対に行っておきたい。

##パッケージコントロール Package Control
<!-- package control:: -->
Sublime Textではパッケージと呼ばれる機能を拡張する仕組みがあり、これを使うと簡単にいろいろな機能を追加することができる。
パッケージコントロールを使用するにはコンソールからパッケージコントロールをインストールし、コマンドパレット(Cmd + Shift + P(Mac))パッケージコントロール用のコマンドを入力する

###パッケージコントロールのインストール
<a href="https://sublime.wbond.net/installation#st3" target="_blank">Package Control Installation</a>

メニューの View > Show Console　
もしくは Ctrl + ` (` は Shift + @)
でコンソールエディタを開いてに以下のテキストを貼り付けて実行(returnキー)する
 
~~~
 import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read()) 
~~~

###パッケージコントロールの使い方
コマンドパレットを  
  メニュー  `Tool > Command Palette`  
  ショートカット  `Cmd + Shift + P`  
で開いて
Package Control と入力するとコマンドパレットにパッケージコントロールの候補が表示される。  
![](http://sunsunsoft.com/contents/sublimetext/image/package_control.png)

よく使うのは  
  Package Control: Install Package   パッケージのインストール  
  Package Control: Remove Package    パッケージのアンインストール  

###等幅フォントをインストール＆設定
デフォルトの状態だとスペースと半角文字の横幅が異なるのでスペースインデントの幅が行によってバラバラになる(特にMac)。等幅フォントをインストールしてスペースのインデントが揃うようにする。

![](http://sunsunsoft.com/contents/sublimetext/image/font_hanjp.png)  
等幅フォント (Source Han Code JP)  
  
<br>
![](http://sunsunsoft.com/contents/sublimetext/image/font_wingdings.png)  
等幅でないフォント (Wingdings 3 Regular)  
スペースと半角文字の横幅が異なるため、インデント位置がずれる

**等幅フォント(Mac)**

~~~
Macではフォントの横幅が一定でないため、同じもし数でも横幅がバラバラで見づらい。
そこでMacに等幅フォントをインストールして、これを使用する。
「Source Han Code JP」

  https://github.com/adobe-fonts/source-han-code-jp/releases/tag/2.000R

  zipファイルをダウンロードし、OTC/SourceHanCodeJP.ttc を実行すると"Font Book"アプリが立ち上がりフォントが表示される。Font Bookの右下の「フォントをインストール」ボタンを押すとフォントがインストールされる。
~~~

**等幅フォント(Windows)**
~~~
Windowsに Source Han Code JP をインストールして
ユーザー設定ファイルに
  "font_face": "SourceHanCodeJP-Normal",
の行を追加、以下の他のフォント名でもOK
  "font_face": "SourceHanCodeJP-ExtraLight",
  "font_face": "SourceHanCodeJP-Light",
  "font_face": "SourceHanCodeJP-Normal",
  "font_face": "SourceHanCodeJP-Regular",
  "font_face": "SourceHanCodeJP-Medium",
  "font_face": "SourceHanCodeJP-Bold",
  "font_face": "SourceHanCodeJP-Heavy",
~~~

##日本語を使用する
SublimeTextは英語のツールなのでメニューが全て英語。また、日本語変換を入力する際に不具合が発生する。以下の対応で日本語対応する。

###メニューを日本語化
<!-- japanize: -->
![](http://sunsunsoft.com/contents/sublimetext/image/japanese_menu.png)
パッケージコントローラから Japanize パッケージをインストールする。
[http://webkaru.net/dev/sublime-text-3-japanize/](http://webkaru.net/dev/sublime-text-3-japanize/)

###日本語で検索時に文字が消えないようにする
そのままだと、検索パネルに日本語を入力し変換確定の Returnキーを押すと入力したときに入力していた文字が消える。その対処。  
[Sublime Text 3 で日本語を検索したとき文字が消える不具合を直す](http://memo.sanographix.net/post/101061111635)

###F7でカタカナ変換できるようにする
デフォルトではF7にbuild機能が割り当てられているのでこれを無効化する  
[Sublime Text 3でF7でカタカナ変換できなくてぐぬぬってなっている人に向けて書いた記事。](https://wayohoo.com/sublime-text/how-to-be-able-to-use-the-f7-key-in-sublime-text-3.html)

###IMESupport(Windows)
Windows環境で日本語入力変換候補がウィンドウの左上に表示されるのを防いでくれる。
パッケージコントロールから **IME Support** をインストールする

#ファイル・フォルダのパス path
<!-- path:: -->

|内容|パス(Mac)|パス(Windows)|
|---|---|---|
パッケージ<br>以下このフォルダを<br>**~Packages**と置き換えて読んでください | ~/Library/"Application Support"/"Sublime Text 3"/Packages/ | %appdata%\Sublime Text 3\Packages\
ユーザーフォルダ | ~Packages/User | ~Package/User
ユーザー設定 | ~Packages/User/Preferences.sublime-settings | ~Packages/User/Preferences.sublime-settings
ユーザーのキー設定 | ~Packages/User/Default (OSX).sublime-keymap | 
言語設定<br>特定の元の設定を行いたい場合に使用される | ~Packages/*.tmLanguage
スニペット<br>自前で定義した書式で入力補完 | ~Packages/*.sublime-snippet
テーマの設定<br>テーマファイル.tmThemeの一部をカスタマイズしたいときに使用 | ~Packages/*.sublime-theme

#ユーザー設定 User Settings
メニューの  
`Sublime Text > Preferences > Settings User`  
でユーザーの見た目や動作を好みに変更できる。
設定ファイルを直接編集したい場合は以下のファイル  
`~/Library/Application Support/Sublime Text 3/Packages/User/Preferences.sublime-settings`

|有効度|プロパティ|説明|
| :--: |---|---|
  ★★★ | color_scheme          | カラースキーマ<br>`"Packages/Theme - Cobalt2/cobalt2.tmTheme"`<br>メニューの [Preferences] - [Color Scheme] からも設定可能。その場合はこのプロパティが自動で設定される。
  ★★ | theme                 | SublimeTextの外観のテーマ<br>サイドバーやタブバーのレイアウトが変わる。<br>例:Cobalt2.sublime-theme<br> ![](http://sunsunsoft.com/contents/sublimetext/image/cobalt2.png)
  ★★★ | font_face             | フォントタイプ <br>例:`SourceHanCodeJP-Normal`<br>**Macで**Sublime Textで使用出来るフォント名の調べ方は、Font Bookアプリを起動し、詳細表示に切り替えてからフォントを選択すると "Post Script名" に表示される。<br> ![](http://sunsunsoft.com/contents/sublimetext/image/mac_font_name.jpg)
  ★★★ | font_size             | フォントサイズ 例:15<br>ショートカットやメニューからフォントサイズが変更されたら連動する
  ★★★ | tab_size              | タブサイズ 例:4
  ★ | draw_indent_guides    | カーソルがある行のインデント位置に縦線を表示してくれる<br>true/false
  ★★ | draw_white_space      | 半角スペースを表示するか<br>"none":表示しない<br>"selection":選択部分のみ表示<br>"all":常に表示 
  ★ | rulers                | ルーラーを表示する列数のリスト<br>例: [40,80] のように複数設定できる。１つだけ設定する場合でも[]で囲む。
  ★ | gutter                | ガーター(ウィンドウの左側の行数や折りたたみボタンを表示する領域)を表示する true/false<br>![](http://sunsunsoft.com/contents/sublimetext/image/gutter.png)<br>↑ガーターの領域
  ★★ | line_numbers          | 行番号を表示するかどうか true/false<br>![](http://sunsunsoft.com/contents/sublimetext/image/line_number.png)
  ★ | fold_buttons          | ガーターに折りたたみボタンを表示するかどうか true/false
  ★★ | highlight_modified_tabs | 変更したファイルのタブがハイライトされる<br>(Cobalt2のテーマでは黄色い●がついた)<br>![](http://sunsunsoft.com/contents/sublimetext/image/highlight_modified_tabs.png)
  ★ | highlight_line        | カーソル行をハイライトするか true/false<br>trueに設定すると行の背景がうっすらと色が変わる
  ★ | show_full_path        | ウィンドウのバーに表示するファイル名をフルパスで表示するかどうか true/false
  ★ | translate_tabs_to_spaces | タブを入力した際に自動でスペースに変換するか true/false
  ★ | open_files_in_new_window | Finder等からファイルを開いたときに新しいウィンドウで開くかどうか(false推奨)
  ★ | detect_indentation    | ファイルの内容からタブサイズを自動で取得するかどうか true/false<br>タブサイズがスペース８つのファイルを開いた場合は、タブを押したときにスペースが８つ挿入される。
  ★ | trim_automatic_white_space | 改行時に行に存在する無駄なスペースを削除するかどうか true / false
  ★★ | word_wrap             | ワードラップするか。ワードラップはウィンドウに収まらない行を改行して表示する機能<br>true/false/auto<br>(autoの場合はファイルの拡張子によって自動でON/OFF判定する)
  ★★ | wrap_width            |  ワードラップ有効時に何文字で折り返すかどうか<br>0ならウィンドウ幅で折り返す
  ★ | draw_centered         | テキストを中央に表示するかどうか true/false
  ★ | auto_match_enabled    | カッコやコーテーション等を入力時にそれを閉じるための文字を自動で入力するかどうか true/false<br>(),{},'',"" 等で有効
  ★ | word_separators       | ダブルクリックや Ctrl + D で文字列を選択する際に、選択範囲を区切るのに使用する文字列<br>例:"./\\()\"'-:,.;<>~!@#$%^&*|+=[]{}`~?",
  ★★ | file_exclude_patterns | サイドバーに表示するファイルリストから除外するファイルを正規表現("*.xlsx"とか)で設定する。複数設定できるので配列形式
  ★★ | folder_exclude_patterns | サイドバーに表示するファイルリストから除外するフォルダを設定する。
  
#パッケージ package
<!-- package:: -->
パッケージコントロールのインストール  
`Cmd + Shift + P  ->  Package Control: Install Package`  
から以下のパッケージをインストールする。  

|有効度|パッケージ名|説明|
|:--:|---|---|
★★ | Cobalt2      | colorスキーマ。Cobaltをカスタマイズしたもの。渋い色合い。<br>インストールするとメニューの[Preferences - Color Scheme]に項目が追加される
★ | Flatland     | colorスキーマ。ダーク調の渋い色合い
★ | Smart Delete | 改行を削除した際に、次の行のインデントを削除してくれる。
★★★ | Emmet        | 短縮コマンドを展開してhtmlタグを生成するツール<br>[SublimeText Emmet公式集](http://docs.emmet.io/cheat-sheet/)<br>タグの入力を省力化してくれる。
★★★ | BracketHighlighter | HTMLの開始タグ～終了タグや、コードのカッコのペアをわかりやすく表示してくれる。これど導入してこのdivの終了タグってどのdivだ？みたいなのに時間を取られないようにしたい。
★★ | IMESupport | Windows環境で日本語入力変換候補がウィンドウの左上に表示されるのを防いでくれる。
★ | Diffy | 2つのファイルの差分を表示してくれるプラグイン。<br>Shift + Alt + 2(“) を押してウインドウを分割表示し、Ctrl + K + D を押すと量ファイルの差分がハイライトされる。
★★★ | OmniMarkupPreviewer | マークダウン用の機能を使えるようになる。機能が使えるのは .md ファイルのみ<br>プレビュー  Cmd + Opt + O<br>html出力   Cmd + Opt + X<br>目次を展開するには<br>[Preferences] - [Package Settings] - [OmniMarkupPreviewer] - [settings - Default]<br>"extensions": ["tables", "strikeout", "fenced_code", "codehilite","toc"]    <-- "toc"を追加<br>.mdファイル内の目次を挿入したい位置に[TOC]を挿入する。上下に空行がないと正しく出力されないので注意。
★ | Markdown Extended | Markdownに挿入したコードのシンタックスハイライト<br>mdファイルのhtmlコードをハイライトする

#ショートカット shortcut
<!-- shortcut:: -->
SublimeTextは機能が豊富だが、それを呼び出すためにその都度メニューから項目をたどると効率が悪い。ショートカットを覚えて効率を上げたい。

[Keyboard Shortcuts - Windows/Linux](http://docs.sublimetext.info/en/latest/reference/keyboard_shortcuts_win.html)  
[Keyboard Shortcuts - OSX](http://docs.sublimetext.info/en/latest/reference/keyboard_shortcuts_osx.html)  
[ゆとりっち【保存版】Sublime Text 3のショートカットキーの一覧（Windows/Mac）](http://www.starlod.net/sublime-text-3-shortcutkey.html)

###ショートカットの追加方法
ショートカットを登録:
メニュー - [Sublime Text] - [Preferences] - [Key Bindings - User] で設定ファイルを開き、
~~~
{ "keys": ["super+shift+h"], "command": "example" },
~~~
みたいな行を追加する
  
###一般:

|ショートカットキー|Mac|Windows|
|---|---|---|
コンソール(console)を開く<!-- console:: --> | Ctrl + `  // `は Shift + @ | Ctrl + `
コマンドパレット<!-- command palette:: --> | Cmd + Shift + P  | Ctrl + Shift + P |  
フォント拡大 | Cmd + ; | Ctrl + ; 
フォント縮小 | Cmd + - | Ctrl + -
フォント拡大縮小 | --- | Ctrl + Wheel
折りたたみ | Cmd + Opt + [ | Ctrl + Shift + [
折りたたみ展開 | Cmd + Opt + ] | Ctrl + Shift + ]
レベルnで折りたたみ<br>レベル1でトップ階層にあるメソッドやクラスを折りたたむ<br>※カーソルのあるブロックは折りたたまれない | Cmd + K,1-9 | Ctrl + K,1-9
すべて展開 | Cmd + K,J<br>Cmd + K,0 | Ctrl + K,J<br>Ctrl + K,0

**ブックマーク**  
ブックマークはファイルを閉じるとすべて解除される 

|ショートカットキー|Mac|Windows|
|---|---|---|
ブックマーク追加 | Cmd + F2 | Ctrl + F2
ブックマーク解除 | Cmd + Shift + F2 | Ctrl + Shift + F2
次のブックマークに移動 | F2 | F2
前のブックマークに移動 | Shift + F2 | Shift + F2

**ファイル**

|コマンド|Mac|Windows|
|---|---|---|
新しいタブを開く | Cmd + N | Ctrl + N
ファイルを閉じる | Cmd + W | Ctrl + W
ファイルを開く(絞り込み入力ができる)<br>![](http://sunsunsoft.com/contents/sublimetext/image/file_open.png) | Cmd + P | Ctrl + P
次(右)のタブに移動 | Cmd + Shift + ]<br>Cmd + Opt + → | Ctrl + PageDown
前(左)のタブに移動 | Cmd + Shift + [<br>Cmd + Opt + ← | Ctrl + PageUp
  
<br>
 
**ウィンドウを分割**  
分割されたウィンドウはグループと呼ばれる。

|コマンド|画面配置|Mac|Windows|
|---|:--:|---|---|
1画面 | □  | Cmd + Opt + 1          | Shift + Alt + 1   
2 行 | □ □     | Cmd + Opt + 2          | Shift + Alt + 2   
3 行 | □ □ □     | Cmd + Opt + 3          | Shift + Alt + 3   
2 列 | □<br>□     | Shift + Cmd + Opt + 2  | Shift + Alt + 8    
3 列 | □<br>□<br>□     | Shift + Cmd + Opt + 3  | Shift + Alt + 9  
4 グリッド | □ □<br>□ □ | Cmd + Opt + 5          | Shift + Alt + 5  
編集するグループを移動 || Ctrl + 1~4 | Ctrl + 1~4
編集中のファイルをn番目のグループに移動 || Ctrl + Shift + 1~4 | Ctrl + Shift + 1~4

<br>
**カーソル移動**

|コマンド|Mac|Windows|
|---|---|---|
カーソルを１文字進む | Ctrl + F | ---
カーソルを１文字戻る | Ctrl + B | ---
カーソルを１行進む | Ctrl + N | ---
カーソルを１行戻る | Ctrl + P | ---
カーソルを１ページ分上にスクロール | Fn + ↑ | Pageup
カーソルを１ぺージ分下にスクロール | Fn + ↓ | Pagedown
★対応するカッコに移動 | Ctrl + M | Ctrl + M

<br>
**編集系**

|コマンド|Mac|Windows|
|---|---|---|
コピー(選択状態でなければ現在行をコピー) | Cmd + C | Ctrl + C
履歴から貼り付け | Cmd + K,V | Ctrl + K,V 
インデント<br>複数行をまとめて行うことも可能 | Cmd + ] | Ctrl + ]
アンインデント | Cmd + [ | Ctrl + [
複数行を選択状態でインデント | Cmd + Tab | Ctrl + Tab
複数行を選択状態でアンインデント | Cmd + Shift + Tab | Ctrl + Shift + Tab
行の移動 | Ctrl + Cmd + ↑ or ↓ | Ctrl + Shift + ↑ or ↓
現在行を複製(下の行に挿入される) | Cmd + Shift + D | Ctrl + Shift + D
行末まで削除 | Cmd + K,K | Ctrl + K,K
行の削除 | Ctrl + K | ---
行の削除(改行も) | Ctrl + Shift + K | Ctrl + Shift + K
行を結合<br>(カーソルのある行とその次の行を結合) | Cmd + J | Ctrl + J 
カーソル行の前に改行を挿入 | Cmd + Shift + Return | Ctrl + Shift + Enter
コメントアウト<br>※すでにコメントアウトされていた場合はコメントアウトを削除<br>※挿入されるコメントはファイルフォーマットに依存<br>(txtファイル等の非プログラミング言語のファイルは挿入されない) | Cmd + / | Ctrl + /
範囲コメントアウト<br>※コメントブロック内ではコメントアウトを削除<br>※範囲コメントアウトのない言語では通常のコメントアウト | Cmd + Shift + / | Ctrl + Shift + /

<br>
**検索・置換系**

|コマンド|Mac|Windows|
|---|---|---|
検索パレットを開く | Cmd + F | Ctrl + F
置換 | Cmd + Opt + F | Ctrl + H
次を置換 | Cmd + G | Ctrl + Shift + H
Grep(複数ファイルから検索) | Cmd + Shift + F | Ctrl + Shift + F
関数の定義に移動(関数ジャンプ) | Cmd + R | Ctrl + R
ファイル名で検索(絞り込み機能付き)<br>"フォルダ名 ファイル名"<br>と指定すると特定のフォルダ以下のファイルに絞る込むことができる。 | Cmd + P | Ctrl + P
正規表現トグル<br>※検索パネルにカーソルがある状態で有効 | Cmd + Opt + R | Alt + R
検索文字列を全選択 | Opt + Return | Alt + Enter
カーソル位置の文字列を全検索して選択状態にする | Cmd + Ctrl + G | Alt + F3
    
<br>
**変換**

|コマンド|Mac|Windows|
|---|---|---|
選択範囲をすべて大文字に変換する | Cmd + K,U | Ctrl + K,U
選択範囲をすべて小文字に変換する | Cmd + K,L | Ctrl + K,L

<br>
**選択** 

|コマンド|Mac|Windows|
|---|---|---|
矩形選択 | Opt を押しながら、左クリックしドラッグ | Shiftを押しながら、右クリックしドラッグ
複数箇所選択<br>カーソル位置が複数選択に追加される | Cmd + 左クリック | Ctrl + 右クリック

<br>
**html**

|コマンド|Mac|コマンド(Win)|
|---|---|---|
開始タグと終了タグを行き来する | Ctrl + Shift + T | ---
親要素をたどる | Cmd + Shift + A | Ctrl + Shift + A
選択範囲を広げる | Ctrl + d | ---
選択範囲を縮める | Ctrl + j | ---

<br>

#DropBoxで同期
<!-- dropbox -->
複数のマシンでSublime Textを使用する場合、それぞれの環境で一から設定を行うのは面倒なので DropBoxを使用して同期させる方法を説明する。

仕組み的には Sublime Text のユーザー設定フォルダの向き先をそっくりDropBox以下のリンクを向くようにする。
Sublime Textの設定ファイルは Packages/User というフォルダに入っている。これをDropBoxの適当な場所に移動して、Sublime Textから参照するリンクに置き換える。

手順は以下

 1. /Sublime Text 3/Packages/User -> Dropbox/Sublime/User に移動
 2. /Sublime Text 3/Packages/User を削除
 3. Dropbox/Sublime/User のリンクを作成し、/Sublime Text 3/Packages/ 配下にコピー

[Package Control Syncing](https://packagecontrol.io/docs/syncing)  

**Windows**  
コマンドプロンプトから以下のコマンドを入力
~~~bat
# 初回設定(1台目のPC)
# Dropbox以下にSublime Textの設定ファイルを移動する
cd "%appdata%\Sublime Text 3\Packages\"
mkdir %homepath%\Dropbox\Sublime
mv User %homepath%\Dropbox\Sublime\
cmd /c mklink /D User %homepath%\Dropbox\Sublime\User

# 次回設定(2台目以降のPC)
cd "%appdata%\Sublime Text 3\Packages\"
rmdir -recurse User
cmd /c mklink /D User %homepath%\Dropbox\Sublime\User
~~~
  
**Mac**  
ターミナルから以下のコマンドを入力
~~~sh
# 初回設定(１代目のMac)
# Dropbox以下にSublime Textの設定ファイルを移動する
cd ~/Library/"Application Support"/"Sublime Text 3"/Packages/
mkdir ~/Dropbox/Sublime
mv User ~/Dropbox/Sublime/
ln -s ~/Dropbox/Sublime/User

# 次回設定(2代目以降のMac)
cd ~/Library/"Application Support"/"Sublime Text 3"/Packages/
rm -r User
ln -s ~/Dropbox/Sublime/User
~~~

※WindowsとMacで同じDropboxの ~Packages/User フォルダを参照すると環境の違いから不具合が起こる場合があるため、WindowsとMac両方を同期する場合はフォルダを分けるとよい。  
例:  
Win : ~/Dropbox/SublimeWin/User  
Mac : ~/Dropbox/SublimeMac/User  

#snippet登録
入力の補完を行ってくれる。例えば事前にスニペットを登録しておくと  
myname と入力後 Tabキーを　押すと  
  `My name is [name].`  
と入力され、nameの部分に好きな文字列を入力できる。テンプレの長い文字列を打たなくてよくなるためコーディングが楽になるはず。

ちゃんと知りたい場合は
[開発効率アップ！Sublime Textのスニペット機能の使い方（登録方法など）](http://nelog.jp/how-to-set-sublime-text-snippet)を参照。かなり詳しく書かれてある。

###Snippetを新規登録

* メニューの [Tool] - [Developer] - [New snippet] を選択。  
    新規snippet用のファイルが開く。  
* snippetファイルを編集  
~~~
  <content>"補完するテキスト <![CDATA[補完テキスト]]>"</content>"
  <tabTrigger>補完トリガーになるテキスト</tabTrigger>  
  <scope>対象になるファイル(text.plain,text.htmlとか)</scope>  

例  -hoge.sublime-snippet
　　<snippet>
    <content><![CDATA[My name is ${1:name}.]]></content>
    <tabTrigger>myname</tabTrigger>
    <scope>text.plain,text.html,text.html.markdown</scope>
  </snippet>
~~~

* ファイルを保存
    - 保存場所は **Packages** フォルダ以下ならどこでもOK
    - ファイル名の拡張子は "**.sublime-snippet**"

* スニペットを使うには
    - トリガーになる文字を入力後 Tabキーを押すか
    - メニューの [Tool] - [Snippets...] で表示される候補から選択する。


#プラグイン plugin
<!-- plugin -->
プラグインを使うとSublimeTextの機能を組み合わせて様々な処理を行うことができる。プラグインのプログラミング言語はPythonで、SublimeText用のAPIが大量に用意されてあるので頑張ればかなり高度な機能を実現することができる。多くのパッケージはこのプラグインで実装されている（と思う）。

SublimeTextのプラグイン作成方法
[SublimeTextのプラグイン作成方法](http://inet-lab.naist.jp/blog/how-to-make-a-sublime-text-2-plugin/)

###プラグイン作成
Tools > Developer > New Plugins ...  
でデフォルトのプラグインが生成される。  
これを適当な名前で保存 ~Packages/User/hoge.py  



デフォルトのプラグイン
~~~python
import sublime, sublime_plugin

class ExampleCommand(sublime_plugin.TextCommand):
    def run(self, edit):
        self.view.insert(edit, 0, "Hello, World!")
~~~
class ExampleCommand の **example** がこのプラグインを実行するための名前になる  
class HelloWorldCommand の場合は **hello_world** になる。

###プラグイン呼び出し
**コマンドラインから呼び出す**  
  Cmd + `
  でコマンドラインを開いてから、 
  `view.run_command("example");`  

**ショートカットから呼び出す**  
  [Preferences] - [Key Bindings - User] で "Default (OSX.sublime) - keymap"を編集  
  []の中に以下の行を追加する  
   `{ "keys": ["super+shift+h"], "command": "example" }`

  これでショートカットキー  
  `Cmd + Shift + H　`  
  でexampleプラグイン(class名はExampleCommand)が実行される  

**コマンドパレットから呼び出す**  
コマンドパレット(ショートカット:Cmd + Shift + P)に登録するには  
[Sublime Root]/User/Default.sublime-commands  
に以下のような定義を追加する  
~~~
  [
    {
        "caption": "Example: example",
        "command": "example"
    },
    ...
  ]
~~~

###現在の日付を貼り付けるプラグイン
Sublime Textには日付挿入の機能が存在しないため自前のプラグインを作成する。
[SublimeTextでかんたんにタイムスタンプを挿入させる](http://b.hishitu.net/4658.html)

~~~python
import sublime, sublime_plugin
from datetime import datetime

class TimestampCommand(sublime_plugin.TextCommand):
  def run(self, edit):
    stamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S ")
    for r in self.view.sel():
      if r.empty():
        self.view.insert (edit, r.a, stamp)
      else:
        self.view.replace(edit, r,   stamp)
~~~
~/Packages/User/timestamp.py　に保存  
ショートカットキーに登録する (~/Packages/User/Default (OSX).sublime-keymap)
{ "keys": ["super+shift+h"], "command": "timestamp" },

###ファイル名からパスを指定してファイルを開く 
sublimetext上でパスを指定してファイルを開く方法。プラグインを作って登録する。
~~~python
import sublime, sublime_plugin

def open_file(window, filename):
    window.open_file(filename, sublime.ENCODED_POSITION)

class OpenFileByNameCommand(sublime_plugin.WindowCommand):
    def run(self):
        fname = self.window.active_view().file_name()
        if fname == None:
            fname = ""

        def done(filename):
            open_file(self.window, filename)

        self.window.show_input_panel(
            "file to open: ", fname, done, None, None)
~~~

ショートカットを登録
[Preferences] - [Key Bindings - User] で "Default (OSX.sublime) - keymap"を開き  
~~~python
{ "keys": ["super+shift+o"], "command": "open_file_by_name" }
~~~

ショートカットから呼び出し:
`Cmd + Shift + O` を押すとSublimeText下に "file to open"というテキストボックスが開くので
/User/shutaro/hoge.txt  
とか  
~/.bashrc  
とかのパスを入力してファイルを開く。  s

#その他・便利機能 others

###サイドバーのフォントサイズ変更
<!-- sidebar:: -->
Sublime Text -> Preferences -> Browse Packages
Open "User"
"Default.sublime-theme" を新規作成。(themeを変更していた場合は"[テーマ名].sublime-theme"を作成)
~~~python
[
    {
        "class": "sidebar_label",
        "color": [150, 150, 150],
        "font.bold": false,
        "font.size": 12
    },
]
~~~

##テキストの見出し検索
テキストファイル(.txt)でドキュメントをまとめた場合などに、テキストが大きくなるとどこにどんな内容が書いてあるのかの把握が難しくなる。そこで、テキストファイルの中の見出し行を書いてその見出しの一覧を表示できるようにする。  
![](http://sunsunsoft.com/contents/sublimetext/image/text_midashi1.png)  
テキストファイルに見出し文字(■や●)を配置すると  
  
![](http://sunsunsoft.com/contents/sublimetext/image/text_midashi2.png)  
関数ジャンプの候補として見出し文字の行が表示される  

###テキストファイルの言語パッケージ定義（テキストのカラー変更）
パッケージインストールから PackageDev をインストールする  
  
* メニュー[ツール] - [Packages] - [Package Development] - [New Syntax Defenition] を選択でテンプレートファイルが開く。ファイルに保存 -> test1.YAML-tmLanguage  

  テンプレートファイル(test1.YAML-tmLanguage)の編集  

~~~
# [PackageDev] target_format: plist, ext: tmLanguage  
---
name: Original Text
scopeName: text.txt
fileTypes: [txt]
uuid: 1ac4a43b-be13-4075-8542-79c7beb4074b

patterns:
- name: markup.italic
  begin: (-src)
  end: (src-)
  comment: ソースコード

- name: entity.name.function.topic
  match: ^(■|▲|▼)(.*)
  comment: 見出し検索にヒットする

- name: markup.italic
  match: ^(\s*)([#$]\s).*
  comment: プロンプト

- name: comment.line
  match: ^(\s*)([:;#]|//)(.*)

- name: comment.language.text
  match: (--(.*))|[■●▲★](.*)(.*)

- name: keyword.control
  match: ^(\s*\w+:+)
  comment: タイトル
~~~
   
  * 保存してからメニューの[Tool] - [Build System] - [Convert to] を選択、F7キーを押す
  * 初回のみどのフォーマットにコンバートするかを選択するポップアップが出るので [JSON to Property List] を選択する。
  * Sublime/User フォルダ以下に test1.tmLanguage というファイルが出力される。このファイルが言語の色をカスタマイズするファイルで、SublimeTextはこのファイルを元に各ファイルの色を変更したり、構造を解析したりする。

###エラー対応
####Unable to open Packagesエラー
tmLanguage ファイルを削除するとSublime Text起動時に  
~~~
Error loading syntax file "Packages/...tmLanguage": Unable to open Packages/.../LaTeX.tmLanguage
~~~
みたいなメッセージのエラーが出る

**対応策**
Win:  
C:\Users\shutaro\AppData\Roaming\Sublime Text 3\Local\Session.sublime_session の エラーを起こしている tmLanguageを探して削除する  

####日本語の下に赤い波のラインが表示されるようになる  
![](http://sunsunsoft.com/contents/sublimetext/image/error_underline.png)  
こういう赤い波線が表示された場合  

シンタックスハイライトが有効になっているので F6 でオフにする。