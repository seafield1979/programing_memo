#セットアップ setup
Sublime Textをインストール後にまずやっておきたいこと。そのままでも文章の作成ぐらいならできるが、プログラミングに使用するなら開発効率が大きく変わってくるので絶対に行っておきたい。

##パッケージコントロール Package Control
Sublime Textではパッケージと呼ばれる機能を拡張する仕組みがあり、これを使うと簡単にいろいろな機能を追加することができる。
パッケージコントロールを使用するにはコンソールからパッケージコントロールをインストールし、コマンドパレット(Cmd + Shift + P(Mac))パッケージコントロール用のコマンドを入力する

###パッケージコントロールのインストール
<a href="https://sublime.wbond.net/installation#st3" target="_blank">Package Control Installation</a>

メニューの 
`View > Show Console`
もしくは $Ctrl + `$
$`$ は
`Shift + @`

でコンソールエディタを開いてに以下のテキストを貼り付けて実行(returnキー)する
 
~~~
 import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read()) 
~~~

##パッケージコントロールの使い方
コマンドパレットを  
  メニュー  `Tool > Command Palette`  
  ショートカット  `Cmd + Shift + P`  
で開いて
Package Control と入力するとコマンドパレットにパッケージコントロールの候補が表示される。  
![](http://sunsunsoft.com/contents/sublimetext/image/package_control.png)

よく使うのは  
  `Package Control: Install Package`   パッケージのインストール  
  `Package Control: Remove Package`    パッケージのアンインストール  

##等幅フォントをインストール＆設定
デフォルトの状態だとスペースと半角文字の横幅が異なるのでスペースインデントの幅が行によってバラバラになる(特にMac)。等幅フォントをインストールしてスペースのインデントが揃うようにする。

![](http://sunsunsoft.com/contents/sublimetext/image/font_hanjp.png)  
等幅フォント (Source Han Code JP)  
  
<br>
![](http://sunsunsoft.com/contents/sublimetext/image/font_wingdings.png)  
等幅でないフォント (Wingdings 3 Regular)  
スペースと半角文字の横幅が異なるため、インデント位置がずれる

###等幅フォント(Mac)


Macではフォントの横幅が一定でないため、同じもし数でも横幅がバラバラで見づらい。
そこでMacに等幅フォントをインストールして、これを使用する。
[「Source Han Code JP」](https://github.com/adobe-fonts/source-han-code-jp/releases/tag/2.000R)


zipファイルをダウンロードし、OTC/SourceHanCodeJP.ttc を実行すると"Font Book"アプリが立ち上がりフォントが表示される。Font Bookの右下の「フォントをインストール」ボタンを押すとフォントがインストールされる。


###等幅フォント(Windows)
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
こんな感じになる
![](http://sunsunsoft.com/contents/sublimetext/image/japanese_menu.png)
パッケージコントローラから Japanize パッケージをインストールする。
[Japanize](http://webkaru.net/dev/sublime-text-3-japanize/)

###日本語で検索時に文字が消えないようにする
そのままだと、検索パネルに日本語を入力し変換確定の Returnキーを押すと入力したときに入力していた文字が消える。その対処。  
[Sublime Text 3 で日本語を検索したとき文字が消える不具合を直す](http://memo.sanographix.net/post/101061111635)

###F7でカタカナ変換できるようにする
デフォルトではF7にbuild機能が割り当てられているのでこれを無効化する  
[Sublime Text 3でF7でカタカナ変換できなくてぐぬぬってなっている人に向けて書いた記事。](https://wayohoo.com/sublime-text/how-to-be-able-to-use-the-f7-key-in-sublime-text-3.html)

###IMESupport(Windows)
Windows環境で日本語入力変換候補がウィンドウの左上に表示されるのを防いでくれる。
パッケージコントロールから **IME Support** をインストールする

###文字列コピペで半濁点が分解されないようにする
SublimeTextにテキストを貼り付けた際に、デフォルトだと ダ -> タ゛、パ -> ハ゜のように２つの文字に分解されるので、濁点(゛)や半濁点(゜)が分解されないようにする。
パッケージコントロールから **SublimeNFDToNFCPaste** をインストールする


#ファイル・フォルダのパス path

|内容|パス(Mac)|パス(Windows)|
|---|---|---|
|パッケージ<br>以下このフォルダを<br>**~Packages**と置き換えて読んでください | ~/Library/"Application Support"/"Sublime Text 3"/Packages/ | %appdata%\Sublime Text 3\Packages\
|ユーザーフォルダ | ~Packages/User | ~Package/User
|ユーザー設定 | ~Packages/User/Preferences.sublime-settings | ~Packages/User/Preferences.sublime-settings
|ユーザーのキー設定 | ~Packages/User/Default (OSX).sublime-keymap | 
|言語設定<br>特定の元の設定を行いたい場合に使用される | ~Packages/*.tmLanguage
|スニペット<br>自前で定義した書式で入力補完 | ~Packages/*.sublime-snippet
|テーマの設定<br>テーマファイル.tmThemeの一部をカスタマイズしたいときに使用 | ~Packages/*.sublime-theme