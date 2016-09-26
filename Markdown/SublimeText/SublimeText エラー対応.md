#エラー対応
##Unable to open Packagesエラー
tmLanguage ファイルを削除するとSublime Text起動時に  

~~~
Error loading syntax file "Packages/...tmLanguage": Unable to open Packages/.../LaTeX.tmLanguage
~~~
みたいなメッセージのエラーが出る

**対応策**
Win:  
C:\Users\shutaro\AppData\Roaming\Sublime Text 3\Local\Session.sublime_session の エラーを起こしている tmLanguageを探して削除する  

##日本語の下に赤い波のラインが表示されるようになる  
![](http://sunsunsoft.com/contents/sublimetext/image/error_underline.png)  
こういう赤い波線が表示された場合  

シンタックスハイライトが有効になっているので F6 でオフにする。