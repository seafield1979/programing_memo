#サイドバーのフォントサイズ変更

Sublime Text -> Preferences -> Browse Packages
Open "User"
"Default.sublime-theme" を新規作成。(themeを変更していた場合は"[テーマ名].sublime-theme"を作成)

以下のように編集する
~~~python
[
    {
        "class": "sidebar_label",
        "color": [150, 150, 150],
        "font.bold": false,
        "font.size": 12  // フォントサイズ
    },
]
~~~