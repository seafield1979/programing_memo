<!-- Stylesheet -->
<link href="http://sunsunsoft.com/css/hoge.css" rel="stylesheet"></link>

<span class="title">Bash on Windows10 メモ</span>

<!-- もくじ -->
[TOC]


#小技 skills

#エラー errors
###su でルートユーザーになれない
su でrootユーザーのパスワードを入力してもなぜか認証失敗する。
~~~sh
$ su 
パスワード:  ~rootユーザーのパスワードを入力~
su: 認証失敗
~~~

stack overflowによると `sudo su`ならうまくいくとのことなので
~~~sh
sudo su
[sudo] password for <ユーザー名>:  ~rootのパスワード入力~
~~~
のコマンドでうまくいった。

