##テキストの見出し検索
テキストファイル(.txt)でドキュメントをまとめた場合などに、テキストが大きくなるとどこにどんな内容が書いてあるのかの把握が難しくなる。そこで、テキストファイルの中の見出し行を書いてその見出しの一覧を表示できるようにする。  
![](http://sunsunsoft.com/contents/sublimetext/image/text_midashi1.png)  
テキストファイルに見出し文字(■や●)を配置すると、Cmd + R　(Winは Ctrl + R) で の関数ジャンプの候補として見出し文字の行が表示される  

###テキストファイルの言語パッケージ定義（テキストのカラー変更）
パッケージインストールから PackageDev をインストールする  
  
* メニュー[ツール] - [Packages] - [Package Development] - [New Syntax Defenition] を選択でテンプレートファイルが開く。  
![](http://sunsunsoft.com/contents/sublimetext/image/text_midashi2.png)  

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

ファイルに保存 -> test1.YAML-tmLanguage  
   
  * 保存してからメニューの[Tool] - [Build System] - [Convert to] を選択、F7キーを押す
  * 初回のみどのフォーマットにコンバートするかを選択するポップアップが出るので [JSON to Property List] を選択する。
  * Sublime/User フォルダ以下に test1.tmLanguage というファイルが出力される。このファイルが言語の色をカスタマイズするファイルで、SublimeTextはこのファイルを元に各ファイルの色を変更したり、構造を解析したりする。