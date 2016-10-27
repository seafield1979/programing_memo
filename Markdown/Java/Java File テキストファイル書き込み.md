#FileIO Write Text
テキスト書き込み

###文字列をファイルに書き込む
文字列をファイルに書き込む場合は

1. FileWriterで書き込みファイルを開く
2. FileWriterオブジェクトの write()メソッドで文字列を書き込む
3. FileWriterオブジェクトをクローズ

```sh
// テキストをファイルに書き込む
// FileWriter write()
public void writeText(String filePath) {
    try {
        FileWriter fw = new FileWriter(filePath);

        // 書き込み処理
        fw.write("こんにちは");

        // ファイルを閉じる
        fw.close();
    } catch(FileNotFoundException e) { 
        System.out.println(e);
    } catch(IOException e) {
        System.out.println(e);
    }
}
```