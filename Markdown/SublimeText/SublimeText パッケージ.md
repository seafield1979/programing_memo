#パッケージ package

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


###パッケージのインストール  
`Cmd + Shift + P` ->  `Package Control: Install Package`  
から以下のパッケージをインストールする。  

|有効度|パッケージ名|説明|
|:--:|---|---|
|★★ | Cobalt2      | colorスキーマ。Cobaltをカスタマイズしたもの。渋い色合い。<br>インストールするとメニューの[Preferences - Color Scheme]に項目が追加される
|★ | Flatland     | colorスキーマ。ダーク調の渋い色合い
|★ | Smart Delete | 改行を削除した際に、次の行のインデントを削除してくれる。
|★★★ | Emmet        | 短縮コマンドを展開してhtmlタグを生成するツール<br>[SublimeText Emmet公式集](http://docs.emmet.io/cheat-sheet/)<br>タグの入力を省力化してくれる。
|★★★ | BracketHighlighter | HTMLの開始タグ～終了タグや、コードのカッコのペアをわかりやすく表示してくれる。これど導入してこのdivの終了タグってどのdivだ？みたいなのに時間を取られないようにしたい。
|★★ | IMESupport | Windows環境で日本語入力変換候補がウィンドウの左上に表示されるのを防いでくれる。
|★ | Diffy | 2つのファイルの差分を表示してくれるプラグイン。<br>Shift + Alt + 2(“) を押してウインドウを分割表示し、Ctrl + K + D を押すと量ファイルの差分がハイライトされる。
|★★★ | OmniMarkupPreviewer | マークダウン用の機能を使えるようになる。機能が使えるのは .md ファイルのみ<br>プレビュー  Cmd + Opt + O<br>html出力   Cmd + Opt + X<br>目次を展開するには<br>[Preferences] - [Package Settings] - [OmniMarkupPreviewer] - [settings - Default]<br>"extensions": ["tables", "strikeout", "fenced_code", "codehilite","toc"]    <-- "toc"を追加<br>.mdファイル内の目次を挿入したい位置に[TOC]を挿入する。上下に空行がないと正しく出力されないので注意。
|★ | Markdown Extended | Markdownに挿入したコードのシンタックスハイライト<br>mdファイルのhtmlコードをハイライトする