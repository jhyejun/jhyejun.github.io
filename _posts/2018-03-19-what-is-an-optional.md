---
title: "[Swift] 옵셔널이 뭐길래?"
layout: post
hidden: false
date: 2018-03-19 23:12
image: "/assets/images/markdown.jpg"
tag:
- swift
star: true
category: blog
author: jhyejun
description: explain an optional
---

### 옵셔널 (Optional)
옵셔널은 enum 형태로 이루어져 있고, 옵셔널은 사용하는 건 switch 문의 방식으로 생각하면 된다.<br>

```
enum Optional<T> {  // <T> 가 뜻하는 바는 모든 타입의 옵셔널이 될 수 있다는 뜻
    case None
    case Some(T)
}
```
<br>

옵셔널 변수 x 가 있고 옵셔널 변수 x를 y에 저장할 때는 switch 문의 방식으로 생각하면 된다.

```
switch x {
    case Some(let value): y = value
    case None:  // None 일 때의 할 행동이 정의되어 있지 않아 충돌이 나게 되어 있음
}
```

하지만 충돌을 피하기 위해 if let을 사용해 옵셔널 변수인 x를 y에 저장하는 경우는 방금 전과 다른 swtich 문의 방식이다.

```
switch x {
    case Some(let value):
        y = value
    case None:
        break   // None 일 때의 할 행동이 break 이기 때문에 충돌나지 않고 그냥 지나가게 되어 있음
}
```
<br>

---

### 옵셔널 체이닝
옵셔널을 사용하기 위한 ?, !는 단순한 구문표현법이고 더 단순한 표현법인 옵셔널 체이닝이 있다.<br>
옵셔널 체이닝은 옵셔널을 사슬처럼 연결해줄 수 있다.<br>

```
var display: UILabel?

if let label = display {
    if let text = label.text {
        let x = tex.hashValue
    }
}
```

옵셔널을 안전하게 사용하기 위해 if let 으로 계속 벗겨내는 코드다.<br>
딱 보면 알 듯이 한눈에 보기 힘들다.<br>
하지만 옵셔널 체이닝을 통해 한줄로 줄일 수 있다.<br>

```
if let value = display?.text?.hashValue
```

옵셔널을 선언할 때가 아닌 옵셔널 값을 사용하려 할 때 ? 를 사용하게 되면<br>
일단 추출을 해보고 추출을 할 수 있다면 그 값을 이용해 다음으로 넘어가지만,<br>
추출 할 수 없다면 전체 표현식에서 nil 을 반환한다.<br>
어느 지점에서든 추출할 수 없다고 판단이 되면 그 전체 표현식은 nil 이 된다.<br>
<br>

---

### 옵셔널 디폴팅
또 다른 ?? 이라는 옵셔널 구문이 있다.<br>
옵셔널이 nil 일 때 기본값을 설정해주는 구문이다.

```
let s: String? = nil

if s != nil {
    display.text = s
}

else {
    display.text = " "
}
```

옵셔널인 변수가 nil 일 때 " " 공백을 주기 위한 코드인데 이 코드도 간결하게 한 줄로 줄일 수 있다.

```
display.text = s ?? " "
```

<br>
이 포스팅은 **[스탠포드 iOS 강의](https://www.inflearn.com/course/stanford-ios-한글자막-강의/){:target="_blank"}** 영상을 
참고로 하여 작성된 포스팅입니다.