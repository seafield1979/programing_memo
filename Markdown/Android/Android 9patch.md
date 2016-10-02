#9patch

リンク
[9Patchについて](http://coffee-ox64.sakura.ne.jp/LT/9patch.pdf)

9パッチの概要から作り方まで網羅されている。図が多用されてイメージしやすい。
[[Android アプリの UI デザイン] 9-patch の作りかたのまとめと Tips](http://dev.classmethod.jp/smartphone/android/android-ui-design-9-patch/)

###9patch用の画像を作成する方法
Android Studio で 9patchを当てたい画像をres/drawable 以下に配置。
画像を右クリックで表示されるポップアップから
`Create 9patch file...`
を選択。

9patchのファイルの仕組みは

* 元の画像はpng限定
* ファイル名は .9.png になる
* 元画像の上下左右に１ピクセル領域を増やして9patch用の情報を埋め込む



![](http://sunsunsoft.com/image/android/nine_patch1.png)
![](http://sunsunsoft.com/image/android/nine_patch2.png)![](http://sunsunsoft.com/image/android/nine_patch3.png)