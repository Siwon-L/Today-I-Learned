# [NSMutableAttributedString](https://developer.apple.com/documentation/foundation/nsmutableattributedstring)
> A mutable string with associated attributes (such as visual style, hyperlinks, or accessibility data) for portions of its text.
> 텍스트 부분에 대한 관련 속성(예: 비주얼 스타일, 하이퍼링크 또는 접근성 데이터)이 있는 변경 가능한 문자열

이 클래스는 문자열에서 부분적으로 속성을 수정 및 삭제 할 수 있다.

<img src="https://i.imgur.com/66VakXZ.png" width="600">

위 그림과 같이 문자열 일부만 속성을 변경시켜 보쟈!!!

먼저, 

```swift
let attribtuedString = NSMutableAttributedString(string: text)
```

위와 같이 문자열을 이용하여, 속성 변화가 가능한 `NSMutableAttributedString` 인스턴스를 만들어준다.
그다음 `attribtuedString`인스턴스에 접근해서 `addAttribute` 메소드를 이용하여 

```swift
attribtuedString.addAttribute(.font, value: UIFont.preferredFont(forTextStyle: .title1), range: range)
```
위 처럼 특정 `range`의 부분의 속성만 변경 시켜줄 수 있다.
여기서 `range`는 [`NSRange`](https://developer.apple.com/documentation/foundation/nsrange) 타입이다. 

```swift
let range = (text as NSString).range(of: String)
```
위 처럼 String을 `NSString`으로 타입 케스팅하여 `.range`를 이용해 특정 문자열의 `NSRange`를 얻을 수 있고, 

```swift
let range = NSMakeRange(_ loc: Int, _ len: Int)
```

아니면, 위와 같이 `NSMakeRange()` 통해 직접 `NSRange`생성할 수 도 있다.
마지막으로, 이렇게 일부의 속성만 변경한 `NSMutableAttributedString`를 

```swift
content.attributedText = attribtuedString
```

content의 `attributedText` 파라미터에 저장한다.
위 같이했을 경우 폰트에 대한 속성만 변경된다. 여기서 다중 속성을 변경하려면, `addAttribute()` 대신 `addAttributes()` 메소드를 사용할 수 있다. `addAttributes()` 는 딕션어리를 이용하여 다중 속성을 변경할 수 있다.

**전체코드**

```swift

content.changePartColorAndFont(content.text!, "-", "-")      
cell.contentConfiguration = content

extension ViewController {
     mutating func changePartColorAndFont(_ data: String, _ from: Character, _ to: Character) {
        let text = self.text ?? ""
        let attribtuedString = NSMutableAttributedString(string: text)
        let firstIndex = Array(data).firstIndex(of: from) ?? 0
        let lastIndex = Array(data).lastIndex(of: to) ?? 0
        let range = NSMakeRange(firstIndex, lastIndex - firstIndex + 1)
        attribtuedString.addAttributes([.font: UIFont.preferredFont(forTextStyle: .title1), .foregroundColor: UIColor.blue], range: range)
        
        self.attributedText = attribtuedString
    }
}
```
