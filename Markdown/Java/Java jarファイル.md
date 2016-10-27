#jarファイル

classファイルをjarにパッケージすることで、プログラムを１ファイルにまとめることができる。

jarファイルに圧縮

~~~sh
# packフォルダ以下を pack.jar に圧縮
# オプション c jarを作成, v 標準出力に詳細表示, f jarファイル名を指定
$ jar cvf pack.jar pack

# jarに複数のファイルやフォルダをパックする場合はスペースで区切って複数指定する
# pack1,pack2以下を pack2.jar に圧縮
$ jar cvf pack2.jar pack1 pack2

# マニフェストファイル(MANIFEST.MF)を指定する
$ jar cvfm pack.jar MANIFEST.MF pack1 pack2
~~~

jarファイルを解凍

~~~sh
$ jar xvf pack.jar
~~~

jarファイルにパッケージされたファイルを確認する

~~~sh
$ jar tf pack.jar
~~~

詳細  
~~~
jar {ctxu}[vfm0Mi] [jar-file] [manifest-file] [-C dir] files ...  
~~~

|オプション|内容|
|:--:|---|
|-c | アーカイブを新規作成する
|-t | アーカイブの内容を一覧表示する
|-x | 指定の (またはすべての) ファイルをアーカイブから抽出する
|-u | 既存アーカイブを更新する
|-v | 標準出力に詳細な出力を生成する
|-f | アーカイブファイル名を指定する
|-m | 指定のマニフェストファイルからマニフェスト情報を取り込む
|-0 | 格納のみ。ZIP 圧縮を使用しない
|-M | エントリのマニフェストファイルを作成しない
|-i | 指定の jar ファイルのインデックス情報を生成する
|-C | 指定のディレクトリに変更し、以下のファイルを取り込む