#snippet登録
<!-- snippet:: -->
入力の補完を行ってくれる。例えば事前にスニペットを登録しておくと  
myname と入力後 Tabキーを　押すと  
  `My name is [name].`  
と入力され、nameの部分に好きな文字列を入力できる。テンプレの長い文字列を打たなくてよくなるためコーディングが楽になるはず。

ちゃんと知りたい場合は
[開発効率アップ！Sublime Textのスニペット機能の使い方（登録方法など）](http://nelog.jp/how-to-set-sublime-text-snippet)を参照。かなり詳しく書かれてある。

こちらのページは書式とパラメータのリファレンスとして分かりやすい。
[sublimetextのスニペット](https://gist.github.com/azusa-tomita/d52b72edfb29858072d0)

##Snippetを新規登録

* メニューの [Tool] - [Developer] - [New snippet] を選択。  
    新規snippet用のファイルが開く。  
* snippetファイルを編集  

~~~python
  <content>"補完するテキスト <![CDATA[補完テキスト]]>"</content>"
  <tabTrigger>補完トリガーになるテキスト</tabTrigger>  
  <scope>対象になるファイル(text.plain,text.htmlとか)。未指定の場合は対象を制限しない</scope>  
  <description>説明文。コマンドパレットに表示される</description>

例  -hoge.sublime-snippet
　　<snippet>
    <content><![CDATA[My name is ${1:name}.]]></content>
    <tabTrigger>myname</tabTrigger>
    <scope>text.plain,text.html,text.html.markdown</scope>
    <description>Input your name</description>
  </snippet>
~~~

* ファイルを保存
    - 保存場所は **Packages** フォルダ以下ならどこでもOK
    - ファイル名の拡張子は "**.sublime-snippet**"

* スニペットを使うには
    - トリガーになる文字を入力後 Tabキーを押すか
    - メニューの [Tool] - [Snippets...] で表示される候補から選択する。

※Macではスニペットファイルを保存したらすぐに使えるようになるが、WindowsではSTを再起動する必要がある

##書式

~~~python
<snippet>
    <content><![CDATA[]]></content>
    <tabTrigger></tabTrigger>
    <scope></scope>
    <description></description>
</snippet>
~~~
###content
置きかわるテキスト。

**<![CDATA[  
"置き換えたいテキスト"  
]]>**
  
~~~
<![CDATA[
"hogehogehogehoge"
]]>  
// "hogehogehogehoge" が出力
~~~

###ユーザー入力ポイント
${入力番号:デフォルトのテキスト}**  
  
~~~
例: <![CDATA[
$This is {1:text1} ${2:text2}
]]>
// This is [text1] text2   text1が選択状態、Tabキーで text2が選択状態になる
~~~

###キャレット位置指定  
$1,$2,..$n**  
tabキーを押すたびに$1、$2で指定した編集点にキャレットが移動する。 $0で最後の編集点に移動。

~~~
例: <![CDATA[
<a href="$1">$2</a>$0
]]>

<a href="|"></a>   初期状態 |がカーソル位置
<a href="">|</a>   Tabキーを押した
<a href=""></a>|   もう一回Tabキーを押した
~~~

###tabTrigger
トリガーとなるキー。エディタでこのキーを入力後 Tabキーで contentに置きかわる。

###scope
スコープ（対象となるファイル）。複数のスコープを適用したい場合は,でつなげる  
未指定だと全てのファイルが対象になる。  
編集中のファイルのスコープを確認するには確認したいファイルを開いて show_scope_name (`Cmd + Opt + P` )。これで  
![](http://sunsunsoft.com/contents/sublimetext/image/show_scope_name.png)  
のようなポップアップが表示される。
~~~
例: 
テキストファイル(.txt)に適用する
<scope>text.plain</scope>

マークダウン(.md)に適用する
<scope>text.html.markdown</scope>

Javaに適用する
<scope>source.java</scope>

PHPとHTMLに適用する
<scope>source.php,text.html.basic</scope>
~~~

###description
コマンドパレットの "Snippet:" に表示されるテキスト。これを記述しないとファイル名が表示される。

![](http://sunsunsoft.com/contents/sublimetext/image/snippet_description.png)  
descriptionに "Java println patter 3" と記述した場合