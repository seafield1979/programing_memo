#ユーザー設定 User Settings

メニューの  
`Sublime Text > Preferences > Settings User`  
でユーザーの見た目や動作を好みに変更できる。
設定ファイルを直接編集したい場合は以下のファイル  
`~/Library/Application Support/Sublime Text 3/Packages/User/Preferences.sublime-settings`

|有効度|プロパティ|説明|
| :--: |---|---|
  |★★★ | color_scheme          | カラースキーマ<br>`"Packages/Theme - Cobalt2/cobalt2.tmTheme"`<br>メニューの [Preferences] - [Color Scheme] からも設定可能。その場合はこのプロパティが自動で設定される。
  |★★ | theme                 | SublimeTextの外観のテーマ<br>サイドバーやタブバーのレイアウトが変わる。<br>例:Cobalt2.sublime-theme<br> ![](http://sunsunsoft.com/contents/sublimetext/image/cobalt2.png)
  |★★★ | font_face             | フォントタイプ <br>例:`SourceHanCodeJP-Normal`<br>**Macで**Sublime Textで使用出来るフォント名の調べ方は、Font Bookアプリを起動し、詳細表示に切り替えてからフォントを選択すると "Post Script名" に表示される。<br> ![](http://sunsunsoft.com/contents/sublimetext/image/mac_font_name.jpg)
  |★★★ | font_size             | フォントサイズ 例:15<br>ショートカットやメニューからフォントサイズが変更されたら連動する
  |★★★ | tab_size              | タブサイズ 例:4
  |★ | draw_indent_guides    | カーソルがある行のインデント位置に縦線を表示してくれる<br>true/false
  |★★ | draw_white_space      | 半角スペースを表示するか<br>"none":表示しない<br>"selection":選択部分のみ表示<br>"all":常に表示 
  |★ | rulers                | ルーラーを表示する列数のリスト<br>例: [40,80] のように複数設定できる。１つだけ設定する場合でも[]で囲む。
  |★ | gutter                | ガーター(ウィンドウの左側の行数や折りたたみボタンを表示する領域)を表示する true/false<br>![](http://sunsunsoft.com/contents/sublimetext/image/gutter.png)<br>↑ガーターの領域
  |★★ | line_numbers          | 行番号を表示するかどうか true/false<br>![](http://sunsunsoft.com/contents/sublimetext/image/line_number.png)
  |★ | fold_buttons          | ガーターに折りたたみボタンを表示するかどうか true/false
  |★★ | highlight_modified_tabs | 変更したファイルのタブがハイライトされる<br>(Cobalt2のテーマでは黄色い●がついた)<br>![](http://sunsunsoft.com/contents/sublimetext/image/highlight_modified_tabs.png)
  |★ | highlight_line        | カーソル行をハイライトするか true/false<br>trueに設定すると行の背景がうっすらと色が変わる
  |★ | show_full_path        | ウィンドウのバーに表示するファイル名をフルパスで表示するかどうか true/false
  |★ | translate_tabs_to_spaces | タブを入力した際に自動でスペースに変換するか true/false
  |★ | open_files_in_new_window | Finder等からファイルを開いたときに新しいウィンドウで開くかどうか(false推奨)
  |★ | detect_indentation    | ファイルの内容からタブサイズを自動で取得するかどうか true/false<br>タブサイズがスペース８つのファイルを開いた場合は、タブを押したときにスペースが８つ挿入される。
  |★ | trim_automatic_white_space | 改行時に行に存在する無駄なスペースを削除するかどうか true / false
  |★★ | word_wrap             | ワードラップするか。ワードラップはウィンドウに収まらない行を改行して表示する機能<br>true/false/auto<br>(autoの場合はファイルの拡張子によって自動でON/OFF判定する)
  |★★ | wrap_width            |  ワードラップ有効時に何文字で折り返すかどうか<br>0ならウィンドウ幅で折り返す
  |★ | draw_centered         | テキストを中央に表示するかどうか true/false
  |★ | auto_match_enabled    | カッコやコーテーション等を入力時にそれを閉じるための文字を自動で入力するかどうか true/false<br>(),{},'',"" 等で有効
  |★ | word_separators       | ダブルクリックや Ctrl + D で文字列を選択する際に、選択範囲を区切るのに使用する文字列<br>例:"./\\()\"'-:,.;<>~!@#$%^&*|+=[]{}`~?",
  |★★ | file_exclude_patterns | サイドバーに表示するファイルリストから除外するファイルを正規表現("*.xlsx"とか)で設定する。複数設定できるので配列形式
  |★★ | folder_exclude_patterns | サイドバーに表示するファイルリストから除外するフォルダを設定する。
  