#ショートカット shortcut
<!-- shortcut:: -->
SublimeTextは機能が豊富だが、それを呼び出すためにその都度メニューから項目をたどると効率が悪い。ショートカットを覚えて効率を上げたい。

[Keyboard Shortcuts - Windows/Linux](http://docs.sublimetext.info/en/latest/reference/keyboard_shortcuts_win.html)  
[Keyboard Shortcuts - OSX](http://docs.sublimetext.info/en/latest/reference/keyboard_shortcuts_osx.html)  
[ゆとりっち【保存版】Sublime Text 3のショートカットキーの一覧（Windows/Mac）](http://www.starlod.net/sublime-text-3-shortcutkey.html)

##ショートカットの追加方法
ショートカットを登録:
メニュー - `[Sublime Text] - [Preferences] - [Key Bindings - User]` で設定ファイルを開き、
~~~
{ "keys": ["super+shift+h"], "command": "example" },
~~~
みたいな行を追加する。superはMacならCommandキー、Windowsなら Ctrlキーを意味する。
  
###一般:

|コマンド|Mac|Windows|
|---|---|---|
|コンソール(console)を開く<!-- console:: --> | Ctrl + `  // `は Shift + @ | Ctrl + `
|コマンドパレット<!-- command palette:: --> | Cmd + Shift + P  | Ctrl + Shift + P |  
|フォント拡大 | Cmd + ; | Ctrl + ; 
|フォント縮小 | Cmd + - | Ctrl + -
|フォント拡大縮小 | --- | Ctrl + Wheel
|折りたたみ | Cmd + Opt + [ | Ctrl + Shift + [
|折りたたみ展開 | Cmd + Opt + ] | Ctrl + Shift + ]
|レベルnで折りたたみ<br>レベル1でトップ階層にあるメソッドやクラスを折りたたむ<br>※カーソルのあるブロックは折りたたまれない | Cmd + K,1-9 | Ctrl + K,1-9
|すべて展開 | Cmd + K,J<br>Cmd + K,0 | Ctrl + K,J<br>Ctrl + K,0

###ブックマーク
ブックマークはファイルを閉じるとすべて解除される 

|コマンド|Mac|Windows|
|---|---|---|
|ブックマーク追加 | Cmd + F2 | Ctrl + F2
|ブックマーク解除 | Cmd + Shift + F2 | Ctrl + Shift + F2
|次のブックマークに移動 | F2 | F2
|前のブックマークに移動 | Shift + F2 | Shift + F2

###ファイル

|コマンド|Mac|Windows|
|---|---|---|
|新しいタブを開く | Cmd + N | Ctrl + N
|ファイルを閉じる | Cmd + W | Ctrl + W
|ファイルを開く(絞り込み入力ができる)<br>![](http://sunsunsoft.com/contents/sublimetext/image/file_open.png) | Cmd + P | Ctrl + P
|次(右)のタブに移動 | Cmd + Shift + ]<br>Cmd + Opt + → | Ctrl + PageDown
|前(左)のタブに移動 | Cmd + Shift + [<br>Cmd + Opt + ← | Ctrl + PageUp
  
<br>
 
###ウィンドウを分割
分割されたウィンドウはグループと呼ばれる。

|コマンド|画面配置|Mac|Windows|
|---|:--:|---|---|
|1画面 | □  | Cmd + Opt + 1          | Shift + Alt + 1   
|2 行 | □ □     | Cmd + Opt + 2          | Shift + Alt + 2   
|3 行 | □ □ □     | Cmd + Opt + 3          | Shift + Alt + 3   
|2 列 | □<br>□     | Shift + Cmd + Opt + 2  | Shift + Alt + 8    
|3 列 | □<br>□<br>□     | Shift + Cmd + Opt + 3  | Shift + Alt + 9  
|4 グリッド | □ □<br>□ □ | Cmd + Opt + 5          | Shift + Alt + 5  
|編集するグループを移動 || Ctrl + 1~4 | Ctrl + 1~4
|編集中のファイルをn番目のグループに移動 || Ctrl + Shift + 1~4 | Ctrl + Shift + 1~4

<br>
###カーソル移動

|コマンド|Mac|Windows|
|---|---|---|
|カーソルを１文字進む | Ctrl + F | ---
|カーソルを１文字戻る | Ctrl + B | ---
|カーソルを１行進む | Ctrl + N | ---
|カーソルを１行戻る | Ctrl + P | ---
|カーソルを１ページ分上にスクロール | Fn + ↑ | Pageup
|カーソルを１ぺージ分下にスクロール | Fn + ↓ | Pagedown
|★対応するカッコに移動 | Ctrl + M | Ctrl + M

<br>
###編集系

|コマンド|Mac|Windows|
|---|---|---|
|コピー(選択状態でなければ現在行をコピー) | Cmd + C | Ctrl + C
|履歴から貼り付け | Cmd + K,V | Ctrl + K,V 
|インデント<br>複数行をまとめて行うことも可能 | Cmd + ] | Ctrl + ]
|アンインデント | Cmd + [ | Ctrl + [
|複数行を選択状態でインデント | Cmd + Tab | Ctrl + Tab
|複数行を選択状態でアンインデント | Cmd + Shift + Tab | Ctrl + Shift + Tab
|行の移動 | Ctrl + Cmd + ↑ or ↓ | Ctrl + Shift + ↑ or ↓
|現在行を複製(下の行に挿入される) | Cmd + Shift + D | Ctrl + Shift + D
|行末まで削除 | Cmd + K,K | Ctrl + K,K
|行の削除 | Ctrl + K | ---
|行の削除(改行も) | Ctrl + Shift + K | Ctrl + Shift + K
|行を結合<br>(カーソルのある行とその次の行を結合) | Cmd + J | Ctrl + J 
|カーソル行の前に改行を挿入 | Cmd + Shift + Return | Ctrl + Shift + Enter
|コメントアウト<br>※すでにコメントアウトされていた場合はコメントアウトを削除<br>※挿入されるコメントはファイルフォーマットに依存<br>(txtファイル等の非プログラミング言語のファイルは挿入されない) | Cmd + / | Ctrl + /
|範囲コメントアウト<br>※コメントブロック内ではコメントアウトを削除<br>※範囲コメントアウトのない言語では通常のコメントアウト | Cmd + Shift + / | Ctrl + Shift + /

<br>
###検索・置換系

|コマンド|Mac|Windows|
|---|---|---|
|検索パレットを開く | Cmd + F | Ctrl + F
|置換 | Cmd + Opt + F | Ctrl + H
|次を置換 | Cmd + G | Ctrl + Shift + H
|Grep(複数ファイルから検索) | Cmd + Shift + F | Ctrl + Shift + F
|関数の定義に移動(関数ジャンプ) | Cmd + R | Ctrl + R
|ファイル名で検索(絞り込み機能付き)<br>"フォルダ名 ファイル名"<br>と指定すると特定のフォルダ以下のファイルに絞る込むことができる。 | Cmd + P | Ctrl + P
|正規表現トグル<br>※検索パネルにカーソルがある状態で有効 | Cmd + Opt + R | Alt + R
|検索文字列を全選択 | Opt + Return | Alt + Enter
|カーソル位置の文字列を全検索して選択状態にする | Cmd + Ctrl + G | Alt + F3
    
<br>
###変換

|コマンド|Mac|Windows|
|---|---|---|
|選択範囲をすべて大文字に変換する | Cmd + K,U | Ctrl + K,U
|選択範囲をすべて小文字に変換する | Cmd + K,L | Ctrl + K,L

<br>
###選択

|コマンド|Mac|Windows|
|---|---|---|
|矩形選択 | Opt を押しながら、左クリックしドラッグ | Shiftを押しながら、右クリックしドラッグ
|複数箇所選択<br>カーソル位置が複数選択に追加される | Cmd + 左クリック | Ctrl + 右クリック

<br>
###html

|コマンド|Mac|コマンド(Win)|
|---|---|---|
|開始タグと終了タグを行き来する | Ctrl + Shift + T | ---
|親要素をたどる | Cmd + Shift + A | Ctrl + Shift + A
|選択範囲を広げる | Ctrl + d | ---
|選択範囲を縮める | Ctrl + j | ---