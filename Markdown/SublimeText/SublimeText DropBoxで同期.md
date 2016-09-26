#DropBoxで同期

複数のマシンでSublime Textを使用する場合、それぞれの環境で一から設定を行うのは面倒なので DropBoxを使用して同期させる方法を説明する。

仕組み的には Sublime Text のユーザー設定フォルダの向き先をそっくりDropBox以下のリンクを向くようにする。
Sublime Textの設定ファイルは Packages/User というフォルダに入っている。これをDropBoxの適当な場所に移動して、Sublime Textから参照するリンクに置き換える。

手順は以下

 1. /Sublime Text 3/Packages/User -> Dropbox/Sublime/User に移動
 2. /Sublime Text 3/Packages/User を削除
 3. Dropbox/Sublime/User のリンクを作成し、/Sublime Text 3/Packages/ 配下にコピー

[Package Control Syncing](https://packagecontrol.io/docs/syncing)  

###Windows
コマンドプロンプトから以下のコマンドを入力

~~~bat
# 初回設定(1台目のPC)
# Dropbox以下にSublime Textの設定ファイルを移動する
cd "%appdata%\Sublime Text 3\Packages\"
mkdir %homepath%\Dropbox\Sublime
mv User %homepath%\Dropbox\Sublime\
cmd /c mklink /D User %homepath%\Dropbox\Sublime\User

# 次回設定(2台目以降のPC)
cd "%appdata%\Sublime Text 3\Packages\"
rmdir -recurse User
cmd /c mklink /D User %homepath%\Dropbox\Sublime\User
~~~
  
###Mac
ターミナルから以下のコマンドを入力

~~~sh
# 初回設定(１代目のMac)
# Dropbox以下にSublime Textの設定ファイルを移動する
cd ~/Library/"Application Support"/"Sublime Text 3"/Packages/
mkdir ~/Dropbox/Sublime
mv User ~/Dropbox/Sublime/
ln -s ~/Dropbox/Sublime/User

# 次回設定(2代目以降のMac)
cd ~/Library/"Application Support"/"Sublime Text 3"/Packages/
rm -r User
ln -s ~/Dropbox/Sublime/User
~~~

※WindowsとMacで同じDropboxの ~Packages/User フォルダを参照すると環境の違いから不具合が起こる場合があるため、WindowsとMac両方を同期する場合はフォルダを分けるとよい。  
例:  
Win : ~/Dropbox/Sublime`Win`/User  
Mac : ~/Dropbox/Sublime`Mac`/User  