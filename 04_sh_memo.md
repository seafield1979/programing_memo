<!-- Stylesheet -->
<link href="http://sunsunsoft.com/css/hoge.css" rel="stylesheet"></link>
<span class="title">シェルスクリプト(sh)のメモ</span>

<!-- もくじ -->
[TOC]

**シェルスクリプトメモ**


**参考にしたサイト** 
<a href="http://linux.just4fun.biz/%E9%80%86%E5%BC%95%E3%81%8D%E3%82%B7%E3%82%A7%E3%83%AB%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%97%E3%83%88.html" target="_blank">逆引きシェルスクリプト</a>

**疑問**

  * 小数値を変数に代入しようとすると"non-numeric argument"のようなエラーになる。どうしたら小数変数を扱える？
  * printf文字列内のエスケープ方法

**どんなことができる**

  * 複雑な手順の自動化
  * Linuxコマンドを使ってあれこれ
  * ファイルがなかったら作る（条件分岐）
  * テキストファイルの一部を抜き出す
  * 一定時間ごとにバックアップを作成

**todo**

  * 標準エラーの出力を一時的にOFFにする方法
  * 文字列に正規表現マッチングを行い、特定の文字列を取得する
  * 指定ディレクトリ以下のファイル情報をリストアップ
  * 今日の日付でフォルダを作成し、指定ファイルをバックアップ
  * ディレクトリ以下のテキストファイルの先頭行をファイルに出力


#基礎 basic

###スクリプトを作成
~~~sh
#vim でプログラムを作成
$vim test1.sh

test1.sh
===
#!/bin/bash
echo "hoge"
===

#スクリプトに実行権限を追加 
#実行権限のないスクリプトを実行すると
#  bash: <ファイル名>: Permission denied
#のようなエラーメッセージが表示される
$chmod 777 test1.sh
~~~

**実行権限の付与**
~~~sh
chmod u+x test1.sh
chmod 777 test1.sh
~~~

環境変数$PATHにカレント(.)を追加する
  export PATH="/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/usr/local/git/bin:."
  ※.bash_profile等に記述してどのターミナルでも使用できるようにする

###スクリプトの実行
実行権限のあるスクリプト(my_script)を実行
~~~sh
# 実行権限のあるスクリプトはファイル名だけで実行できる
$ ./test1.sh

# 実行権限がない場合は sh <ファイル名> で実行できる
$ sh test1.sh
~~~

**シェルスクリプト内でcdコマンド等でディレクトリ移動させる**
シェルスクリプト内で cdコマンドでカレントを変更しても、呼び出し元のプロセスには反映されないので
シェルスクリプトの最後で

`exec /bin/bash`

を実行する。これでターミナルで直接向き合っているカレントプロセスをシェルスクリプトのプロセスに差し替える。

###コメントアウト
<!-- comment:: -->
shプログラム内でのコメントアウトはコメントアウトしたい位置の先頭に # を書く。shには /* ~ */ のような範囲コメントアウトは存在しない。
~~~sh
#コメントアウト!
##これでもコメントアウト!
echo "hoge"  # 行の途中でもOK
~~~

#HelloWorld  
<!-- hello:: -->
ただ "Hello World!!"と表示するだけのスクリプト  

~~~sh
- helloworld.sh -

#!/bin/bash
echo "Hello World!!"
~~~
これを実行するにはコマンドラインから以下を入力する。
~~~sh
$ sh helloworld.sh
~~~
もしくはファイルに実行権限(x)を追加してファイル名で実行する。
~~~sh
#すべてのタイプのユーザー(a)に実行権限(x)を追加(+)
$ chmod a+x helloworld.sh  

$ ./helloworld.sh
hello world!!

#NG カレントディレクトリにあるファイルでも先頭に ./ をつけないとエラー
$ helloworld.sh  
-bash: helloworld.sh: command not found
~~~
#変数 variable
<!-- var:: variable:: -->

