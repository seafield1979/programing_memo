#自前のViewでタッチ処理を行う
自前のViewで描画を行った際に、Viewの中のアイコン等をユーザーのタッチ操作で操作できるようにする。Viewが受け取れるイベントはタッチイベントのみ。この情報を元に自前でクリックやドラッグを検出する


自前で作成したViewで onTouch イベント情報を元に以下のアクションを判定する。

* クリック（タッチ）
* ロングクリック
* ロングタッチ
* ドラッグ

それぞれのアクションがユーザーの意図通りに発生させられるようにする。

タッチダウン
  ↓
タッチアップ


###クリック
あるポイントをタッチして放す。
タッチ位置からタッチを離した位置が離れていた場合はクリックにならない。

タッチダウン
  ↓  一定時間経過
タッチアップ



###ロングクリック
あるポイントをタッチして放すまでの時間が一定時間以上。
タッチ時間が短かったら、またタッチ位置とタッチを離した位置が離れていたらロングクリックにならない。

タッチダウン
 ↓
一定時間経過

onTouchのアクションだけでは判定できないため、タイマーを使用する。


###ロングタッチ
あるポイントを一定時間以上タッチし続ける。
あるポイントをタッチして、そこから（あまり）移動せずにそのまま。

###ドラッグ
タッチしたままタッチ位置を移動する。