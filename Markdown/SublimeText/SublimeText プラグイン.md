#プラグイン plugin
<!-- plugin -->
プラグインを使うとSublimeTextの機能を組み合わせて様々な処理を行うことができる。プラグインのプログラミング言語はPythonで、SublimeText用のAPIが大量に用意されてあるので頑張ればかなり高度な機能を実現することができる。多くのパッケージはこのプラグインで実装されている（と思う）。

[SublimeTextのプラグイン作成方法](http://inet-lab.naist.jp/blog/how-to-make-a-sublime-text-2-plugin/)

###プラグイン作成
`Tools > Developer > New Plugins ...  `
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