スクリプトの変数の型は文字列と数値の２種類。文字列変数でも数値のような文字列、例えば"100"とかなら数値変数として使うことが可能。

特徴

 * +等で変数同士の演算はできない。演算にはexprコマンドを使用する。
 * 未定義の変数はnull。参照してもエラーにはならない。
 * 文字列の連結は変数や文字列を並べるだけ

~~~sh
  str="100"
  i=`expr 200 + $str`  #ok iに300が入る
~~~

###変数の宣言
`変数名=値`    <- 途中でスペースを入れると command not found エラーが発生する

~~~sh
abc=100
str="hello"

#文字列の中に値の変数を埋め込む $変数名 ${変数名}
str2="hello $abc ${str}"
i=`expr 100 + 200`  #計算結果を代入
i=`expr $a + $b`
i=`expr $i + 1`  #インクリメント
~~~

###変数の参照  
`$変数名 or ${変数名}`
~~~sh
echo $abc
echo ${abc}
~~~

###特殊な変数
システムが用意した特殊な変数がある。

|変数名|説明|
|---|---|
$$ | シェル自身のPID（プロセスID）
$! | シェルが最後に実行したバックグラウンドプロセスのPID
$? | 最後に実行したコマンドの終了コード（戻り値）
$- | setコマンドを使って設定したフラグの一覧
$* | 全引数リスト。"$*"のように「"」で囲んだ場合、"$1 $2 … $n" と全引数を一つにくっついた形で展開される。
$@ | 全引数リスト。"$@"のように「"」で囲んだ場合、"$1" "$2" … "$n" とそれぞれの引数を個別にダブルクォートで囲んで展開される。
$# | シェルに与えられた引数の個数
$0 | シェル自身のファイル名
$1～$n | シェルに与えられた引数の値。$1は第１引数、$2は第２引数…となる。

#echo printf
<!-- echo:: printf:: -->
echoやprintf でコンソール(標準出力)や指定のファイルに文字列を表示できる

echo は **改行あり**  
printf は **改行なし**  

~~~sh
#echo
echo "[文字列]"
echo "${変数} "

# \nで改行
echo "hello world\n\n"  

# hoge.txt に書き込み
echo "hoge" > hoge.txt  

# hoge.txt に追加書き込み
echo "hoge" >> hoge.txt  

#printf でフォーマットを指定して出力
a=100
b=200
printf "${a} ${b} \n"
printf "hello world \n" >> hello.txt
~~~

#配列 array
<!-- array:: -->

~~~sh
#配列の初期化
  array=(1 2 3 4 "5" "6" "7")

#指定要素に代入
  array[0]=100
  array[2]=100

#配列の参照
  ${array[要素番号]}
  echo ${array[0]}

#全配列の参照
  ${array[@]}
  # forループで値を参照
  for arg in ${array[@]}
  do
    echo arg
  done

