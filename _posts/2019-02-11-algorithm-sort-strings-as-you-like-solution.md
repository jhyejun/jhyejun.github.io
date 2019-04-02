---
title: "[알고리즘] 프로그래머스 - 문자열 내 마음대로 정렬하기 문제 풀이"
layout: post
hidden: false
date: 2019-02-11 21:39
tag:
- 알고리즘
- 프로그래머스
star: true
category: blog
author: jhyejun
description: algorithm sort strings as you like solution
---

![문자열 내 마음대로 정렬하기 문제](/assets/images/blog/algorithm-sort-strings-as-you-like-solution/problem.png){: width="100%" height="70%"}

문제는 단순하다.<br>
**문자열로 된 리스트에서 각 문자열들의 n 번째 글자를 뽑아서 정렬하면 된다.**<br>

문자열에서 n 번째 글자를 뽑는 걸 반복해서 사용해야 하기 때문에<br>
**extension 으로 String 타입에 함수를 추가했다.**<br>

그리고 문자열 리스트인 strings 를 정렬시킨다.<br>
**정렬은 각 문자열에서 n 번째 글자를 뽑아서 비교하고 문자열을 오름차순으로 정렬한다.**<br>
만약 비교하려는 문자열들의 n 번째 글자가 같은 글자일 경우<br>
**문자열 자체를 비교해서 문자열을 오름차순으로 정렬한다.**<br>

```
import Foundation

func solution(_ strings:[String], _ n:Int) -> [String] {
	return strings.sorted {
	    if $0[n] == $1[n] {
	        return $0 < $1
	    } else {
	        return $0[n] < $1[n]
	    }
	}
}
	
extension String {
	subscript (i: Int) -> String {
	    return String(self[index(startIndex, offsetBy: i)])
	}
}
```
