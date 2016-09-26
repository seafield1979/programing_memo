##正規表現処理クラス
正規表現でマッチているかチェック、マッチした文字列を取得するには以下のクラスを使うと便利。

```swift
class Regexp {
    let internalRegexp: NSRegularExpression
    let pattern: String
    
    init(_ pattern: String) {
        self.pattern = pattern
        do {
            self.internalRegexp = try NSRegularExpression(pattern: pattern, options: [.CaseInsensitive])
        } catch let error as NSError {
            print(error.localizedDescription)
            self.internalRegexp = NSRegularExpression()
        }
    }
    
    func isMatch(input: String) -> Bool {
        let matches = self.internalRegexp.matchesInString( input, options: [], range:NSMakeRange(0, input.characters.count) )
        return matches.count > 0
    }

    func matches(input: String) -> [String]? {
        let nsInput = input as NSString
        if self.isMatch(input) {
            var results = [String]()
            if let matches = internalRegexp.firstMatchInString(nsInput as String, options: NSMatchingOptions(), range: NSMakeRange(0, input.characters.count))
            {
                for i in 0...matches.numberOfRanges - 1 {
                    results.append(nsInput.substringWithRange(matches.rangeAtIndex(i)))
                }
            }
            return results
        }
        return nil
    }
}


// 正規表現テスト
func testRegEx() {
    let pattern = "(hoge1) (hoge2)"
    let str = "hoge1 hoge2"
    
    // retにマッチした文字列が配列形式で返る
    let ret:[String] = Regexp(pattern).matches(str)! //http以下を取得
    print ("--- matches ---")
    
    ret.forEach { print($0) } // $0は配列の各要素
}
```