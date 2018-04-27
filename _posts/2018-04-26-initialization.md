---
title: "[Swift] Initialization"
layout: post
hidden: false
date: 2018-04-26 23:49
tag:
- swift
star: true
category: blog
author: jhyejun
description: explain Initialization
---

# 내용정리 전

### 초기화 (Initialization)
클래스에는 두가지 타입의 init 메소드가 있다.<br>
**convenience initializers(편의 초기화함수)**와
**designated initializers(지정 초기화함수)**가 있다.
지정 이니셜라이저는 'convenience' 단어와 함께 표시되지 않고
편의 초기화함수는 'convenience init 함수명' 이렇게 선언한다.

지정 초기화는 뱌로 위의 부모클래스에 있는 지정 초기화를 호출해야 한다.
그리고 부모 클래스에 있는 편의 초기화를 호출할 수 없다.
또한 부모 클래스의 지정 초기화 함수를 호출해야 한다.
부모 클래스 초기화 함수인 super.init을 전까지
모든 프로퍼티를 초기화 해야 한다.
그리고 부모클래스에 있는 어떤 프로퍼티를 건들기 위해서는
부모클래스의 init을 호출해야 한다.

너희 일을 하고 부모를 부른 뒤에야 프로퍼티에 접근할 수 있어

프로퍼티에 접근하기 전에 초기화하도록 하고
너희의 초기화가 그것들이 생기기 전에 자기 일을 하길 바랄거야
편의 초기화함수는 '그 클래스 안에서' 편의 init이나 지정 init을

호출하고 초기화해야 해
또 그렇게만 할 수 있어
부모를 호출할 수는 없어
편의 초기화 함수는 부모클래스의 초기화 함수를 부를 수 없어
편의 초기화함수는 어떤 프로퍼티의 값을 설정할 수 있기 전에
반드시 그 init을 호출해야 해
자신의 클래스나 부모 클래스에 있는 것 말이지
지정 초기화는 자신의 프로퍼티를 먼저 초기화 할 수 있도록 하고
그럼 그것들이 부모를 부를거고 초기화가 될거야
편의 초기화함수가 init으로 돌아올 때까지
모든 프로퍼티의 값이 설정되고 다른 것으로 다시 설정될 수 있어
이게 바로 convenience야
다른 init을 부르는 것은 완료되어야 해
어떤 프로퍼티에 접근할 수 있기 전에 말이야
내가 말하는 건 Access(접근)이지 set(설정)이 아니야
자기 클래스에 있는 모든 프로퍼티와 메소드에 접근 할 때 말야
그래서 self 무언가로 접근할 수 없는거지
전부 초기화되기 전까지는 자기 안에 있는
어떤 프로퍼티나 메소드에 접근할 수 없는거야
다른 init을 먼저 불러야만 해

왜 편의 초기화를 사용해야하는가라고 질문했는데

지정 초기화함수의 인자에 대한 적절한 기본값이 있어
그리고 사실 스위프트에서는 기본값을 두는게 가능해
기본적으로 = 다음에 초기값을 줄 수 있는 것이지
하지만 계산되는 값으로 기본값을 두길 원한다고 해보자
아마 다른 인자에 기초하겠지
편의 초기화함순느 너희가 만든 초기화함수가
더 적은 인자를 갖도록 해주고
다시 돌아서서 지정 초기화 함수를 호출하게 해주지
말그대로 편의를 위한 것일 뿐이야
휴 규칙도 참 많고
생각해야 할 것도 많고
그리고 너희의 초기화함수를 만들기 시작하면
계속 이런 규칙을 떠올려야만 해

왠만하면 너희만의 init을 만드는 걸 피하도록 해


init을 상속하기

