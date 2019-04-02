---
title: "[Swift] 메모리 관리 ARC"
layout: post
hidden: false
date: 2018-07-23 11:12
tag:
- Swift
star: true
category: blog
author: jhyejun
description: iOS Memory Management ARC
---

### 자동 참조 계수 (ARC: Automatic Reference Counting)
**iOS는 앱의 메모리 사용을 추적하고 관리하는 자동 참조 계수(이하 ARC)를 사용한다.**<br>
ARC는 인스턴스가 더이상 필요가 없을 때 클래스 인스턴스에 사용된 메모리를 자동적으로 해제한다.<br>
간단한 것으로 프로퍼티, 상수, 변수에 레퍼런스가 지정될 때 여기에 들어있는 카운트를 증가시키고<br>
프로퍼티, 상수, 변수가 해제되면 카운트를 감소시킨다.<br>
보유한 카운트가 0이 되면 메모리를 해제하는 방식이다.<br>
대부분의 경우에 메모리 작업은 잘 작동해서 메모리 관리를 생각할 필요가 없다.<br>

하지만, 몇가지 경우에 ARC는 메모리 관리를 위한 코드 일부와 관련있는 더 많은 정보가 필요하다.<br>

> **참조 계수는 클래스의 인스턴스에만 적용**되며,<br>
 구조체와 열거형은 값 타입이지 참조 타입이 아니라<br>
 참조로 저장되거나 잔딜히지 못한다.<br>

**ARC를 쉽게 설명하자면** 사람들이 있는데 **서로의 멱살을 잡고(참조)** 있다.<br>
지금 상황에서는 **상황이 종료(메모리 할당 해제)**되지 않지만<br>
**멱살을 놓고(참조 해제)** 대화를 하다보면 **상황이 종료(메모리 할당 해제)**된다.<br>

ps. 떠오른 생각이 멱살 밖에 없어서...

---

### ARC 작동 방식 (How ARC Works)
클래스의 새로운 인스턴스를 만들때 마다,<br> 
**ARC는 인스턴스에 대한 정보를 저장하기 위해 메모리 덩어리(chunk)를 할당한다.**<br>
이 메모리는 인스턴스의 저장 프로퍼티와 연관된 값과 함께,<br>
인스턴스의 타입에 관한 정보를 유지한다.<br>

추가적으로, 인스턴스가 더이상 필요하지 않을 때 ARC는 메모리를 다른 목적으로<br>
사용할 수 있도록 인스턴스에 의해 사용된 메모리를 해제한다.<br>
**클래스 인스턴스가 더 이상 필요하지 않을 때 메모리 공간을 가지지 않는 것을 보장한다.**<br>

하지만 ARC가 사용중에 있는데 인스턴스를 할당 해제하면<br>
**인스턴스의 프로퍼티 접근이나 인스턴스 메소드의 더 이상 호출할 수 없다.**<br>
만약 **인스턴스에 접근하려 한다면 크래쉬(Crash)**가 난다.

필요한 인스턴스가 사라지지 않는지 확인해야 하고,<br>
ARC는 각 클래스 인스턴스에 현재 참조중인 프로퍼티, 상수, 변수를 추적한다.<br>
ARC는 인스턴스에 대해 활성화된 참조가 하나라도 있으면, 할당 해제하지 않는다.<br>

프로퍼티, 상수, 변수에 클래스 인스턴스를 할당할 때마다<br>
프로퍼티, 상수, 변수는 **인스턴스에 강한 참조(strong reference)를 만든다.**<br>
그 참조는 **강한 참조**로 호출되기 때문에, 인스턴스를 강하게 유지하며
강한 참조가 남아있는 만큼은 메모리 할당을 해제하지 않는다.

---

### ARC 동작 (ARC in Action)
다음 예제는 자동 참조 개수 동작 방식이고,<br>
하나의 상수 프로퍼티 name을 정의한 간단한 클래스 Person으로 시작된다.<br>

