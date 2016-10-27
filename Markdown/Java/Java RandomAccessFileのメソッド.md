#RandomAccessFileのメソッド
Javaでファイル入出力するにはRandomAccessFileクラスがあれば他のクラスは必要ない、というくらい便利なクラス

###基本系

|戻り値|メソッド|内容|
|---|---|---|
|void | seek(long) | ファイルの先頭を起点として、ファイルポインタを設定します。
|void | close() | RandomAccessFileストリームを終了し、すべてのリソースを開放します。
|long | getFilePointer() | ファイルに対する現在のファイルポインタの位置を返します。
|long | length() | ファイルの長さを返します。

###read系

|戻り値|メソッド|内容|
|---|---|---|
|int | read( ) | ファイルから1バイトのデータを読み込みます。
|int | read(byte[] b) | このファイルから最大 b.length バイトのデータをバイトの配列の中に読み取ります。
|int | read(byte[] b, int off, int len) | このファイルから最大 len バイトのデータをバイトの配列の中に読み取ります。
|char | readChar( ) | ファイルからUnicode文字を読み込みます。
|String | readLine( ) | ファイルから1行を読み込みます。
|String | readUTF( ) | ファイルから文字列を読み込みます。
|byte | readByte( ) | ファイルから符号付8ビット値を読み込みます。
|double | readDouble( ) | ファイルからdoubleを読み込みます。
|float | readFloat( ) | ファイルからfloatを読み込みます。
|int | readInt( ) | ファイルから符号付32ビット整数を読み込みます。
|long | readLong( ) | ファイルから符号付64ビット整数を読み込みます。
|short | readShort( ) | ファイルから16ビット数を読み込みます。
|int | readUnsignedByte( ) | ファイルから符号なし8ビット数を読み込みます。
|int | readUnsignedShort( ) | ファイルから符号なし16ビット数を読み込みます。

###write系

|戻り値|メソッド|内容|
|---|---|---|
|void | write(int b) | このファイルに指定されたバイトを書き込みます。
|void | write(byte[]) | 現在のファイルポインタの位置から、引数に指定されたbyte配列分のデータをファイルに書き込みます。
|void | write(byte[] b, int off, int len) | 指定されたバイト配列のオフセット off から len バイトを、このファイルに書き込みます。
|void | writeUTF(String) | マシンに依存しないUTF-8エンコーディングを使って、文字列をファイルに書き込みます。
|void | writeChar(int) | charを2バイト値としてファイルに書き込みます。
|void | writeChars(String) | 文字列を一連の文字としてファイルに書き込みます。
|void | writeBoolean(boolean) | booleanを1バイト値としてファイルに書き込みます。
|void | writeByte(int) | byteを1バイト値としてファイルに書き込みます。
|void | writeBytes(String) | 文字列を一連のバイト値としてファイルに書き込みます。
|void | writeDouble(double) | doubleをlongに変換してから、そのlong値を8バイト値として上位バイトから先にファイルに書き込みます。
|void | writeFloat(float)  | floatをintに変換してから、そのint値を4バイト値として上位バイトから先にファイルに書き込みます。
|void | writeInt(int) | intを4バイト値としてファイルに書き込みます。
|void | writeLong(long) | longを8バイト値としてファイルに書き込みます。
|void | writeShort(int) | shortを2バイト値としてファイルに書き込みます(上位バイトから先に書き込む)。