만약 지정 init을 어떤 것도 실행하지 않는다면
부모클래스의 모든 지정 init을 상속하게 될거야
지정 init을 하나라도 실행한다면
부모클래스에 있는 지정 init을 가져오지 않아도 돼
부모클래스의 모든 지정 init을 override하거나
그것들 중 어느것도 실행하지 않는다면
부모 클래스로부터 모든 편의 초기화 함수를 상속 받을 수 있어
다시 말해 부모 클래스의 편의 초기화 함수는
부모 클래스의 지정 초기화 함수에 전부 종속적이야
서로가 함께 연결되고 동작하고 있는 것이지
그래서 그것들을 전부 오버라이드하거나
아무것도 오버라이드 하지 않고 전부 가져와야 해
편의 초기화함수가 상속받는 것을 납득할 수 있도록 말야
초기화함수를 실행하지 않으면
부모클래스의 모든 초기화함수를 갖게 될거야
편의든 지정이든 말이지
이 규칙들에 따라 상속받은 모든 init은
전에 본 슬라이드의 규칙도 적용이 돼
클래스 안에서 편의는 지정을 호출해야 한다는 등
어쩌구 저쩌구 했던 것들 말이야
상속하게 되면 이제 너의 클래스 안에 있는거야
계속 생각해 봐야 되는 것들이지

required init(필수 초기화함수)를 보자
다른 주제야
키워드 required와 함께 init을 표시할 수 있어
자식클래스는 이 init을 반드시 실행해야 한다 뜻이야
선택적인게 아니지 꼭 그렇게 해야만 해
저기 있는 규칙을 따른다면 상속 할 수도 있어
이건 꼭 실행해야 해, 필수야
수요일날 할 UIView를 예로 들 수 있어
UIView는 required init이 있어 곧 보게 될거야

Failable initializer (실패할 수 있는 초기화함수)를 보자
실패할 수도 있는 초기화함수야
Double에서 본 적이 있지
초기화함수에 String을 넘겨줬을 때
Double이 String을 받는 초기화함수가 있을 때
String을 전달하면 Double값이 아니니까
초기화에 실패해서 nil을 반환하겠지
Failable initializer는 옵셔널 값이 반환될 수 있도록 하고
이 물음표로 명시해 줄 수 있어

init?(arg1: Type1, ...) {
    // might return nil in here
}

여기에 물음표를 넣어주면 이 초기화함수를
어떤 클래스의 옵셔널을 반환하는 초기화함수로 만들어주지
초기화가 실패하면 nil을 반환할거야
이 안에서 뭔가가 잘못되면 nil을 반환하고
nil은 여기에 접근하려는 사람에게 다시 돌어올거야
이해되니?

Failable initializer는 꽤 드물어

let image = UIImage(named: "foo") // image is an Optional UIImage (i.e. UIImage?)
Usually we would use if-let for these cases ...

image로 예를 들면 UIImage 객체로 let image가 있고
이 image는 옵셔늘 UIImage 이지
아마 여기선 if let을 사용하겠지

if let image = UIImage(named: "foo") // image is an Optional UIImage(i.e. UIImage?)

if let image = UIImage(named: "foo") {
    // image was successfully created
}
else {
    // couldn't create the image
}

if let image = UIImage(named: "foo")라고 하면
{} 안에 뭐든 해주면 되겠지

이번엔 객체를 만들기야
보통은 객체를 만들 때 적절한 인자로
초기화함수를 호출해서 만들어
타입의 이름을 사용하지

let x = CaculatorBrain()
let y = ComplicatedObject(arg1: 42, arg2: "hello", ...)
let z = [String]()

이 세가지들은 상수니까 영원히 비어있을거야
하지만 가끔은 다른 객체들이 객체를 만들어줄거야
그 객체들이 메소드를 호출해서
너희에게 객체를 만들어서 줄거야
하지만 아무런 준비없이 객체를 만들 때는
이런 표현으로 객체를 만들어주면 돼
이렇게 해왔었지 그동안
꽤 확실한 내용이었어

<br>
이 포스팅은 **[스탠포드 iOS 강의](https://www.inflearn.com/course/stanford-ios-한글자막-강의/)** 영상을
참고로 하여 작성된 포스팅입니다.