#配列の要素数
  array=(1 2 3)
  echo ${#array[@]}  #3
  echo ${#array[*]}  #3
~~~

#ループ loop
繰り返し処理を行う。配列の全要素に対して処理を行いたい時などに使用する。

##for文
<!-- for:: -->

~~~sh
for 変数 in リスト
do
  処理
done

#指定回数ループ
echo "loop1"
for i in `seq 0 10`
do
  echo i=$i
done

#指定回数ループ2
echo "loop2"
for i in {0..10}
do
  echo i=$i
done

#リストをループ
for i in 1 2 3 4 5
do
  echo i=$i
done

#配列をループ
array=(1 2 3 4 5)
for i in ${array[@]}
do
  echo $i
done
~~~

##while文
<!-- while:: -->
条件を満たす間ずっとループし続ける。無限ループになる可能性があるので注意。

~~~
#基本
while 条件
do
  コマンド
done

#配列ループ
array=(1 2 3 4 5 -1 1)
i=0

while [ $i -ne -1 ]
do
  echo $i
  i=`expr $i + 1`   #注意 $i + 1 のように要素の間にスペースを入れないとエラーになる
done

#演算 expression
<!-- expression:: expr:: -->

##expr
exprコマンドで変数通しを足したり引いたりの演算ができる。
※各値、演算子はスペースで区切ること
~~~sh
  `expr 演算`
  `expr 1 + 2`

  echo `expr 1 + 2`
  i=100
  echo `expr ${i} + 1`
  i=`expr ${i} + 1`     //exprの結果を変数に代入
~~~

##比較演算子
<!-- compare:: -->
変数や定数値が同じ値か大小かの判定を行い。if文やwhile文等、条件式を受け取る場所で使用する。他の言語で使われる '>' や '<=' は使用できないので注意。
~~~sh
a=100
# Cで言うところの if (a >= 100) 
if [ $a -ge 100 ]; then
  echo "$a is grater than 100\n"
fi

~~~

**数値の比較**

|演算子|意味|
|---|---|
  [ a -eq b ] | a == b  ※[ の後にスペースを空けないと構文エラーになる ]
  [ a -ne b ] |  a != b
  [a -lt b ] |  a < b
  [a -gt b ] |  a > b
  [a -le b ] |  a <= b
  [a -ge b ] |  a >= b

**文字列の比較**

|演算子|意味|
|---|---|
  [ a = b ] |   aとbの文字列が等しいか
  [ a != b ] |  aとbの文字列が等しくないか
  [ a ] |       aがNULLでないときに真
  [ -n a ] |    aがNULLでないときに真
  [ -z a ] |    aがNULLのときに真


**論理演算子 **

|演算子|意味|
|---|---|
　条件1 -a 条件2 |  条件1と条件2の両方が真のときに真
  条件1 -o 条件2 |  条件1と条件2のどちらかが真のときに真
  !条件 |           条件が偽のときに真

~~~
# Cで言うところの if (a == 1 && b == 2) 
if [ ${a} -eq 1 -a ${b} -eq 2 ] ; then
  echo "ok"
else
  echo "ng"
fi
~~~

#正規表現 regular expression
<!-- regular:: reg:: -->
文字列に正規表現マッチングしているかどうかを判定する。
例えば 文字列が hoge から始まる hoge123 や hogehoge 等にマッチする文字列だけ処理したい場合などに使用する。
※shスクリプトでは正規表現中の()で囲った部分にマッチする文字列を取得することはできなかった。

###正規表現にマッチするかどうかの判定
`if [[ 対象文字列 =~ 正規表現 ]]; then`

~~~sh
str1="abc"
if [[ str1 =~ ^ab[cd]$ ]]; then
  echo "matched"
else
  echo "unmatched"
fi
~~~

**メタ文字**

|メタ文字|マッチする文字|
|---|---|
. | 任意の一文字
[...] | 列挙された任意の文字<br>例: [abc] abcのどれかにマッチ<br>[a-z] aからzまでのどれかにマッチ
[^...] | 否定文字クラス 列挙されていない任意の文字
^ | 行の先頭位置<br>^hoge なら hogeから始まる文字列にマッチ
$ | 行の末尾位置<br>hoge$ なら hogeで終わる文字列にマッチ
&#124; |  隔てられた表現のうちのいずれかのものにマッチする
? | 任意の文字が0個か1個
* | 任意の文字が0個以上
+ | 任意の文字が1個以上
{min,max} | 任意の文字がmin個以上、max個以下

#if文
<!-- if:: -->
指定の条件を見たいしているなら以下のブロックを処理する

###if文
~~~sh
#if
  if [ 条件式 ]; then
    処理
  fi

  if [ $i -eq 10 ] ; then
    ecoh "i == 10"
  fi 
  #[$i -eq 10] のように各要素の間のスペースを省略するとエラーになる

#if else
  if 条件式 ; then
    処理
  else
    処理
  fi

#if elif else
  if 条件式 ; then
    処理
  elif 条件式 ; then
    処理
  else
    処理
  fi
~~~

###testコマンド
条件式を評価して結果を0か1で返す

~~~sh
#test 条件式 ; 処理
i=1
test $i -eq 1 ; echo $?   # $?に結果が入る 真なら0 偽なら1
~~~


#コマンドの実行 command
<!-- command:: -->
任意のコマンドを実行して結果を取得できる。これを使用すると文字列変数に入ったコマンドをそのまま実行することができる。

~~~sh
#`コマンド`
echo `ls -la`

#コマンドの引数に変数を使用する
`cat $filename`

#コマンドも引数も変数の値を使用する
command="ls"
param="-la"
`$command $param` 

#コマンドの実行出力を取得する
ret=`ls`  #retにはファイル名の配列が入る
for file in $ret[@]
do
  echo $file
done
~~~

#case文
<!-- case:: switch:: -->
caseの後に列挙したどれかにマッチしたら対応する処理を実行する。

~~~sh
case 値 in
  パターン1 ) 処理1 ;;
  パターン2 ) 処理2 ;;
  パターン3 ) 処理3 ;;
  * ) その他のパターンの処理 ;;
