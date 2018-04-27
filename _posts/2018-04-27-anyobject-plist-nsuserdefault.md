---
title: "[Swift] AnyObject, Plist, NSUserDefault"
layout: post
hidden: false
date: 2018-04-27 01:38
tag:
- swift
star: true
category: blog
author: jhyejun
description: explain AnyObject, Plist, NSUserDefault
---

# 내용정리 전

### AnyObject
조금 복잡한데 이건 특별한 타입이야
실제로는 프로토콜인데 아직 공부하지 않았으니까
어쨌든 이건 특별한 타입이니까 '타입'으로 생각하면 돼
프로토콜도 타입일거야
AnyObject는 Objective-C와의 호환을 위해 사용되곤 했어
지금도 그렇게 흔히 쓰여
우리 수업중에 Objective-C 를 배우려고 할 수도 있는데
Objective-C에는 id라고 하는 매우 중요한 타입이 있어
id는 모르는 클래스의 객체에 대한 포인터 주소를 말해
그러니까 뭐든 될 수 있는 매우 열려있는 타입이곘지
스위프트에서는 그렇게 하지 않아
스위프트는 타입을 분명하게 지정해주어야 해
하지만 스위프트로도 iOS의 모든 API를 사용해야하니까
AnyObject라고 하는 타입이 도입된거야
그래서 스위프트에서 AnyObject도
모르는 클래스의 객체에 대한 포인터 주소를 뜻하겠지
클래스에 대한 객체에만 사용하지 구조체에는 사용하지 않아
Objective-C 에서의 의미는 같지
그래서 요즘에는 대신에 불투명한 타입을 사용해
객체에 대한 포인터를 갖고 싶은데 그게 무슨 타입인지 모르거나
다른 사람이 무슨 타입인지 알지 못하게 하고 싶을 때 말이지
그럼 이제 어떻게 하면 될지 이야기 해보자
클래스에만 해당되고 구조체에는 아니라고 했고
그건 그렇고 스위프트에는 Any라고 하는 타입도 있어
말 그대로 뭐든 될 수 있는거야
구조체든 클래스든
사실 우리 수업에서는 Any를 전혀 사용하지 않을거야
너희도 전혀 사용하고 싶지 않을거야
스위프트 안에 있긴 한데
내 생각엔 언어를 좀 더 완벽하게 만들기 위해 넣은 것 같긴한데
난 이걸 너희가 왜 사용해야 하는지 이유를 떠올리기 힘들어
그래서 Any는 다루지 않을거야
우리는 AnyObject에 대해선 알아볼거야
그럼 어디서 AnyObject를 보게 될까?
메소드 인자의 타입이 적어도 두개 이상 될 수 있을 때
예를 들어, 다음 수업에서 prepareForSegue 라는 걸 볼텐데
하나의 MVC로부터 다른 MVC로 화면을 전환을 준비하지
언젠가 여러개의 MVC를 만들게 될거고
한 MVC에서 다른 MVC로 전환을 할 때
prepareForSegue는 그 전환을 준비하는 메소드야
이 메소드의 sender, 그러니까 전환을 초기화하는 객체는
수많은 종류의 타입이 될 수 있잖아
클릭할 수 있는 버튼이 되거나 테이블의 한 줄이 될 수도 있고
컨트롤러 안에 커스텀한 코드가 될지도 모르는 거야
그러니까 이 sender는 AnyObject가 되어야만 해
우리는 이게 버튼일지 테이블의 한 줄일지 뭔지 모르니까
어떤 객체가 많은 타입이 될 수 있어서 무슨 타입이 될 지 모를 때
이걸 AnyObject라고 타입을 지정해주는거야
또 다른 예로는 계산기 앱에서 봤던 touchDigit 메소드야
IBaction을 만들기 위해 컨트롤 키를 누르고 코드로 끌어왔을 때
타입을 AnyObject에서 UIButton으로 바꿔준 것 기억하니?
그렇게 바꾸지 않았더라면
touchDigit의 sender를 AnyObject 타입으로 만들었을거고
touchDigit은 UIButton이나 UISlider 아니면
다른 무언가로 메시지가 보내졌을거야
우린 그렇게 하지 않았지
우린 touchDigit 안에서 sender가 UIButton이길 원했으니까
그래서 currentTitle(현재의 타이틀)같은
UIButton의 프로퍼티를 사용할 수 있었던 거야
하지만 AnyObject가 될 수도 있어
AnyObject를 사용하는 다른 예는 cookie(쿠키)를 반환하려고 할 때야
쿠키는 돌려받는 건데 누군가한테 뭔가를 주면
그들은 그 안에 뭐가 들어있는지 모르고 너도 말을 해주지 않겠지
그냥 그대로 돌려받기만 하는거야
그러니까 쿠키는 상태를 저장하고
뭔가를 기억하고 있다가 돌려주는거야
인터넷 브라우저도 쿠키가 있어서
브라우저를 통해 사이트에 들어가면
사이트는 사이트에 대한 것과 너에 대한 정보를
쿠키안에 저장하고 있다가 사이트를 나갔다가 다시 들어왔을 때
쿠키를 들여다보고 해석할거야
사이트는 브라우저에게 쿠키를 제공하지만
브라우저는 그 안에 뭐가 있는지 알 수 없어
불분명한 타입인거지
브라우저가 아는건 AnyObject라는 것일 뿐이야

