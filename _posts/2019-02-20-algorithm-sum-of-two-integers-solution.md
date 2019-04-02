---
title: "[알고리즘] 프로그래머스 - 두 정수 사이의 합 문제 풀이"
layout: post
hidden: false
date: 2019-02-20 21:34
tag:
- 알고리즘
- 프로그래머스
star: true
category: blog
author: jhyejun
description: algorithm sum of two integers solution
---

![두 정수 사이의 합 문제](/assets/images/blog/algorithm-sum-of-two-integers-solution/problem.png){: width="100%" height="70%"}

정말 단순하게 풀었다.<br>

a 와 b 까지 포함해서 더해야하기 때문에<br>
**a 와 b 의 대소를 구분하고**<br>
**작은 숫자에서 높은 숫자까지 반복문을 돌려 값을 계속 더한 걸 반환하는 방법으로 풀었다.**<br>

```
import Foundation

func solution(_ a:Int, _ b:Int) -> Int64 {
    var result: Int = 0
        
    if b > a {
        for i in a ... b {
            result += i
        }
    } else {
        for i in b ... a {
            result += i
        }
    }
        
    return Int64(result)
}
```
