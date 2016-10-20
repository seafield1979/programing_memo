#ViewGroup

ActivityにsetContentViewで配置できるViewはひとつだけ。ボタンやテキスト等のWidgetはViewGropuと呼ばれるWidgetを配置する用のViewに配置してから、これをActivityにsetContentViewする。

Activity
↑  setContentView
ViewGroup (RelativeLayoutとかLinerLayoutとか)
↑
Widgets (ButtonとかTextViewとか)

使用頻度の高いViewGroup

* FrameLayout（Viewを重ねあわせて配置）
* LinearLayout（Viewを縦もしくは横の一方向に順番に配置）
* RelativeLayout（View同士の位置関係から相対的に配置）
* ScrollView (画面に収まりきらない場合にスクロールする、子要素は１つだけ)
* TableLayout (縦横のテーブル、各列、行のサイズは可変)


##LinearLayout

##RelativeLayout

##TableLayout

特徴

* 行の幅、列の高さはそれぞれ行、列の中の要素(Widget)の最大のサイズが適用される
* 行をまたぐには android:layout_span="2"
* 行（横方向）はTableRow