그럼 AnyObject 타입을 어떻게 사용할 수 있을까?
우린 이게 무슨 타입이 될지 모르잖아
우린 우리가 아는 타입으로 변환을 해주어야 해
사실 이렇게 변환하는 건 가능하지 않을지도 모르지
변환하려고 할 떄 그 타입이 아닐 수도 있으니까
그래서 스위프트에선 as 키워드를 사용하는데
다른 타입으로 변환하는 걸 '시도' 하는 표현이야
다시 말해 AnyObject를 어떤 타입'으로서'(as) 대하겠다는거야
선택적으로 말이야
그래서 이건 옵셔널을 반환해
우린 보통 as에 if-let 구문을 사용해
as가 옵셔널을 반환하니까

```
let ao: Anyobject = ...
if let foo = ao as? SomeClass {
    // we can use foo and know that it is of type SomeClass in here
}
```

ao 라는 AnyObject 타입의 지역변수가 하나 있는데
내가 모르는 어떤 클래스(someClass)로 할당이 될거야
그렇지만 난 ao가 SomeClass라면 사용하려고 시도해 볼거야
ao는 SomeClass 일지도 모르니까 SomeClass가 맞는지 보고
맞다면 SomeClass로 사용할거야
if let foo = ao as? SomeClass {} 라고 하면
이 괄호안에는 AnyObject가 아닌 SomeClass 타입인
지역변수 foo가 들어가게 될거야
알겠니?
이정도로 간단해
기본적으로 AnyObject를 특정한 클래스로 '캐스팅' 하는거야
조건에 맞다면 말야
이게 AnyObject를 사용하는 방법이야
그렇지 않으면 그 안에 무엇이 들어있는지 모르고 전달만 하는거야
무엇을 해야할지 아는 사람에게 전달하지
쿠키라면 우리는 무엇을 해야할지 모르니까

### Property List
AnyObject를 사용하는 또 다른 곳은 Property List야
Property List는 기본적으로 Array, Dictionary, String,
Double, Int, NSData, NSDate의 조합으로 되어 있어
이 클래스들로 데이터 구조를 만들면 Property List가 생긴거야
그러니까 이건 그냥 그걸 표현하기 위한 단어일 뿐이야
하지만 너흰 의문이 생길지도 몰라
AnyObject는 구조체가 아니라 클래스로만 캐스팅 되는데
String, Array, Dictionary, Double은 구조체 잖아
그럼 어떻게 이것들이 AnyObject가 되어서
Property List에 들어갈 수 있는지 말이야
답은 '브리징'이야
이것들이 Objective-C로 자동으로 브리핑되고
자동으로 클래스인 NSDictionary, NSArray, NSNumber로 다뤄져서
AnyObject가 되도록 허락하는거야
Property List는 보이지 않게 전달 돼
그 안을 보는 사람은 Dictionary, Array, String 같은게
있다는 것만 알고 데이터가 무엇을 의미하는지는 아무것도 몰라
그냥 전달되기만 하는거야 이해하기 쉽게 Property List를 사용하는 iOS의 API를 보자

## NSUserDefault
NSUserDefault 라는게 있는데
NSUserDefault가 하는 일은 Property List를 받아서
디스크에 영구적으로 기억되게 만들어
그래서 앱을 종료했다가 다시 켜서 봐도 그대로 있는 것이지
그러니까 기본적으로 Property List의 데이터 베이스인 셈이야
Dictionary, Array, String, Date 구조체의 데이터 모음이야
이건 작은 데이터베이스이니까
영어사전같이 영어 전체를 저정해야하는 큰 데이터에 사용하지마
'설정'같은 작은 것에만 사용해
API로 간단하게 표현되는데
setObject(AnyObject, forKey: String)으로
Property List를 저장하고 다시 가져올 수도 있는거야
이렇게 '쿠키'로 저장되고 전달되는 과정을 볼 수 있을거야
NSUserDefault는 그 안에 Dictionary, Array, String이
있다는 것만 알지 구체적으로 무슨 데이터가 들었는지는 몰라
더 작게 저장하고 불러오는 것도 가능해
예를 들어 Double로 Property List를 하나 만들 수 있어
왜냐하면 Double 그 자체가 Property List이거든
Array처럼 이 클래스 들 중 하나니까
어떻게 NSUserDefault를 사용할까?
클래스 메소드 혹은 타입 메소드를 사용해서
공유해서 사용할 것을 새로 만들거야
NSUserDefault.standardUserDefualt()라고 치면
standardUserDefault의 공유되는 인스턴스를 만들 수 있어
그리고 특정한 Property List를 가져오도록 해서
데이터를 수정하게 되면 자동으로 저장이 되지만
강제로 디스크에 저장하게 하고 싶다면
Bool 값을 반환하는 동기화를 해 줄 수 있어
반환값은 우리가 거의 무시하게 될텐데
실패했을 때 무엇을 해야할지 확실하지 않기 때문이야
아마 디스크가 꽉 차서 실패하거나 할텐데
그 시점에 무엇을 해야할지 모르잖아
어쨌든 대개는 반환값을 무시하면 돼

### Casting
캐스팅에 관한 건데 캐스팅은 AnyObject에만 쓰이는 건 아냐
예를 들면, 나중에 슬라이드에서 볼 수 있겠지만
어떤 클래스의 자식 클래스가 하나 있다고 하면
as를 사용해서 캐스팅해 볼 수 있겠지
자식 클래스인지 아닌지 확실치 않은 클래스로 캐스팅할 수 있고
as가 대신 자식 클래스인지 아닌지 말해줄거야
여기 슬라이드에서 예를 들면,
ViewController의 기본 클래스인 vc가 있다고 하면
vc.displayValue 라고 할 수는 없지만
as 를 통해 캐스팅하면 다른 클래스의 자식 클래스를 사용 가능하다

<br>
이 포스팅은 **[스탠포드 iOS 강의](https://www.inflearn.com/course/stanford-ios-한글자막-강의/)** 영상을
참고로 하여 작성된 포스팅입니다.