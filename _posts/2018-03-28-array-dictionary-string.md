---
title: "[Swift] Array, Dictionary, String"
layout: post
hidden: false
date: 2018-03-28 17:26
image: "/assets/images/markdown.jpg"
tag:
- swift
star: true
category: blog
author: jhyejun
description: explain Array, Dictionary, String
---

### 배열 (Array)

```
var a = Array<String>()
... is the same as ...
var a = [String]()

let animals = ["Giraffe", "Cow", "Doggie", "Bird"]
```
a를 초기화하는 코드 두줄은 비어있는 String 타입의 배열을 생성한다는 같은 코드이다.<br>

그 아래 animals는 값을 넣어서 배열을 생성하는 것이다.<br>

반복을 해가면서 지역변수에 animals 가 각각 할당되면서<br>
Array 안에 있는 모든 요소들을 열거할 수 있다.<br>

```
// enumerating an Array
for animal in animals {
    println("\(animal)")
}
```

<br>
다음 Array 메소드인 **filter**를 사용하면 for문이나 if문을 장황하게 늘어놓지 않고<br>
한 줄로 사용 가능하다.<br>

```
filter(includeElement: (T) -> Bool) -> [T]

let bigNumbers = [2, 47, 118, 5, 9].filter({ $0 > 20})
// bigNumbers = [47, 118]
```

**filter**는 Array에 있는 메소드이다.<br>
Array에 있는 무슨 타입이든 받아서 Bool을 반환하는 클로져를 제공하는 메소드다.<br>
Array의 모든 요소마다 클로져를 실행해서<br>
클로져가 true로 반환하는 요소는 새 Array에 포함시키고<br>
true가 아닌 요소들은 제외한다.<br>
그렇게 새롭게 만들어진 Array를 filter가 반환한다.<br>
<br>

또 다른 것으로 **map**이라는 메소드가 있다.<br>

```
map(transform: (T) -> U) -> [U]

let stringfield: [String] = [1, 2, 3].map { String($0) }
```

**map**은 클로져를 받고 클로져는 Array안에 있는각 요소들을 다른 것으로 바꾸는 메소드다.<br>

위의 stringfield 부분은 Int로 된 Array를<br>
map { String($0) } 을 통해<br>
String으로 된 Array로 바꾸는 부분이다.<br>
<br>

**reduce**라고 불리는 또 다른 함수가 있다.

```
reduce(initial: U, combine: (U, T) -> U) -> U

// adds up the numbers in the Array
let sum: Int = [1, 2, 3].reduce(0) { $0 + $1 }
```

**reduce**는 Array를 하나의 결과로 줄이는 메소드다.<br>

위의 sum 부분은 초기값을 0으로 설정한 다음<br>
{ $0 + $1 } 의 클로져를 사용해서 배열 요소들을 줄이는 부분이다.<br>

설명하자면, 3개의 요소들 중 먼저 앞의 두개의 요소가 클로져를 통해 합쳐져<br>
한개의 요소가 되어서 총 2개의 요소가 되었다.<br>
남은 두개의 요소도 클로져를 통해 합쳐져 한 개의 요소가 되면 반환한다.<br>

이처럼 여러개의 요소를 한 개의 요소로 줄이는 것이다.<br>
<br>

그런데 여기서도 클로져가 소괄호 밖에 나와 있는게 보인다.<br>
이걸 **트레일링 클로져 문법**이라고 부른다.<br>

**트레일링 클로져 문법**이란<br>
메소드에서 인자가 클로져밖에 없거나 클로져가 마지막 인자일 때 클로져 부분을<br>
작은 괄호 밖으로 꺼낼 수 있는 걸 뜻한다.<br>
인자가 클로져밖에 없을 경우는 작은 괄호가 아예 없어도 된다.<br>

이 **트레일링 클로져 문법** 때문에 코드 읽기가 정말 수월하다.<br>

---

### 딕셔너리 (Dictionary)
Dictionary는 Array와 비슷하다.<br>
key와 value로 되어 있는 Array라고 생각하면 된다.<br>

```
var pac10teamRankings = Dictionary<String, Int>()
... is the same as ...
var pac10teamRankings = [String: Int]()

pac10teamRankings = ["Stanford": 1, "Cal": 10]
```
pac10teamRankings 초기화하는 코드 두 줄은 String 타입의 key와 Int 타입의 value를 가지는<br>
빈 딕셔너리를 생성하는 코드이다.<br>

그 아래 pac10teamRankings는 값을 넣어서 초기화를 시키는 것<br>

딕셔너리 요소들을 열거하는 방법은 tuple을 이용하며 된다.<br>

```
// use a tuple with for-in to enumerate a Dictionary
for (key, value) in pac10teamRankings {
    print("\(key) = \(value)")
}
```

key는 딕셔너리 안에 있는 각각의 key가 될거고<br>
마찬가지로 value는 딕셔너리 안에 있는 각각의 value가 된다.<br>

이것도 역시나 key만 원한다면 value 자리에 언더바를 넣고<br>
value만 원한다면 key 자리에 언더바를 넣으면 된다.<br>

---

### 스트링 (String)
String은 Swift에서 조금 복잡하다.<br>
전부 유니코드이기 때문이다.<br>

말 그대로 전체가 유니코드라는 뜻인데<br>
전체 유니코드는 모든 종류의 언어를 지원해서<br>
하나의 문자가 정말 많은 코드가 될지도 모른다.<br>

문자 하나는 더 이상 하나의 코드가 아니라서<br>
String에 문자단위로 순서를 매길 때 Int로 나타내지 않는 이유이다.<br>
그래서 순서는 String 안에서 어떻게 움직이는지 알고 있는 index로 구성이 된다.<br>
여러 코드로 표현되는 이모지처럼 말이다.<br>

String에는 많은 멋진 메소드들이 있다.
```
startIndex -> String.Index
endIndex -> String.Index
hasPrefix(String) -> Bool
hasSuffix(String) -> Bool
lowercaseString -> String
uppercaseString -> String
```
hasPrefix는 하나의 String이<br>
다른 string의 접두사(prefix)를 가지고 있는지 아닌지를 알려준다.<br>

일괄적으로 소문자로 바꾸는 함수(lowercaseString)와<br>
일괄적으로 대문자로 바꾸는 함수(uppercaseString)도 있다.<br>

<br>
이 포스팅은 **[스탠포드 iOS 강의](https://www.inflearn.com/course/stanford-ios-한글자막-강의/)** 영상을
참고로 하여 작성된 포스팅입니다.