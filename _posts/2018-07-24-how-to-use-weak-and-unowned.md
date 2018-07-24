---
title: "[Swift] weak와 unowned의 사용법"
layout: post
hidden: false
date: 2018-07-24 02:23
tag:
- swift
star: true
category: blog
author: jhyejun
description: how to use weak and unowned
---

# 현재 내용 정리 중에 있습니다.

강력 순환 참조(Strong Reference Cycle)를 벗어나기 위해<br>
약한 참조(weak reference)와 미소유 참조(unowned reference)를 사용합니다.<br>

weak와 unowned를 언제 사용해야 하는지 정리해보았습니다.<br>

### weak와 unowned
weak와 unowned의 차이점은 옵셔널이냐 옵셔널이 아니냐의 차이입니다.<br>
즉, unowned는 값이 있음을 가정하고 사용하며<br>
unowned 값이 nil이라고 한다면 크래쉬가 발생할 수 있습니다.<br>

다음은 weak를 사용하는 코드입니다.<br>

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

다음은 unowned를 사용하는 코드입니다.<br>

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

Apartment 클래스에서 tenant 변수는 옵셔널로 사용하기 때문에<br>
순환 참조를 피하기 위해서는 weak로 사용합니다.<br>
CreditCard 클래스에서 customer 상수는 항상 값을 가지고 있어야 하므로<br>
순환 참조를 피하고자 unowned로 사용합니다.<br>