---
title: "[Swift] Initialization"
layout: post
hidden: false
date: 2018-04-26 23:49
tag:
- Swift
star: true
category: blog
author: jhyejun
description: explain Initialization
---

### 초기화 (Initialization)

Swift 에서의 객체는 반드시 저장 프로퍼티에 대해서 초기화가 요구된다.<br>
하지만 시스템 자체적으로 초기화를 해주지 않기 때문에<br>
3가지 방법 중 하나를 택해서 초기화를 해줘야 한다.

1. **초기값 지정** (let x = 0)
2. **옵셔널** (let x: Int?)
3. **Initializer** 를 통해 초기화

1번 2번은 이미 알고 있고 3번에 대해서 알아보려고 한다.

기본적으로 class의 프로퍼티를 모두 초기값을 지정을 해줘야 한다는 것을 명심하자.

클래스에는 두가지 타입의 init 메소드가 있다.<br>
**Convenience Initializer (편의 초기화 함수)**와<br>
**Designated Initializer (지정 초기화 함수)**가 있다.<br>

#### Designated Initializer (지정 초기화 함수)
Designated Initializer (지정 초기화 함수)는 클래스에 반드시 1개 이상 필요하고<br>
**초기화가 필요한 모든 프로퍼티를 단독으로 초기화 가능한 초기화 함수**이다.

#### Convenience Initializer (편의 초기화 함수)
Convenience Initializer (편의 초기화 함수)는<br>
**초기화가 가능한 모든 프로퍼티를 단독으로 초기화할 수 없다.**<br>
일부 프로퍼티만 처리한 뒤에 다른 Initializer를 이용해서 전체 초기화를 수행한다.<br>
(편의 초기화 함수는 중복되는 초기화 코드 방지를 위해 사용된다.)

```
convenience init(red: Int, green: Int, blue: Int, alpha: CGFloat) {
    let floatRed = CGFloat(red) / 255
    let floatGreen = CGFloat(green) / 255
    let floatBlue = CGFloat(blue) / 255
        
    self.init(red: floatRed, green: floatGreen, blue: floatBlue, alpha: alpha)
}
```
이 코드는 UIColor 클래스의 편의 초기화 함수이다.<br>
코드를 보는 것과 같이 초기화 과정은 편의 초기화 함수에서<br>
지정 초기화 함수 순서로 동작한다.

---

## Initializer 상속

만약 부모 클래스의 지정 초기화 함수를 어떤 것도 실행하지 않는다면<br>
부모클래스의 모든 지정 초기화 함수를 상속하게 된다.

하지만 지정 초기화 함수를 하나라도 실행한다면<br>
부모클래스에 있는 지정 초기화 함수를 가져오지 않아도 된다.<br>
부모클래스의 모든 지정 초기화 함수를 **override**하거나<br>
아무것도 **override**하지 않는다면<br>
부모 클래스로부터 모든 편의 초기화 함수를 상속 받을 수 있다.

다시 말해서 부모 클래스의 편의 초기화 함수는<br>
부모 클래스의 지정 초기화 함수에 전부 종속적이라서<br>
서로가 함께 연결되고 동작하고 있는 것이라고 생각하면 된다.

그래서 그것들을 전부 **오버라이드**하거나<br>
아무것도 **오버라이드** 하지 않고 전부 가져와야한다.<br>

만약 초기화함수를 실행하지 않는다면<br>
부모클래스의 편의든 지정이든 구분없이 모든 초기화함수를 갖게 된다.<br>

---

## Required Initializer (필수 초기화 함수)
키워드 **required**와 함께 init을 표시할 수 있다.<br>
자식클래스는 이 init을 반드시 실행해야 한다는 뜻인데<br>
**override** 키워드는 생략된다.<br>
(**편의 초기화 함수**인 경우 **override** 키워드는 생략하지 않는다.)

---

## Failable Initializer (실패할 수 있는 초기화 함수)
실패할 수도 있는 초기화함수이다.<br>
**Failable Initializer**는 옵셔널 값이 반환될 수 있도록 하고<br>
**물음표**로 명시해 줄 수 있어

```
init?(arg1: Type1, ...) {
    // might return nil in here
}
```

위 코드처럼 물음표를 넣어주면 이 초기화함수를<br>
어떤 클래스의 옵셔널을 반환하는 초기화함수로 만들어준다.<br>
초기화가 실패하면 nil을 반환하고<br>
혹은 이 안에서 뭔가가 잘못되면 nil을 반환한다.

**Failable initializer**를 사용하는 곳은 꽤 드물다.

```
if let image = UIImage(named: "foo") // image is an Optional UIImage(i.e. UIImage?)

if let image = UIImage(named: "foo") {
    // image was successfully created
}
else {
    // couldn't create the image
}
```

image로 예를 들면 UIImage 객체로 let image가 있고<br>
이 image는 옵셔널 UIImage 이지<br>
아마 여기선 if let을 사용할 것이다.

<br>
이 포스팅은 **[스탠포드 iOS 강의](https://www.inflearn.com/course/stanford-ios-한글자막-강의/){:target="_blank"}** 영상을
참고로 하여 작성된 포스팅입니다.<br>
그리고 또 **[giftbot님의 Swift init 초기화 이해하기](https://m.blog.naver.com/PostView.nhn?blogId=itperson&logNo=220995098199&categoryNo=103&proxyReferer=https%3A%2F%2Fwww.google.com%2F){:target="_blank"}**에서도 참고했습니다.<br>

이 **Initializer** 파트는 너무 어려워서 이해가 잘 안됐다..<br>
추후에 구글링을 통해서 다시 한번 정리가 필요할 것 같다.