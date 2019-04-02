---
title: "[알고리즘] 프로그래머스 - 수박수박수박수박수박수? 문제 풀이"
layout: post
hidden: false
date: 2019-02-20 21:52
tag:
- 알고리즘
- 프로그래머스
star: true
category: blog
author: jhyejun
description: algorithm string iterator subak solution
---

![수박수박수박수박수박수? 문제](/assets/images/blog/algorithm-string-iterator-subak-solution/problem.png){: width="100%" height="70%"}

이것 또한 매우 단순하게 풀었다.<br>

**n 을 홀수일 경우 "수" 를 문자열 변수에 더하고**<br>
**짝수일 경우는 "박" 을 문자열 변수에 더한다.**<br>
**더한 문자열 변수를 반환하는 방법으로 풀었다.**<br>

```
import Foundation

func solution(_ n:Int) -> String {
    var result: String = ""
        
    for i in 1 ... n {
        if i % 2 == 1 {
            result += "수"
        } else {
            result += "박"
        }
    }
    
    return result
}
```
