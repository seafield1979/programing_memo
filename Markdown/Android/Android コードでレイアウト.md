#コードでレイアウト layout by code


###RelativeLayout
RelativeLayoutで配置したView(button3)の下にボタンを追加

```java
Button newButton = new Button(this);
newButton.setText("新しいボタン");

RelativeLayout.LayoutParams layout = new RelativeLayout.LayoutParams(
                RelativeLayout.LayoutParams.WRAP_CONTENT,
                RelativeLayout.LayoutParams.WRAP_CONTENT);
layout.addRule(RelativeLayout.BELOW, R.id.button3);
layout.addRule(RelativeLayout.CENTER_HORIZONTAL);
button5.setLayoutParams(layout);
        
// ViewGroupに追加
RelativeLayout relativeLayout = (RelativeLayout)findViewById(R.id.relativeLayout);
relativeLayout.addView(button5);
```

addRule

```java
LayoutParamsオブジェクト.addRule(int vert, int subject);

例
// button3の下に配置
layout.addRule(RelativeLayout.BELOW, R.id.button3);
// 横方向、中央に表示
layout.addRule(RelativeLayout.CENTER_HORIZONTAL);
```

