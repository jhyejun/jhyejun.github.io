---
title: "[Swift] 클로저에서의 강한 순환 참조 :+1:!"
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
:+1:!
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
**클로저의 선언부에서 캡쳐 목록(capture list)을 정의**하는 것으로 해결할 수 있다.<br>
**캡쳐 목록은 클로저 본문에 하나 이상의 참조를 캡쳐할 때 사용하는 규칙을 정의한다.**<br>
두 클래스 인스턴스 사이에서의 강한 순환 참조 때 처럼,<br>
**강한 참조 대신 약한 참조 혹은 미소유 참조로 선언해서 정의**한다.<br>
상황에 따라서 약한 참조와 미소유 참조 중 선택해서 사용한다.<br>

**캡쳐 목륵의 각 항목은 클래스 인스턴스에 참조(self)하거나**<br>
**어떤 값으로 초기화된 변수(delegate = self.delegate)에**<br>
**weak나 unowned 키워드로 연결된다.**<br>
이것들은 대괄호([ ]) 안에 작성되고 콤마로 구분한다.<br>

클로저의 매개변수 목록과 반환 타입 앞에 캡쳐 목록을 위치시킨다.<br>

```
lazy var someClosure: (Int, String) -> String = {
    [unowned self, weak delegate = self.delegate!] (index: Int, stringToProcess: String) -> String in
    // closure body goes here
}
```

클로저가 매개변수 목록이나 반환 타입을 지정하지 않는다면<br>
컨텍스트에 의해 추록되기 때문에,<br>
캡쳐 목록은 클로저의 시작 부분에 위치하며 뒤에 in 키워드를 붙여준다.<br>

```
lazy var someClosure: () -> String = {
    [unowned self, weak delegate = self.delegate!] in
    // closure body goes here
}
```

클로저와 인스턴스의 캡쳐가 **항상 서로를 참조할 때,**<br>
**클로저에서 캡쳐를 미소유(unowned) 참조로 정의하고**<br>
**항상 같은 시점에 메모리에서 해제 된다.**<br>

그와 반대로, **캡쳐된 참조가 나중에 nil이 될 수 있다면,**<br>
**클로저에서 캡쳐를 약한(weak) 참조로 정의한다.**<br>
약한 참조는 항상 옵셔널 타입이고<br>
**참조하는 인스턴스가 메모리에서 해제될 때 nil이 된다.**<br>

> **캡쳐된 참조가 nil이 되지 않으면,**<br>
 **항상 약한 참조보다는 미소유 참조로 캡쳐하는게 좋다.**<br>

<br>

이전에 클로저에서 강한 순환 참조가 발생하는 코드에서<br>
캡쳐 리스트를 통해 해결한다면 미소유 참조로 해야 적절하며<br>
적용한 코드는 아래와 같다.<br>

```
class HTMLElement {
    let name: String
    let text: String?
    
    lazy var asHTML: () -> String = {
        [unowned self] in
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

이번에는 **강한 참조대신 미소유 참조를 뜻하는 [unowned self]로 캡쳐**한다.<br>

아래 코드는 이전처럼 HTMLElement 클래스를 생성하고<br>
새로운 인스턴스를 출력하는 코드이다.<br>

```
var paragraph: HTMLElement? = HTMLElement(name: "p", text: "hello, world")
print(paragraph!.asHTML())
// Prints "<p>hello, world</p>"
```

이번에는 아까와 달리<br>
**클로저에 의한 self의 캡쳐를 미소유 참조로 해서,**<br>
**캡쳐된 HTMLElement 인스턴스를 강하게 유지하고 있지 않다.**<br>

paragraph 변수의 강한 참조를 nil로 설정하면<br>
HTMLElement 인스턴스는 메모리에서 해제되고,<br>
메모리에서 해제 되었다는 메시지를 볼 수 있다.<br>