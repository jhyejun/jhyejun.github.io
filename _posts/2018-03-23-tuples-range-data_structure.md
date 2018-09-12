---
title: "[Swift] Tuples, Range, Data Structure"
layout: post
hidden: false
date: 2018-03-23 16:03
image: "/assets/images/markdown.jpg"
tag:
- swift
star: true
category: blog
author: jhyejun
description: explain Tuples, Range, Data Structure
---

### 튜플 (Tuples)
서로 다른 타입들을 그룹으로 묶어서 하나의 타입을 만든다.<br>
타입이 들어갈 자리라면 튜플을 어디에서든 사용 가능하다.

```
let x: (String, Int, Double) = ("hello", 5, 0.85) // 튜플 생성
let (word, number, value) = x
print(word)     // prints hello
print(number)   // prints 5
print(value)    // prints 0.85
```
위의 코드는 튜플을 생성하고 튜플을 word, number, value 에 할당하고 출력한다.

```
let x: (w: String, i: Int, v: Double) = ("hello", 5, 0.85)
print(x.w) // prints hello
print(x.i) // prints 5
print(x.v) // prints 0.85
```
이 코드는 튜플을 생성하는 동시에 할당해서 출력한다.

```
func getSize() -> Double { return  }
// 일반적인 Double 반환 타입 함수

func getSize() -> (weight: Double, height: Double) {
return (250, 80)
} // 튜플 반환 타입의 함수

let x = getSize()
print("weight is \(x.weight)") // weight is 250
print("height is \(getSize().height)") // height is 80
```

튜플은 완벽하게 유효한 반환 타입이 될 수 있으며 여러 개의 값을 반환할 수 있다.

---

### Range
무엇이든 연속적으로 표현될 수 있는 것의 양 끝 점을 가르킨다.<br>
Range는 Array처럼 일반화 된 타입이기 떄문에<br>
Int로 된 Range나 무언가의 인덱스로 된 Range도 될 수 있다.<br>
Range는 이렇게 구성되어 있다.

```
struct Range<T> {
var startIndex: T
var endIndex: T
}
```


대표적으로 사용되는 Range의 예시이다.
```
let array = ["a", "b", "c", "d"]
let subArray1 = array[2...3]    // subArray1 will be ["c", "d"]
let subArray2 = array[2..<3]    // subArray2 will be ["c"]
for i in 27...104 { }           // Range is enumeratable, like Array, String, Dictionary
```

---

### Data Structures in Swift
Class, Struct, Enum 유사점과 차이점

유사점
- 모두 똑같은 방법으로 선언이 된다.<br>
(키워도 + 이름 + {} 구조)
- 모두 프로퍼티와 함수를 가질 수 있다.<br>
(다만 enum은 저장 프로퍼티를 가질 수 없지만 계산 프로퍼티는 가질 수 있다.<br>
enum이 기억하고 있는 저장소는 case이며 case에는 연관값이 있다.)
- Initializer(초기화 함수)를 가질 수 있다.<br>
(enum만 제외하고 초기화 함수를 가지도록 되어있다.<br>
연관값을 구별되는 값들에 할당해주기 때문에 enum은 초기화 함수를 가질 필요가 없다.)

차이점
- 클래스에서는 상속을 할 수 있지만, struct와 enum은 불가능하다.
- struct와 enum은 값으로 전달되는 값 타입, 클래스는 참조 타입으로<br>
포인터(메모리 주소)로 전달되고 힙(동적으로 할당되는 메모리 영역) 메모리에 있다.

<br>

구조체를 사용할 때는<br>
클래스보다는 더 작고 스스로 자립하며 값으로<br>
복사되는 게 말이 되고 값 타입을 원할 때 사용한다.<br>
(예를 들면 String, Int 같은 타입이나 그림을 그리기 위한 점, 직사각형 같은 것들)<br>
위에 해당사항이 없다면 클래스를 선택하면 된다.<br>
대부분 큰 것은 클래스를 사용하게 된다.<br>

참조를 하는 class는 UI를 구성하는데 많이 사용하고<br>
값을 복사하는 struct와 enum은 Model을 구성하는데 많이 사용한다.<br>
(물론 상황에 따라 다르겠지만 간단하게 이렇게 생각해도 될 거 같다.)

<br>
이 포스팅은 **[스탠포드 iOS 강의](https://www.inflearn.com/course/stanford-ios-한글자막-강의/){:target="_blank"}** 영상을
참고로 하여 작성된 포스팅입니다.