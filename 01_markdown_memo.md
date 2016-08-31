<link href="https://raw.github.com/simonlc/Markdown-CSS/master/markdown.css" rel="stylesheet"></link>
<link href="http://sunsunsoft.com/css/hoge.css" rel="stylesheet"></link>

<span class="title">
  MarkDown書式
</span>


[TOC]

## リンク
###
[Markdownでスタイルシート](http://qiita.com/skkzsh/items/99e30bbbfe69f379b583)  
[Markdown記法 サンプル集](http://qiita.com/tbpgr/items/989c6badefff69377da7)  


## スタイルシート
    <link href="./hoge.css" rel="stylesheet"></link>

<a id="midashi"></a>
##  見出し
  (ハッシュ)の数と見出しのレベルが一致します。  

    # 見出し h1
    ## 見出し h2
    ### 見出し h3
    #### 見出し h4
    ##### 見出し h5
    ###### 見出し h6
  
##段落  p::
  空白行で囲まれた1行、または複数行の文章は、ひとつの段落になります。
  改行が入っていても、1行以上の空白が入っていなければ、ひとつの段落と見なされます。

      空行
    段落1 hogehoge
      空行
    段落2 hogehoge

段落1 hogehoge

段落2 hogehoge

##改行 br::
  テキストの後に改行を入れたい場合は行末にスペースを2つ入れる
~~~
  hogehoge[スペース][スペース]
  hogehoge[スペース][スペース]
~~~

  hogehoge  
  hogehoge  

  改行だけの行にしたい場合は `<br>` を入れる
    <br/>
    <br/>
    <br/>
  ⬆︎改行

##強調 em: strong: 取り消し線:
  2つの*(アスタリスク)か、_(アンダースコア)で文言を囲みます。

    *italic* _italic_
    **強調**  __強調__
    ~~取り消し~~

  *italic* _italic_  
  **強調**  __強調__  
  ~~取り消し~~  

##リスト list::
###箇条書きリスト
  -,*,+ のどれかを先頭につける  
  
    - 色は匂へど
        * 常ならむ
            * 有為の奥山
        * 今日越えて  (半角スペースを2つ)
    + 酔ひもせず


  - 色は匂へど
      * 常ならむ
          * 有為の奥山
      * 今日越えて  (半角スペースを2つ)
  + 酔ひもせず
  
###番号付きリスト 
  この他に順序リストとして示す場合は、数字とピリオドで示すことができます。
  ただし数字の順列は無視され、並びが優先されます。  

    1. 番号付きリスト1
        1. 番号付きリスト1_1
        1. 番号付きリスト1_2
    1. 番号付きリスト2
    1. 番号付きリスト3

  1. 番号付きリスト1
      1. 番号付きリスト1_1
      1. 番号付きリスト1_2
  1. 番号付きリスト2
  1. 番号付きリスト3

##罫線 rule:: 
  3つ以上の*(アスタリスク), -(ハイフン), _(アンダースコア)と半角スペースのみの行は、罫線として扱われます。  

    ***
    * * *
    *****
    - - -
    ---------------------------------------
    ___

  ***
  * * *
  *****
  - - -
  ---------------------------------------
  ___
  上記の全ての行は<hr>に変換されます。

##引用  quotation::

    > お世話になります。xxxです。
    > 
    > ご連絡いただいた、バグの件ですが、仕様です。


> お世話になります。xxxです。
> 
> ご連絡いただいた、バグの件ですが、仕様です。

■２重引用  duble quotation::

    > お世話になります。xxxです。
    > 
    > ご連絡いただいた、バグの件ですが、仕様です。
    >> お世話になります。 yyyです。
    >> 
    >> あの新機能バグってるっすね

> お世話になります。xxxです。
> 
> ご連絡いただいた、バグの件ですが、仕様です。
>> お世話になります。 yyyです。
>> 
>> あの新機能バグってるっすね

##リンク link::
  角括弧で文言、丸括弧でURLを囲むと、リンクになります。  
  `[ローカルホスト](http://localhost:8888)`  

  [ローカルホスト](http://localhost:8888)  
  
  またリンクを変数のように扱うこともできます。
  この時、リンクの定義は文中のどこに設置しても構いません。

    I get 10 times more traffic from [Google] [1] than from
    [Yahoo] [2] or [MSN] [3].

    [1]: http://google.com/        "Google"
    [2]: http://search.yahoo.com/  "Yahoo Search"
    [3]: http://search.msn.com/    "MSN Search"

  I get 10 times more traffic from [Google] [1] than from
  [Yahoo] [2] or [MSN] [3].

  [1]: http://google.com/        "Google"
  [2]: http://search.yahoo.com/  "Yahoo Search"
  [3]: http://search.msn.com/    "MSN Search"
  
##ページ内リンク link:
`[リンクテキスト](#リンク先のヘッダー行のテキスト)`

~~~html
  リンク先
    <a id="midashi"></a>
  リンク
    [見出しへのリンク](#midashi)
~~~

[見出しへのリンク](#midashi)

##pre記法
preでテキストをそのまま表示する。preの中ではマークダウンの公文はすべて無視され、そのままの形で表示される。

####スペース4つ
    空行
    行の先頭にスペース４つの行（複数)
    空行
でこのブロックがpreで表示される

    # Space
    class Hoge
      def hoge
        print 'hoge'
      end
    end

#### チルダ３つで囲む

    ~~~
    class Hoge
      def hoge
        print 'hoge'
      end
    end
    ~~~

~~~
class Hoge
  def hoge
    print 'hoge'
  end
end
~~~


#### シンタックスハイライト syntax:: highlight::
プログラミング言語の公文が強調表示される

#####ruby
    ~~~ruby
    class Hoge
      def hoge
        print 'hoge'
      end
    end
    ~~~

~~~ruby
class Hoge
  def hoge
    print 'hoge'
  end
end
~~~

#####php
    ~~~php
    <?php
    function hoge(){
      echo "hoge";
      func1("hoge");
      $hoge = 1 + 2;
    }
    ?>
    ~~~

~~~php
<?php
function hoge(){
  echo "hoge";
  func1("hoge");
  $hoge = 1 + 2;
}
?>
~~~

言語名:ファイル名 でファイル名のみが表示される

```ruby
  puts 'The best way to log and share programmers knowledge.'
```

##code記法  code:: 
`テキスト`  
バッククォートで文字列を囲むことで部分的にpreを適用させることができる。バッククォートde
囲まれたエリアはマークダウンが適用されない。
~~~
インストールコマンドは `gem install hoge` です
~~~

インストールコマンドは `gem install hoge` です

##テーブル table::
１行目がヘッダー行
2行目が幅寄せ書式  
  :-- 左寄せ  
  --: 右寄せ  
  :--: 中央  
3行目以降がデータ行  

    |header1|header2|header3|
    |:--|--:|:--:|
    |align left|align right|align center|
    |a|b|c|

|header1|header2|header3|
|:--|--:|:--:|
|align left|align right|align center|
|a|b|c|

※テーブル内の | をエスケープしたい場合は | の代わりに `&#124;` を記入

##注釈 
###注釈っぽいもの
マークダウンではaタグのページ内リンクが使えるのでそれを使って実現。

    リンク
      ここが注釈だ<sup>[1](#goto1)</sup>
    リンク先
      注釈のようなことを実現可能です
      <small><a id="goto1" href="#return1">戻る</a></small>

    
<a id="return1"></a>
ここが注釈だ<sup>[1](#goto1)</sup>

##画像 image::
  代替テキストは画像が読み込めなかった場合に、画像の表示予定だった位置に表示されるテキスト

####タイトル無しの画像を埋め込む:

    ![代替テキスト](http://www.sunsunsoft.com/image/ume_s.jpg)

  ![代替テキスト](http://www.sunsunsoft.com/image/ume_s.jpg)

####タイトル有りの画像を埋め込む:  

    ![代替テキスト](http://www.sunsunsoft.com/image/miro2_s.jpg "画像タイトル")

  ![代替テキスト](http://www.sunsunsoft.com/image/miro2_s.jpg "ミロ")  
  画像にマウスカーソルを合わせると画像タイトルが表示される

##Qiita拡張マークダウン
以下のマークダウンはQiita拡張。QiitaやKobitoでのみ使用できる。

###チェックボックス checkbox::
※Qiita用 Kobitoで確認できる

    - [ ] タスク1  
    - [x] タスク2  
    - [x] タスク3  

- [ ] タスク1  
- [x] タスク2  
- [x] タスク3  

###注釈

    注釈です [^1]  
    注釈です [^2]  
    [^1]:ほげほげほげ  
    [^2]:はげはげはげ

###preのシンタックスハイライトにファイル名を表示する

~~~ruby.hoge.rb
class Hoge
  def hoge
    print 'hoge'
  end
end
~~~

  ![](http://www.sunsunsoft.com/image/memo/markdown_pre_ruby.png)

###数式 math:
※sublimtetextで使うには OmniMarkdupPreviewer の設定に

    "mathjax_enabled" : true

を追加する。

$x^2 + y^2 = 1$  ƒ
$a = \{1, 2, 3\}$  
$a = \\{1, 2, 3\\}$  

##[注釈]
注釈だよ。注射苦じゃないよ
<sup><a id="goto1" href="#return1">戻る</a></sup>
