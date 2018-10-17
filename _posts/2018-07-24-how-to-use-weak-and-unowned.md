---
title: "[Swift] weak와 unowned의 사용법"
layout: post
hidden: false
date: 2018-07-24 02:23
tag:
- Swift
star: true
category: blog
author: jhyejun
description: how to use weak and unowned
---

### 강한 순환 참조의 해결법
Swift는 클래스 타입의 프로퍼티로 작업할 때<br>
**강한 순환 참조(Strong Reference Cycle)를 해결**하는 두가지 방법을 제공한다.<br>
두 가지 방법으로 **약한 참조(weak reference)**와<br>
**미소유 참조(unowned reference)**를 제공한다.<br>

**약한 참조**는 다른 인스턴스의 생명주기가 짧을 때 사용하며<br>
**미소유 참조**는 다른 인스턴스가 같은 생명주기를 가지거나 더 길게 유지 될 때 사용한다.<br>

**강한 순환 참조**는 이전 게시글인<br>
[메모리 관리 ARC]({{ site.url }}/blog/memory-management-arc){:target="_blank"}에서 알 수 있다.

---

### 약한 참조 (weak reference)
**약한 참조(weak reference)**는<br>
**참조하는 인스턴스를 강하게 유지 하지 않는 참조**이며<br>
**다른 인스턴스의 생명주기가 짧을 때 사용한다.**<br>
참조하고 있는 인스턴스를 강하게 유지하지 않기 때문에,<br>
**약한 참조로 참조하고 있는 동안에 인스턴스가 메모리 해제되는 것이 가능**하다.<br>
참조하는 인스턴스가 메모리에서 해제되면,<br>
**ARC는 자동으로 약한 참조를 nil로 설정**한다.<br>
약한 참조는 언제든지 nil로 값이 변경될 수 있기 때문에<br>
**상수보다는 옵셔널 타입의 변수로 선언**되어야 한다.<br>
약한 참조의 사용법은 프로퍼티나 변수 선언 앞에 **weak** 키워드를 사용하면 된다.<br>
약한 참조를 사용하는 코드는 아래와 같다.<br>

> **주의**<br>
 ARC가 약한 참조를 nil로 설정했을 때, 프로퍼티 옵저버는 호출되지 않는다.<br>
 
<br>

```
class Person {
	let name: String

	init(name: String) { self.name = name }
	var apartment: Apartment?

	deinit { println("\(name) is being deinitialized") }
}
 
class Apartment {
	let number: Int

	init(number: Int) { self.number = number }
	weak var tenant: Person?

	deinit { println("Apartment #\(number) is being deinitialized") }
}
```

---

### 미소유 참조 (unowned reference)
**미소유 참조(unowned reference)**도 인스턴스가 참조하는 것을 강하게 유지하지 않는다.<br>
하지만 **약한 참조**와는 달리 **미소유 참조**는<br>
**다른 인스턴스와 같은 생명주기를 가지거나 더 긴 생명주기를 가질 때 사용한다.**<br>
미소유 참조의 사용법은 프로퍼티나 변수 선언 앞에 **unowned** 키워드를 사용하면 된다.<br>
**미소유 참조는 항상 값을 가지고 있는 것으로 간주하기 때문에**<br>
**ARC는 미소유 참조의 값을 nil로 설정하지 않는다.**<br>
**미소유 참조는 옵셔널이 아닌 타입의 상수로 선언되어야 한다.**<br>
미소유 참조를 사용하는 코드는 아래와 같다.

> **중요**<br>
 참조가 항상 메모리가 해제되지 않는 인스턴스를<br>
 참조하는게 확실할 때 미소유 참조를 사용한다.<br>
 인스턴스의 메모리가 해제된 후에<br>
 미소유 참조의 값에 접근을 시도하면, 크래쉬가 발생한다.<br>

<br>

```
class Customer {
	let name: String
	var card: CreditCard?

	init(name: String) {
		self.name = name
	}

	deinit { println("\(name) is being deinitialized") }
}
 
class CreditCard {
	let number: UInt64
	unowned let customer: Customer

	init(number: UInt64, customer: Customer) {
		self.number = number
		self.customer = customer
	}
	
	deinit { println("Card #\(number) is being deinitialized") }
}
```

**클래스 인스턴스 사이에서의 강한 순환 참조** 뿐만 아니라<br>
클로저에서도 강한 순환 참조가 발생할 수 있는데<br>
**클로저에서의 강한 순환 참조가 발생하는 경우와 강한 순환 참조 해결법**은<br>
[클로저에서의 강한 순환 참조]({{ site.url }}/blog/strong-reference-cycles-in-closure){:target="_blank"}에서 알 수 있다.
