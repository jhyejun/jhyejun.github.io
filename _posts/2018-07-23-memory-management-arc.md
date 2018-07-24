---
title: "[Swift] 메모리 관리 ARC"
layout: post
hidden: false
date: 2018-07-23 11:12
tag:
- swift
star: true
category: blog
author: jhyejun
description: iOS Memory Management ARC
---

# 현재 내용 정리 중에 있습니다.

### 자동 참조 계수(ARC: Automatic Reference Counting)
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

---

### ARC 작동 방식(How ARC Works)

클래스의 새로운 인스턴스를 만들때 마다,<br> 
ARC는 인스턴스에 대한 정보를 저장하기 위해 메모리 덩어리(chunk)를 할당한다.<br>
메모리는 인스턴스의 타입 정보와 저장 속성에 할당된 인스턴스 값을 쥔다.<br>

게다가 인스턴스가 더이상 필요가 없으면 ARC는 인스턴스에<br>
사용된 메모리를 해제하고 메모리는 다른 목적을 위해 사용되어진다.<br>
더 이상 필요가 없을 때 메모리에는 클래스 인스턴스가 공간을 차지하지 않는다는 확신한다.<br>

그러나 ARC는 사용중에 인스턴스를 할당 해제하면<br>
인스턴스의 속성 접근이나 인스턴스 메소드 호출이 더이상 가능하지 않다.<br>
대신에 인스턴스를 접근하려고 하면, 앱은 크래쉬가 날 수 있다.<br>

인스턴스가 필요한 동안에는 사라지지 않게 하기 위해선,<br>
ARC는 많은 속성, 상수 그리고 변수가 현재 각 클래스 인스턴스를 참조하기 위해 추적한다.<br>
ARC는 적어도 하나의 활성화 참조가 있는 이상 인스턴스는 할당 해제되지 않고 계속 존재한다.<br>

속성, 상수 또는 변수에 클래스 인스턴스를 할당할 때,<br>
속성, 상수 또는 변수는 인스턴스에 강한 참조를 만든다.<br>
“강한” 참조라는 참조는 인스턴스르 강하게 유지하며,<br>
강한 참조가 남아있다면 해당 인스턴스를 할당 해제하지 못한다.<br>


### ARC 사용(ARC in Action)

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

Person 클래스는 이니셜라이저를 가지며 인스턴스의 name 속성을 설정하고<br>
초기화 진행중이다고 표시하는 메시지를 출력한다.<br>
Person 클래스는 디이니셜라이저를 가지며 클래스의 인스턴스가 해제될 때 메시지를 출력한다.<br>

다음은 Person 타입의 세 개 변수를 정의하여 다양한 참조를 설정 사용하는 예제이다.<br>

```
var reference1: Person?
var reference2: Person?
var reference3: Person?
```

변수 중 첫번째는 Person 인스턴스를 만들어 할당한다.<br>

```
reference1 = Person(name: "John Appleseed")
// prints "John Appleseed is being initialized"
````

Person 클래스의 이니셜라이저가 호출되는 시점에 메시지를 출력하는데,<br>
이는 초기화가 확실하게 되었음을 말한다.<br>

새로운 Person 인스턴스가 reference1 변수에 할당되었기 때문에,<br>
reference1에 새로운 Person 인스턴스가 강력참조로 된다.<br>
ARC는 Person 인스턴스가 메모리에서 유지되고 할당 해제되지 않도록 한다.<br>

다음은 같은 Person 인스턴스를 나머지 두개의 변수에 할당한다.<br>

```
reference2 = reference1
reference3 = reference1
````

이제 하나의 Person 인스턴스에 세 개의 강한 참조가 있다.<br>

만약 강한 참조 중 두개를 nil로 할당하여 깨진다면,<br>
하나의 강력 참조만 남고 Person 인스턴스는 할당 해제되지 않는다.<br>

ARC는 세번째 강력 참조가 깨지기 전까지 Person 인스턴스가 할당 해제되지 않고,<br>
세변째 변수에 nil로 정리하면 Person 인스턴스는 할당 해제 된다.<br>

```
reference3 = nil
// prints "John Appleseed is being deinitialized"
```