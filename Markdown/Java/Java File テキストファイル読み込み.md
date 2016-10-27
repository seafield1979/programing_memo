#FileIO Read Text

テキストファイル読み込み

###テキストファイル読み込み(１文字づつ)
1文字づつ文字を取得したい場合は

1. FileReaderでファイルを開き
2. FileReaderオブジェクトのread()で１文字取得する

```sh
// テキストファイルを１文字づつ出力
public void readChar(String filePath) {
    try {
        FileReader fr = new FileReader(filePath);

        int ch;
        while (-1 != (ch = fr.read())) {
            System.out.println((char)ch);
        }
        fr.close();
    } catch(FileNotFoundException e) { 
      System.out.println(e);
    } catch(IOException e) {
      System.out.println(e);
    }
}
```

###テキストファイル読み込み(１行づつ)
1行づつテキストを読み込みたい場合は

1. FileReaderでファイルをオープン
2. BufferReaderで1行づつ文字列を読み込む

```sh
public void readText(String filePath) {
    try {
        FileReader fr = new FileReader(filePath);
        BufferedReader br = new BufferedReader(fr);

        String line;
        // 1行づつ読み込む
        while ((line = br.readLine()) != null) {
            System.out.println(line);
        }
        br.close();
    } catch(FileNotFoundException e) { 
        System.out.println(e);
    } catch(IOException e) {
        System.out.println(e);
    }
}
```

