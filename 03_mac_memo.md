<link href="http://kevinburke.bitbucket.org/markdowncss/markdown.css" rel="stylesheet"></link>
<link href="http://sunsunsoft.com/css/hoge.css" rel="stylesheet"></link>

<span class="title">Macメモ</span>

[TOC]

#基本 basic

###キー入力 
<!-- key:: -->
    \  バックスラッシュ  
    内蔵キーボード    `option + ¥`  
    外付けキーボード  `右のcommand + ¥`  
    ※sublime textだと表示上は ¥ と \の区別がつかない

#セットアップ setup
<!-- setup:: -->
新しいMacを入手したら最初にやっておきたいこと。これらのことを行うと使い勝手が向上するかも。

##トラックパッド
  **ダブルタップでドラッグ**  
  デフォルトだとドラッグをするために手（というか指）を必要とするため、ダブルタップでドラッグ開始するようにして指一本でもドラッグ＆ドロップできるようにする。  

  `システム環境設定 - アクセシビリティ - マウス/トラックパッド - トラックパッドオプション` でドラッグを有効にするにチェック
  右側のリストから「ドラッグロックあり」を選択

##Finderに隠しファイルを表示する
  デフォルトだとFinder .で始まる隠しファイルが表示されないので、ターミナルから以下のコマンドを入力する