esac

#整数値で分岐
i=1
case $i in
    1 ) echo "1" ;;  # $iの値が1のときに実行される
    2 ) echo "2" ;;
    3 ) echo "3" ;;
  esac

#文字列 ワイルドカードも使用可能
s="bca"  
case $s in
  "a" ) echo "a" ;;
  "b" ) echo "b" ;;
  "c" ) echo "c" ;;
  a* ) echo "a*" ;;   # aではじまる文字列
  *a ) echo "*a" ;;   # aで終わる文字列
esac
~~~

#ファイルチェック file
<!-- file:: -->
if文やwhile文の条件で、ファイルの形式・パーミッション・特性で評価したい場合、以下のように記述する

|ファイルチェック演算子 | 意味 |
|---|---|
  -e ファイル名 | ファイル、ディレクトリが存在するなら真
  ! -e ファイル名 | ファイル、ディレクトリが存在しないなら真<br>!は他の演算子にも使用可能
  -d ファイル名 | ディレクトリなら真
  -f ファイル名 | 通常ファイルなら真
  -L ファイル名 | シンボリックリンクなら真
  -r ファイル名 | 読み取り可能ファイルなら真
  -w ファイル名 | 書き込み可能ファイルなら真
  -x ファイル名 | 実行可能ファイルなら真
  -s ファイル名 | サイズが0より大きければ真
  ファイル1 -nt ファイル2 |  ファイル1がファイル2より新しければ真
  ファイル1 -ot ファイル2 |  ファイル1がファイル2より古ければ真

  特定の正規表現にマッチするファイルがあるかどうかを調べるには
    `ls hoge*`
  のようにlsコマンドを使用する 

~~~sh
# -e ファイル名    ファイル、ディレクトリが存在するなら真
if [ -e test1.txt ]; then
    echo "test1.txt is exist!"
fi

# -d ファイル名    ディレクトリなら真
if [ -d dir1 ]; then
    echo "dir1 is a directory!"
fi

# -f ファイル名    通常ファイルなら真
if [ -f test1.txt ]; then
    echo "test1.txt is file!"
fi

# -r ファイル名    読み取り可能ファイルなら真
if [ -r test1.txt ]; then
    echo "test1.txt is readable!"
fi

# -w ファイル名    書き込み可能ファイルなら真
if [ -w test1.txt ]; then
    echo "test1.txt is writable!"
else
    echo "test1.txt is not writable!"
fi

# -x ファイル名    実行可能ファイルなら真
if [ -x test1.txt ]; then
    echo "test1.txt is executable!"
else
    echo "test1.txt is not executable!"
fi
~~~

#関数 function
<!-- func:: function:: -->
関数を定義するとその関数を呼び出せるようになる。
特徴

 * 関数の引数の名前は定義できない（自動で$1,$2...に入る)
 * 戻り値が返せない(return で返すのは標準出力の結果のみ)
 * 関数の書き方が何パターンか存在する
 * 関数定義の後に関数呼び出しを行う必要がある

