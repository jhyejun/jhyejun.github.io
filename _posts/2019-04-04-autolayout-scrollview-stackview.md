---
title: "[IOS] 오토레이아웃 - ScrollView, StackView"
layout: post
hidden: false
date: 2019-04-04 20:57
tag:
- IOS
- AutoLayout
- 오토레이아웃
star: true
category: blog
author: jhyejun
description: autolayout scrollview stackview
---

### 오토레이아웃 (AutoLayout)
**오토레이아웃이란?**<br>
두개의 뷰 사이의 관계를 **제약 조건(Constraints)** 이라는 것을 이용하여<br>
**뷰의 위치 혹은 크기를 지정**하는 것이다.<br>

쉽게 말하자면, A B C 라는 사람(뷰)이 있으면<br>
A는 왼쪽 벽에 딱 붙어 있어야한다는 조건,<br>
C는 오른쪽 벽에 딱 붙어 있어야한다는 조건,<br>
B는 A의 오른쪽에서 3m 떨어지고<br>
C의 왼쪽에서 5m 거리에 있어야 한다는 조건을 걸어서<br>
A B C 사람(뷰)들의 각 위치를 알 수 있고<br>
사람(뷰)들의 움직일 수 있는 행동반경(크기)를 지정할 수 있다.<br>

<br>
**오토레이아웃의 장점**<br>
- **화면 회전 혹은 기기 해상도 어디에든 잘 맞게** 화면이 구성된다.
- **유지보수**성이 뛰어나진다.

---

### 스크롤뷰 (ScrollView)
**스크롤뷰는 안에 있는 콘텐츠가 자신보다 커져야 스크롤이 가능**하다.<br>

**스크롤뷰에서 오토레이아웃을 사용하는 것은 이제껏 적용해왔던 방법**과는 다르다.<br>
이전까지는 기준이 되는 뷰에서와 자신에게 제약조건을 적용해 오토레이아웃을 사용했는데<br>
이 방법은 **기준이 되는 뷰의 크기 혹은 위치를 정확하게 알 수 있을 때 가능한 방법**이다.<br>

하지만 스크롤뷰는 스크롤이 가능해서<br>
**스크롤뷰의 크기가 무한으로 커질 수 있기 때문에**<br>
기존의 적용해왔던 방법으로는 부족한 점이 있다.<br>

그래서 스크롤뷰에서 오토레이아웃을 사용하는 방법은 무엇이냐?<br>

**스크롤뷰 콘텐츠들을 감싸줄 콘텐츠 래퍼뷰(Contents Wrapper)**를 만들어서<br>
콘텐츠들을 감싸준 콘텐츠 래퍼뷰를<br>
**스크롤뷰 내부에서 꽉차게끔 상하좌우의 제약조건을 걸고**<br>
**스크롤뷰와 높이가 같게끔 제약조건을 걸어주면**<br>
이전까지 사용하던 오토레이아웃 적용 방법을 사용할 수 있다.<br>

하지만, 이럴 경우 콘텐츠들이 많아져서 스크롤뷰보다 더 커질 경우<br>
스크롤뷰와 높이가 같다고 제약조건을 걸었기 때문에<br>
**스크롤이 안 되는 문제**가 생긴다.<br>

이 문제를 해결하기 위해서는<br>
아까 걸었던 **스크롤뷰와 컨텐츠 래퍼뷰의 높이가 같다는 제약조건**의<br>
**priority(우선순위) 를 낮게** 잡아주면<br>
콘텐츠들이 많아져도 스크롤이 가능해지게 된다.<br>

---

### 스택뷰 (StackView)
> **스택뷰는 iOS 9 부터 나온 혜성같은 존재**

<br>
**스택뷰는 여러개의 뷰를 하나의 뷰로 합쳐주는 뷰**이다.<br>
합쳐졌던 여러개의 뷰는 스택뷰의 스택으로 존재한다.<br>

스택뷰를 사용하게 되면 각각 뷰에 걸려있던 제약조건을<br>
합치면서 뷰들을 관리하기가 쉬워진다.<br>

스택뷰의 주요 프로퍼티는<br>
distribution, alignment, spacing 가 있다.<br>

하나씩 알아보면,

**Distribution**
- fill (각 내부 뷰들의 컨텐츠 사이즈. 단 누가 늘어나도 되는지 priority를 통해 정해져야함)
![distribution_fill](/assets/images/blog/autolayout-scrollview-stackview/distribution_fill.png){: width="100%" height="50%"}
- fillEqually (내부 뷰들의 크기를 같게)
![distribution_fillEqually](/assets/images/blog/autolayout-scrollview-stackview/distribution_fillEqually.png){: width="100%" height="50%"}
- fillProportionally (각 뷰들의 콘텐츠 사이즈의 비율대로 스택뷰 사이즈를 비율 분할함)
![distribution_fillProportionally](/assets/images/blog/autolayout-scrollview-stackview/distribution_fillProportionally.png){: width="100%" height="50%"}
- equalSpacing (내부 뷰들 사이의 간격을 Spacing 값으로 설정)
![distribution_equalSpacing](/assets/images/blog/autolayout-scrollview-stackview/distribution_equalSpacing.png){: width="100%" height="50%"}
- equalCentering (내부 뷰들의 Center 간격을 같게 설정)
![distribution_equalCentering](/assets/images/blog/autolayout-scrollview-stackview/distribution_equalCentering.png){: width="100%" height="50%"}

<br>
**Alignment**
- fill (정렬없이 채우기)
![alignment_fill](/assets/images/blog/autolayout-scrollview-stackview/alignment_fill.png){: width="100%" height="50%"}
- leading (텍스트가 시작하는 쪽으로 정렬)
![alignment_leading](/assets/images/blog/autolayout-scrollview-stackview/alignment_leading.png){: width="100%" height="50%"}
- top (위쪽 정렬)
![alignment_top](/assets/images/blog/autolayout-scrollview-stackview/alignment_top.png){: width="100%" height="50%"}
- firstBaseline (첫째줄 텍스트의 아랫라인으로 정렬)
![alignment_firstBaseline](/assets/images/blog/autolayout-scrollview-stackview/alignment_firstBaseline.png){: width="100%" height="50%"}
- center (가운데 정렬)
![alignment_center](/assets/images/blog/autolayout-scrollview-stackview/alignment_center.png){: width="100%" height="50%"}
- trailing (텍스트가 끝나는 쪽으로 정렬)
![alignment_trailing](/assets/images/blog/autolayout-scrollview-stackview/alignment_trailing.png){: width="100%" height="50%"}
- bottom (아랫쪽 정렬))
![alignment_bottom](/assets/images/blog/autolayout-scrollview-stackview/alignment_bottom.png){: width="100%" height="50%"}
- lastBaseline (마지막줄 텍스트의 아랫라인으로 정렬)
![alignment_lastBaseline](/assets/images/blog/autolayout-scrollview-stackview/alignment_lastBaseline.png){: width="100%" height="50%"}

<br>
**Spacing**
스택뷰 내부의 뷰들간의 간격<br>

<br>
**스택뷰의 장점**
- 조작이 쉽다.
- 조립하기 쉽다.
- 유지보수가 쉽다.
- 가볍게 쓸 수 있다.

---

### 정리
위에서 배운 스크롤뷰에서 콘텐츠 래퍼뷰를<br>
스택뷰로 사용하면 더 효과적으로 사용할 수 있다.<br>


**이 글은 '야곰님의 오토레이아웃 정복하기' 에서 배운 걸 정리하였습니다.**

#### 잘못된 부분에 대한 피드백은 언제나 작성자에게 힘과 도움이 됩니다.