```
class Person {
    let name: String
    init(name: String) {
        self.name = name
        println("\(name) is being initialized")
    }
    deinit {
        println("\(name) is being deinitialized")
    }
}
````

Person 클래스는 인스턴스의 name 프로퍼티를 설정하고 초기화 진행중을 나타내는<br>
메시지를 출력하는 초기화(initializer)를 가지고 있다.<br>
또한 Person 클래스는 클래스의 인스턴스가 메모리에서 해제 될 때<br>
메시지를 출력하는 해제(deinitializer)를 가지고 있다.<br>

다음은 새로운 Person 인스턴스에 여러개의 참조 설정에 사용되는 Person? 타입의<br>
변수 일부를 정의하는데 **이 변수들은 옵셔널타입이기 때문에 자동으로 nil로 초기화 되고,**<br>
**현재는 Person 인스턴스를 참조하지 않는다.**<br>

```
var reference1: Person?
var reference2: Person?
var reference3: Person?
```

첫번째 변수에 Person 인스턴스를 만들어 할당한다.<br>

```
reference1 = Person(name: "John Appleseed")
// prints "John Appleseed is being initialized"
````

Person 클래스의 이니셜라이저가 호출되는 시점에 메시지를 출력하는데,<br>
이는 초기화가 되었음을 뚯한다.<br>

**새로운 Person 인스턴스가 reference1 변수에 할당되었기 때문에,**<br>
**reference1에 새로운 Person 인스턴스가 강한 참조로 된다.**<br>
**이렇게 되면 현재 강한 참조가 하나 이상이기 때문에**<br>
**ARC는 Person 이 메모리에서 유지되고 할당 해제되지 않도록 한다.**<br>

다음은 같은 Person 인스턴스를 나머지 두개의 변수에 할당한다.<br>

```
reference2 = reference1
reference3 = reference1
````

**이제 하나의 Person 인스턴스에 세 개의 강한 참조가 있다.**<br>

다음처럼 만약 강한 참조 중 두개를 nil로 할당하여 깨진다면,<br>
**하나의 강력 참조만 남고 Person 인스턴스는 할당 해제되지 않는다.**<br>

```
reference1 = nil
reference2 = nil
```

**ARC는 세번째 강력 참조가 깨지기 전까지 Person 인스턴스가 할당 해제되지 않고,**<br>
**세변째 변수에 nil로 정리하면 그 때서야 Person 인스턴스는 할당 해제 된다.**<br>

```
reference3 = nil
// prints "John Appleseed is being deinitialized"
```

---

### 강한 순환 참조 (Strong Reference Cycles)
ARC의 한가지 문제점이라 하면 두 개의 객체가 상호 참조하는 경우와 같은<br>
**강한 순환 참조가 만들어 질 수 있다는 점**이다.<br>
**이렇게 되면 이 순환 참조에 연관된 객체들은 레퍼런스 카운트가**<br>
**0에 도달하지 않게 되어 메모리 누수가 발생하게 된다.**<br>

스위프트와 같이 ARC를 기반으로 하는 메모리 관리 모델에서는<br>
**어떻게 하면 강한 순환 참조를 발생시키지 않도록 하느냐**가<br>
메모리 관리상의 가장 큰 관심사가 된다.<br>

**강한 순환 참조에 대해서 쉽게 설명하면**<br>
A와 B가 있는데 **서로 멱살을 잡고(참조)** 있다.<br>
둘 중 누가 먼저 **멱살을 놓기 시작(참조 해제)** 해야<br>
이 험악한 **상황이 종료(메모리 할당 해제)** 될텐데<br>
계속 서로 멱살을 안 놓다 보니 **험악한 상황(메모리 누수)**이 지속되고 있는 것이다.<br>
**이 상황이 바로 강한 순환 참조를 뜻한다.**

**이 강한 순환 참조를 해결하기 위한 방법**은<br>
다음 게시글인 [weak와 unowned의 사용법]({{ site.url }}/blog/how-to-use-weak-and-unowned){:target="_blank"}에서<br>
확인할 수 있다.<br>
