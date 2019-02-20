---
title: "[알고리즘] 프로그래머스 - 서울에서 김서방 찾기 문제 풀이"
layout: post
hidden: false
date: 2019-02-20 21:45
tag:
- 알고리즘
- 프로그래머스
star: true
category: blog
author: jhyejun
description: algorithm find kim seobang in seoul solution
---

![서울에서 김서방 찾기 문제](/assets/images/blog/algorithm-find-kim-seobang-in-seoul-solution/problem.png){: width="100%" height="70%"}

이것 또한 단순하게 풀었다.<br>

문자열 리스트에서 "Kim" 인 엘리먼트 인덱스 값을 반환하면 되기 때문에<br>
**index 함수를 사용하여 문자열 리스트에서 "Kim" 인 엘리먼트 인덱스 값을 받게 했고**<br>
**반환받은 인덱스 값은 스트링 안에 포함해서 반환하는 방법으로 풀었다.**<br>
**(index 함수는 옵셔널 값을 반환하기 때문에 옵셔널 체이닝으로 옵셔널을 풀었다.)**<br>

```
import Foundation

func solution(_ seoul:[String]) -> String {
    return "김서방은 \(seoul.index { $0 == "Kim" } ?? 0)에 있다"
}
```
