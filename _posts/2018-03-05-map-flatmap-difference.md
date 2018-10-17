---
title: "[Swift] map, flatmap 무엇이 다를까?"
layout: post
hidden: false
date: 2018-03-05 17:34
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Swift
star: true
category: blog
author: jhyejun
description: explain difference to map and flatmap
---

map 은 **배열의 각 요소에게 연산을 실행**한다.


예를 들면, array.map(함수A) 를 하게 된다면 모든 요소에 함수A를 실행하고 다시 배열을 반환한다.

<pre><code>array = [1, 2, 3, 4, 5]
array.map($0 * 2)</code></pre>

위와 같이 한다면 [2, 4, 6, 8, 10] 이 반환된다.

map 내의 쓰는 함수를 단순한 연산이 아니라 클로저를 사용해 다양하게 확인할 수 있다.


그렇다면 flatmap 은 무엇일까

이름에서부터 느껴지겠지만 flatmap 은 방금 말한 map 과 크게 다르지 않다.

하지만 flatten 이라는 함수가 합쳐졌다는 게 다른 점.


flatten 함수는 중첩배열을 풀고 옵셔널을 지우는 함수.

그냥 간단하게 예제를 보면 아주 간단하게 이해가 될 것 같다.

<pre><code>var array: [Int?] = [1, [2, nil], [3, 4], nil, 5]
array.flatmap($0 * 3)</code></pre>

위와 같이 한다면 [3, 6, 9, 12, 15] 이 반환된다.

안에 있는 배열들을 풀고, 옵셔널 요소들을 지운다고 생각하면 된다.


flatmap 함수는 map 의 응용버전이라 map 함수 쓰는 상황 중 특수한 상황에 쓰게 될 것 같은데

아직 언제 쓰일 거 같은 지 감이 안잡히기는 한다.



#### 참고 블로그
<https://soooprmx.com/archives/6784>{:target="_blank"}<br>
<http://seorenn.blogspot.kr/2015/09/swift-flatmap.html>{:target="_blank"}