~~~sh
$defaults write com.apple.finder AppleShowAllFiles -boolean true
$killall Finder
~~~
##アプリ内のウィンドウを切り替えるショートカット変更
デフォルトでは同じアプリで複数のウィンドウを開くもの（例えばXCodeは1プロジェクト毎に1ウィンドウが開く)を切り替える場合、Dockのアイコンからウィンドウを切り替えるか、Exposeから目的のウィンドウを探さなくてはならない。これは面倒なので `Opt + Tab` で同じアプリのウィンドウを切り替えられるようにする。

  [同一アプリ内の複数ウインドウをショートカットキーで切り替える](http://m.designbits.jp/12070412/)  
  Opt + Tab でアプリ内のウィンドウを切り替えられるようになる。

##ポップアップの項目をキーボードで選択する
  ![ポップアップ](http://sunsunsoft.com/image/memo/mac_popup.png)
  macではデフォルトではファイルの保存確認等のポップアップをマウスカーソルでしか選択できない。
  キーボードを使っても選択できるようにする設定方法

  `[システム環境設定] - [キーボード] - [ショートカット] - [Mission Control]` と選択後、
    `「フルキーボードアクセス」`を`「すべてのコントロール」`にチェック

  ポップアップの項目の選択方法 

  |キー|説明|
  |!--|!--|
    Tab | 次の項目(右へ)
    Shift + Tab | 前の項目(左へ)
    space | Tabで選択した項目の決定


#OSの機能
##ショートカットキー
Macは便利なショートカットキーがたくさん用意されているので、必要なものを覚えていくと作業効率がアップするかもしれない。
<!-- shortcut:: -->

|ショートカット|コマンド|
|!--|!--|
  Cmd + Tab | アプリを切り替え
  Cmd + F1 <br>(Opt + Tab) | 同一アプリのウィンドウ切り替え (キーボード - ショートカット - キーボード から カスタマイズ可能)
  Cmd + H |            アプリを隠す
  Cmd + M |            アプリの最小化
  Cmd + Q |            アプリ終了
  Cmd + Opt + H  |     アクティブなウィンドウ以外を隠す
  Cmd + Shift + S |    別名で保存
  Ctrl + スペース |      spotlight起動
  Ctrl + ← or → |     スクリーンのページ切り替え
  Ctrl + ⬆ |           Expose
  
  **finder**

|ショートカット|コマンド|
|---|---|
  Cmd + ↓<br>Cmd + O |  ファイル選択
  Cmd + Shift + N |     フォルダ新規作成
  Cmd + [ |        戻る
  Cmd + ] |        進む
  Cmd + i |        (ファイルを選択状態で)ファイルの情報を表示

  **スクリーンショット**

  |コマンド|説明|
  |!--|!--|
  Cmd + Shift + 3 | 画面全体
  Cmd + Shift + 4 | ドラッグで選択した部分
  Cmd + Shift + 4, space | カメラアイコンで選択した一つのウインドウ

  **spectacle**

|ショートカット|コマンド|
|---|---|
  Cmd + Opt + F | 全画面: 
  Cmd + Opt + ← or → | 画面左右半分: 
  Cmd + Opt + ↑ or ↓ | 画面上下半分: 




#Finder
<!-- finder:: -->
###デフォルトの検索位置を設定  
  Finder - `メニュー[環境設定] - [詳細]`  
  検索実行時: を**「現在のフォルダ内を検索」**に設定

###ショートカット
  |ショートカット|説明|
  |!--|!--|
  Cmd + O | ファイル実行
  Return | 名前を変更
  Cmd + Shift + N | フォルダを作成
  Cmd + Shift + G | パスを指定して移動<br>/ ルートに移動<br>~ ユーザールートに移動
  Ctrl + Cmd + T | よく使う項目に追加

###パス名をコピー
[El Captain以降]  
ファイル/フォルダを右クリックしてポップアップを表示 -   
**Option** キーを押すと  
"~をコピー" が  
"~のパスをコピー" に変わる

###複数のウィンドウを結合
Finderメニューの `[ウィンドウ] - [すべてのウィンドウを結合]`


#ツール tools
<!-- tool: tools: -->
##Spectacle
キーボードからウィンドウの移動、リサイズができるようになる。 Windowsで `Winキー + ← or →` でウィンドウを画面の左右端に移動できる機能があり便利だったので、Macでも同じような操作ができないか探していたところこのアプリを見つけた。  
[Macの画面を自在に操り作業効率アップするアプリ「Spectacle」](http://www.smile-hack.com/entry/spectacle)

![](http://sunsunsoft.com/image/memo/app_spectacle.png)  
ウィンドウを移動するショートカットキーがたくさん用意されている

![](http://sunsunsoft.com/image/memo/spectacle_before.png)  
ウィンドウ移動前
![](http://sunsunsoft.com/image/memo/spectacle_done.png)   
ウィンドウ移動後

 
##New File Menu Free (App Store)  
Finderの右クリックのポップアップに テキストファイルの新規作成を追加  
![](http://sunsunsoft.com/image/memo/NewFileMenuFree_popup.png)
![](http://sunsunsoft.com/image/memo/NewFileMenuFree_create.png)
    
##グラブ
  時間差スクリーンショットを撮影する。スクリーンショットキーを押すと消えてしまう表示をキャプチャーしたい場合に有効。  
  [Macでタイマーを使用して時間差でスクリーンショットを撮る方法](http://inforati.jp/apple/mac-tips-techniques/application-hints/how-to-use-mac-grab-app-and-take-screen-shot.html)

##Font book
  Macで使用出来るフォントが確認出来る。このアプリで詳細表示設定すると正しいフォント名を確認出来る。

#Excel
使用感はWindowsとほぼ同じだが、キー入力が異なる場合がある。

<!-- excel:: -->
|コマンド|キー|
|---|---|
　タブ内の改行 | Command + Option + Return
  セル編集<br>編集したいセルを選択してからキー入力する | ctrl + u
  表示拡大縮小 | Ctrl + Option + マウスホイール
  枠線に合わせる  | 適当なオブジェクトを選択する  <br>書式設定タブの「配置」-「グリッドに合わせる」を選択

#スクリーンショット screen shot
<!-- ss:: screenshot:: -->
Macはショートカットで簡単にスクリーンショットを取れる

##スクリーンショットの保存場所を変更する
  デフォルトではスクリーンショットの保存場所デスクトップでごちゃごちゃになるので変更する

  例: 保存先をデフォルトのデスクトップからピクチャ(~/Pictures)に変更  
  ターミナルから
~~~sh
$ defaults write com.apple.screencapture location ~/Pictures/
$ killall SystemUIServer
~~~

##ショートカット
  |コマンド|説明|
  |!--|!--|
  Cmd + Shift + 3 | 画面全体
  Cmd + Shift + 4 | ドラッグで選択した部分
  Cmd + Shift + 4, space | カメラアイコンで選択した一つのウインドウ

#SSH鍵生成
##鍵(id_rsa,id_rsa.pub)作成
  [MacでSSH認証のための公開鍵と秘密鍵を生成する。](http://d.hatena.ne.jp/crosshope/20110509/ssh_keygen)

##SSH接続
###コマンド
  $ssh [ユーザー名]@[ホスト名]
  　例: $ssh syutaro@10.212.20.17
        $ssh syutaro@unchan.net

###ショートカット接続(.ssh/config)
SSH接続の設定ファイル

    $ vim ~/.ssh/config
    Host centos
      HostName  192.168.1.2
      Port      22
      User      username
      IdentityFile  ~/.ssh/id_rsa
    Host centos2
      HostName  192.168.1.3
      Port      60022
      User      hogeuser
      IdentityFile  ~/.ssh/hoge_rsa
  
  
#ターミナル　terminal
<!-- terminal: -->
###.bashrcが読み込まれる設定
  ※macではデフォルトでは.bashrcを読み込んでくれないため .bash_profileで実行する
~~~sh
.bash_profile　に以下の行を追加
  if [ -f ~/.bashrc ] ; then
    . ~/.bashrc
  fi
~~~
###.bashrc
　起動時のスクリプト  
　ユーザーのホームディレクトリに .bashrc を作成し、プロンプト起動時の処理を記述する  
  
~~~sh
#エイリアスを追加
alias ls="ls -la"
alias cd1="cd /work/oogiri_git"

#ディレクトリ変更
cd "/var/www/html"
~~~

###プロンプトをカスタマイズ 
    .bash_profile で .bashrcを作成
    ----------------
    #.bash_profile
      if [ -f ~/.bashrc ]; then
      . ~/.bashrc
      fi
    ----------------
    .bashrにプロンプト変更の処理を追加
    http://www.atmarkit.co.jp/flinux/rensai/linuxtips/002cngprmpt.html を参照
      \d  「曜 日 月 日」の形式（例：Fri Jan 5）で日付を表示する
      \h  ホスト名のうち最初の「.」までの部分を表示する
      \H  ホスト名を表示する
      \n  改行する
      \r  復帰する
      \s  シェルの名前を表示する
      \t  現在の時刻を24時間の「HH:MM:SS」形式で表示する
      \T  現在の時刻を12時間の「HH:MM:SS」形式で表示する
      \@  現在の時刻を12時間の「am/pm」形式で表示する
      \u  現在のユーザー名を表示する
      \w  現在の作業ディレクトリを、ユーザーのホームディレクトリからの絶対パスで表示する
      \W  現在の作業ディレクトリを表示する
      \!  このコマンドの履歴番号を表示する
      \#  このコマンドのコマンド番号（現在のシェルのセッション中に実行されたコマンドのシーケンスにおける位置）を表示する
      \\  バックスラッシュを表示する
      \[  非表示文字のシーケンスを開始する。これを使って、プロンプト中に端末の制御シーケンスを埋め込むことができる
      \]  非表示文字のシーケンスを終了する
    ----------------
    #.bashrc
      PS1='[\u (\t) \W]\\$'   //[shutaro (23:45:06) php]$ 
      PS1="[\u (\t) \w]\\$"   //[shutaro (23:45:49) ~/Dropbox/programing/php]$
    ----------------

  .bash_profileをリロード  

    $source ~/.bash_profile

##iTerm
複数タブ表示や見た目のカスタマイズが出来るターミナルアプリ

###ショートカット
  |機能|コマンド|
  |!--|!--|
  セッションを検索して開く |  Shift + Cmd + O
  新しいタブ |           ⌘ + T
  次のウインドウ |        ⌘ + ］
  前のウィンドウ |        ⌘ + ［
  次のタブ |             ⌘ + Shift + ］
  前のタブ |             ⌘ + Shift + ［
  新しいウィンドウ(水平) | ⌘ + D
  新しいウィンドウ(垂直) | ⌘ + Shift + D
  ウィンドウを閉じる |     ⌘ + W
  画面リフレッシュ |      Cmd + R

###フォントサイズを変更する
  メニューの [Preferences]  
  [Profiles] - [Font]  
  ![iterm_font](http://sunsunsoft.com/image/memo/iterm_font.png)


#アプリ app
<!-- app:: -->
###Safari
<!-- safari:: -->
ショートカット

|機能|キー|
|!--|!--|
  新しいタブを開く | Cmd  + T
  タブを閉じる | Cmd + W
  タブを移動(右) | Cmd + Shift + ［

###Chrome
<!-- chrome:: -->
ショートカット

|機能|キー|
|!--|!--|
   新しいタブを開く | Cmd + T
  カレントのタブを閉じる | Cmd + W
  検証モード | Cmd + Option + J
  検証モードで要素の選択 | Cmd + Shift + C
  前のページ(戻る) | Cmd + ←<br>Cmd + [
  次のページ(やり直し) | Cmd + →<br>Cmd + ]

###その他
  |アプリ名|説明|
  |!--|!--|
  item2 |               タブ表示ができていろいろとカスタマイズできるターミナル。ショートカットキーが豊富
  WinArchiver Lite |    パスワード付きzipファイル作成
  Cyber Duck |          ssh接続ができるファイル転送ソフト。WindowsでいうところのWinSCPみたいなもの。
  Spectacle |           ショートカットキーでWindowsライクなウィンドウの移動
  Git Tower |           Gitクライアント(有料)。操作感がMacライク
  DiffMerge |           ファイル比較ツール
  Compare Merge |       ファイル比較ツール(AppStoreで500円)  

  
#Apache
  最新のMacOSXには標準でApacheがインストールされている。
  起動
  
    $ sudo apachectl start
  停止 
  
    $ sudo apachectl stop
  再起動

    $ sudo apachectl restart
  設定ファイル  

    /etc/apache2/httpd.conf (標準)
    /Applications/MAMP/conf/apache/httpd.conf (MAMP)

  ドキュメントルート
    ルートのパス(ここが localhost/のルートディレクトリになる)

     /Library/WebServer/Documents/ (標準)
     /Applications/MAMP/htdocs (MAMP)

  Apachの設定  
  /private/etc/apache2/httpd.conf を開く
  PHPを開く  
  118行目あたりにある  

    #LoadModule php5_module libexec/apache2/libphp5.so
    の #を削除してファイルを保存する。(権限がないのでsudo vimで開くとよい)

  ブラウザで http://localhost にアクセスすると「It works!」が表示される
  自前のphpを実行したい場合は
  /ライブラリ/WebServer/Documents/
  いかにphpファイルを配置する（このディレクトリは管理者権限でないと変更できないのでsudoコマンドを使用する)

#便利な小技 skills
###ipアドレスを調べる
  コマンドラインから
~~~sh
ifconfig
~~~ 

###パスを通す
  環境変数$PATHにカレント(.)を追加する
  
    export PATH="/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/usr/local/git/bin:."
    
    ※.bash_profile等に記述してどのターミナルでも使用できるようにする

###hosts
<!-- hosts: -->
/private/etc/hosts を編集  
  IPアドレスとドメインの対応テーブルを設定
  hostsを編集後

    $ sudo killall -HUP mDNSResponder

  で DNSキャッシュをクリア

