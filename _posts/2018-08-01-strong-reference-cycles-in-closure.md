---
title: "[Swift] 클로저에서의 강한 순환 참조"
layout: post
hidden: false
date: 2018-08-01 01:53
tag:
- swift
star: true
category: blog
author: jhyejun
description: strong reference cycles in closure
---

### 클로저에서의 강한 순환 참조
**클래스처럼 클로저는 참조 타입이기 때문에 강한 순환 참조가 발생**할 수 있다.<br>
클래스 인스턴스의 프로퍼티에 클로저를 할당 할 때<br>
**클로저에 참조를 할당하기 때문에 강한 순환 참조가 발생**할 수 있고,<br>
클로져의 본문이 **인스턴스를 캡쳐(capture)**할 때<br>
**클로저가 self를 캡쳐하게 되면서 강한 순환 참조가 발생**할 수 있다.<br>

**캡쳐(capture)**란 클로저의 본문에서 인스턴스의 프로퍼티에 접근하거나<br>
인스턴스의 메소드를 호출하는 것을 **캡쳐(capture)**라고 한다.<br>

아래 코드는 **클로저에서 self 참조를 사용**할 때,<br>
강한 순환 참조를 만드는 코드이다.

```
class HTMLElement {
    let name: String
    let text: String?
    
    lazy var asHTML: () -> String = {
        if let text = self.text {
            return "<\(self.name)>\(text)</\(self.name)>"
        } else {
            return "<\(self.name) />"
        }
    }
    
    init(name: String, text: String? = nil) {
        self.name = name
        self.text = text
    }
    
    deinit {
        print("\(name) is being deinitialized")
    }
}
```

아래 코드는 HTMLElement 클래스를 생성하고<br>
새로운 인스턴스를 출력하는 코드이다.<br>

```
var paragraph: HTMLElement? = HTMLElement(name: "p", text: "hello, world")
print(paragraph!.asHTML())
// Prints "<p>hello, world</p>"
```

위의 코드에서 HTMLElement 클래스는<br>
HTMLElement 인스턴스와 asHTML 값에 대한 클로저 사이에<br>
강한 순환 참조가 만들어진다.<br>

인스턴스의 asHTML 프로퍼티는 클로저에 강한 참조를 가지고 있지만<br>
클로저는 본문에서 self를 참조하기 때문에<br>
HTMLElement 인스턴스에 강한 참조를 가지고 있다는 것을 뜻한다.<br>

paragraph 변수에 nil을 설정하고<br>
HTMLElement 인스턴스에 강한 참조를 깨트리려 해도<br>
**서로 강한 참조이기 때문에 강한 순환 참조가 발생한다.**<br>
**강한 순환 참조이기에 HTMLElement 인스턴스도 클로저도**<br>
**메모리에서 해제 되지 않는다.**<br>

---

### 클로저에서의 강한 순환 참조 해결법

클로저와 클래스 인스턴스 사이에서 강한 순환 참조 해결법은<br>
**클로저의 선언부에서 캡처 목록(capture list)을 정의**하는 것으로 해결할 수 있다.<br>
**캡쳐 목록은 클로저 본문에 하나 이상의 참조를 캡쳐할 때 사용하는 규칙을 정의한다.**<br>
두 클래스 인스턴스 사이에서의 강한 순환 참조 때 처럼,<br>
강한 참조 대신 약한 참조 혹은 미소유 참조로 선언해서 정의한다.<br>
상황에 따라서 약한 참조와 미소유 참조 중 선택해서 사용한다.<br>


