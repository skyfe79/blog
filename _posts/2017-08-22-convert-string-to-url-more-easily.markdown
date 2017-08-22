---
layout: post
title:  "String을 URL로 좀 더 쉽게 만들기"
permalink: /convert-string-to-url-more-easily
date:   2017-08-22 00:00:00 +0900
categories: swift
---

[John Sundell](https://twitter.com/johnsundell) 트위터를 보던 중 유용한 팁이 있어서 적어 봅니다. Swift에서 String으로 URL을 만들려할 때 guard let 이나 if let 을 매번 써야 하는 불편함이 있습니다.

```swift
guard let url = URL(string: "http://blog.burt.pe.kr") else {
	...
}

or

if let url = URL(string: "http://blog.burt.pe.kr") {
	...
}
```

타입안전성 때문이지만 때로는 번거롭기도 하죠. 아래처럼 할 수 있다면 어떨까요?

```
let blogUrl : URL = "http://blog.burt.pe.kr"
```

이렇게 하려면 URL을 아래처럼 확장하면 됩니다.

```swift
extension URL: ExpressibleByStringLiteral {
    public init(stringLiteral value: StaticString) {
        
        guard let url = URL(string: "\(value)"), url != nil else {
        
        	  // 1
            let exception = NSException(
                name: .invalidArgumentException,
                reason: "Invalid URL \(value)",
                userInfo: nil
            )
            
            exception.raise()
            
            // 2
            preconditionFailure("Invalid URL \(value)")
        }
        self = url
    }
    
    public init(extendedGraphemeClusterLiteral value: StaticString) {
        self.init(stringLiteral: value)
    }
    
    public init(unicodeScalarLiteral value: StaticString) {
        self.init(stringLiteral: value)
    }
}


let url: URL = "https://blog.burt.pe.kr"
let task = URLSession.shared.dataTask(with: "https://www.google.com")
```

`주석 1` String에서 URL로 변경할 수 없을 때 Swift throw를 사용하지 않고 Foundation NSException을 만들어 예외를 던집니다. swift throw를 사용하면 함수에 throw를 표시해 줘야 하고 호출하는 쪽에서 try-catch를 사용해야 하기 때문에 이를 피하기 위함입니다. 

`주석 2` guard else 블록 내에서 return을 해야 끝이 나는데 생성자에서 guard 문이므로 빈 return 을 호출할 수 없습니다. 그래서 preconditionFailure() 함수를 사용해서 프로그램을 멈춥니다.

`주의` 편리하지만  `let url: URL = "https://blog.burt.pe.kr"` 을 보면 타입 불일치 같아서 어딘가 어색하네요. 외부에서 얻은 데이터 중에 URL 문자열 중 오류가 있을 경우에는 프로그램을 멈추지 않고 안전하게 보호 할 수 있어야 하는데 위 확장은 프로그램을 멈추게 하므로 실전에서 쓰기는 약간 애매합니다. 그냥 재미로 봅시다 :)

### 참고사항

 1. 위 주석 `1`과 `2`는 John Sundell이 만든 [Require](https://github.com/JohnSundell/Require) 코드입니다.
 2. [John Sundell 트위터](https://twitter.com/johnsundell)에는 iOS 개발에 유용한 트윗과 gist 트윗이 많이 있습니다.