[シェルスクリプト 関数](http://shellscript.sunone.me/function.html#関数の戻り値としての標準出力:04d247d09468c99504aa9287991416de)

~~~sh
関数の定義
function 関数名()
{
   処理
   return 戻り値
}

#関数1
function test10() {
    echo "test10"
}
#関数2
function test11 {
    echo "test11"
}
#関数3
test12() {
    echo "test12"
}

#引数を２つ受け取る
function test2() {
    echo $@  #引数の配列
    echo $#  #引数の数
    echo $1  #引数1
    echo $2  #引数2
}

#戻り値を返す(ただしシェルの仕組み的に普通の戻り値とは違う)
#戻り値を返したい場合は標準出力に文字列を書き出す
function test3 {
    echo "$1 and $2 is good!"
}

test10
test11
test12
test2 "hoge" "mage"

val1=100
str=`test3 "hoge" $val1`
echo "戻り値=${str}"
~~~


#便利・小技 skills
<!-- skill:: teqniquie:: -->

###引数をすべて表示
引数の $@ に配列形式で入る。
~~~sh
for arg in $@
do
    echo ${arg}
done  
~~~


##外部ファイル
外部ファイルを取り込むとそのファイルで定義された関数や変数を使用できる
外部ファイルの取り込みは
~~~sh
source ./"[外部ファイル名]"
例:  source ./"gfunc1"
~~~

サンプル
~~~sh
ファイル名 global_func

#!bin/sh
LOG(){
  # ログ出力先
　　LOG_DIR=./

　　# 引数展開
　　FILENM=`basename $0`
　　MSG=$1

　　# 変数定義
　　LOG_DATE=`date '+%Y-%m-%d'`
　　LOG_TIME=`date '+%H:%M:%S'`
　　LOGFILE="${LOG_DIR}${LOG_DATE}_`basename $0 .sh`.log"

　　# ログ出力実行
　　printf "%-10s %-8s %-14s %-50s\n" \
　　"${LOG_DATE}" "${LOG_TIME}" "${FILENM}" "${MSG}" >>${LOGFILE}
}

呼び出し元
#call_gfunc
source ./"global_func"
LOG "hello world"
~~~

###ファイルの有無を調べる
~~~sh
if [ -e aaa.txt ]; then
  ファイルある
fi
~~~

###ファイルの有無を調べるときにファイル名に正規表現を使用する方法
たとえば hoge* のような正規表現に一致するファイルが存在するかが知りたい場合は、

~~~
array1=`ls hoge*`
for arg in ${array1[@]}
do
  echo $arg
done
~~~

のようにlsコマンドを実行してその出力を変数(配列)に代入する

##スクリプトのオプションを判定
コマンドラインからシェルスクリプトにオプションを渡して、スクリプト側でこれを簡単に取得するには getopts コマンドを使用する。

test_getopts.sh
~~~sh
#!/bin/sh
while getopts l:t: opt
do
  case ${opt} in
  l)
    LIST=${OPTARG};;
  t)
    TYPE=${OPTARG};;
  \?)
  exit 1;;
    esac
done
echo "LIST=${LIST}"
echo "TYPE=${TYPE}"

使用方法
$ getopts.sh -l list -t type
LIST=list
TYPE=type
~~~

##ヒアドキュメント
コマンドに対して複数行の文字列を与える
~~~
[コマンド] <<[任意の文字列]
> 文字列
> 文字列
[最初に指定した任意の文字列]

cat <<_EOT_
> aaa
> bbb
_EOT_
~~~

##ユーザーからの入力を受け付ける
~~~sh
read -p "Please input your name: " name  #nameに入力が入る
echo $name
~~~

##スクリプトの実行ディレクトリを取得する
~~~sh
#Linuxでは
  script_dir_path=$(dirname $(readlink -f $0))
#Macでは
  script_dir_path=$(dirname $(greadlink -f $0))
~~~