#RandomAccessFile

###バイナリファイル読み込み
バイナリファイルを読み込みたい場合はいくつか方法があるが、RandomAccessFileクラスを使うのが一番簡単。

1. RandomAccessFileの読み込みモード("r")でファイルを開く
2. 読み込み用のメソッドでデータを読み込む(read~系のメソッド)
3. 読み込み位置を指定したい場合は seek() を使用してファイルポインタを移動する

```sh
// バイナリファイルを読み込み
// RandomAccessFile
public byte[] readBinary(String filePath) {
    try {
        // 入力ストリームの生成
        RandomAccessFile raf = new RandomAccessFile(filePath, "r");

        // 読み込み位置を指定する
        raf.seek(0);
        
        // データを読み込む
        // (読み込んだサイズ分読み込み位置がすすむ)
        byte[] readData = new byte[(int)raf.length()];
        int len = raf.read(readData);
        
        // 後始末
        raf.close();
        
        return readData;
    } catch(FileNotFoundException e) { 
        System.out.println(e);
    } catch(IOException e) {
        System.out.println(e);
    }
    return null;
}
```

###バイナリファイル書き込み
ファイルにバイナリデータを書き込むのはいくつか方法があるが、RandomAccessFileクラスを使うのが簡単。

1. RandomAccessFileの読み書き込みモード("rw")でファイルを開く
2. RandomAccessFileクラスの書き込み用のメソッドでデータを読み込む(write~系のメソッド)
3. 書き込み位置を指定したい場合は seek() を使用してファイルポインタを移動する
4. close()メソッドでファイルをクローズする

```sh
// ファイルにバイナリを書き込む
// RandomAccessFile
public void writeBinary(String fileName) {
    try {
        System.out.println("writeBinary2");
        RandomAccessFile raf = new RandomAccessFile(fileName, "rw");

        // 書き込み
        for (int i=0; i<256; i++) {
            raf.writeInt(i);
        }

        raf.close();
    } catch(FileNotFoundException e) { 
      System.out.println(e);
    } catch(IOException e) {
      System.out.println(e);
    }
}
```

