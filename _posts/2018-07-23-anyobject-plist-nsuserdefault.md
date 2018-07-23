---
title: "[Swift] 클로저에서 weak와 unowned의 사용법"
layout: post
hidden: false
date: 2018-07-23 11:12
tag:
- swift
star: true
category: blog
author: jhyejun
description: explain AnyObject, Plist, NSUserDefault
---

### AnyObject
**AnyObject**는 Objective-C와의 호환을 위해 사용되곤 했다.<br>
지금도 그렇게 흔히 쓰여 지기는 한다.<br>

그 전에 Objective-C에서 **id**라고 하는 매우 중요한 타입에 대해서 짚고 가야한다.<br>
id는 모르는 클래스(어떤 타입인지 모르는)의 객체에 대한 포인터 주소를 뜻하는데<br>
뭐든 될 수 있는, 매우 열려있는 타입을 뜻한다는 것이다.<br>

하지만 스위프트에서는 그렇게 하지 않는다.<br>
스위프트에서는 타입을 분명하게 지정해주어야 하는데<br>
Objective-C에서 사용하는 iOS의 모든 API를 사용해야하기 때문에<br>
**AnyObject**라고 하는 타입이 도입되었다.<br>

스위프트에서 **AnyObject**도 Objective-C에서의 **id**와 같이<br>
모르는 클래스(어떤 타입인지 모르는)의 객체에 대한 포인터 주소를 뜻한다.<br>
참고로 **AnyObject**는 클래스에 대한 객체에서만 사용되고<br>
구조체에는 사용되지 않는다.<br>

그래서 객체에 대한 포인터를 갖고 싶은데 그게 무슨 타입인지 모르거나<br>
다른 사람이 무슨 타입인지 알지 못하게 하고 싶을 때 사용한다.<br>
<br>

언제 **AnyObject**가 쓰이게 되는지 확인할 필요가 있다.<br>

메소드 인자의 타입이 적어도 두개 이상일 때<br>
예를 들어, 하나의 뷰컨트롤러부터 다른 뷰컨트롤러로 화면을 전환을 준비하는<br>
prepareForSegue 라는 함수가 있는데<br>
이 메소드의 sender(전환이 실행되는 객체)는 수많은 종류의 타입이 될 수 있다.<br>
클릭할 수 있는 버튼이 되거나 테이블의 한 줄이 될 수도 있고<br>
컨트롤러 안에 커스텀한 코드가 될지도 모른다.<br>

그래서 prepareForSegue의 sender는 **AnyObject**가 되어야만 한다.<br>
이게 버튼일지 테이블의 한 줄일지 뭔지 모르니까...<br>
어떤 객체가 많은 타입이 될 수 있어서 무슨 타입이 될 지 모를 때<br>
이걸 **AnyObject**라고 타입을 지정해준다.<br>

그럼 어떻게 **AnyObject** 타입을 제대로 사용할 수 있을까?<br>
이게 무슨 타입이 될지 모르기 때문에 아는 타입으로 변환을 해줘야 한다.<br>
근데 변환하려고 할 때 그 타입이 아닐 수도 있기 때문에<br>
변환하는 게 가능하지 않을 수도 있다.<br>

그래서 이 상황에 대비하기 위해 스위프트에서는 **as** 키워드를 사용한다.<br>
이 키워드는 다른 타입으로 변환하는 걸 **'시도'** 하는 표현이다.<br>
다시 말해서 AnyObject를 어떤 타입**'으로서(as)'** 대해보겠다는거다.<br>

이 키워드는 선택적으로 대하기 때문에 옵셔널을 반환할 경우가 있다.<br>
그래서 **as**를 if let 구문을 통해서 사용한다.<br>

```
let ao: Anyobject = ...
if let foo = ao as? SomeClass {
    // we can use foo and know that it is of type SomeClass in here
}
```

이 예제 코드는
ao 라는 AnyObject 타입의 지역변수가 하나 있는데<br>
어떤 클래스로 할당이 될지는 알 방법이 없다.<br>
하지만 만약 ao가 SomeClass라면 사용하려고한다.<br>
그래서 ao는 SomeClass 일지도 모르니 SomeClass가 맞는지부터 확인하고<br>
만약 맞다면 foo에 SomeClass인 ao를 저장해서 사용하는 예제 코드이다.<br>

이게 조건에 맞다면 **AnyObject**를 특정한 클래스로 **'캐스팅'** 하는 방법이다.<br>

---

### Property List
**AnyObject**를 사용하는 또 다른 곳은 **Property List** 에서 사용한다.<br>
**Property List**는 기본적으로<br>
Array, Dictionary, String, Double, Int, NSData, NSDate의 조합으로 되어는데<br>
이 클래스들로 데이터 구조를 만들면 **Property List**가 생긴 것이다.<br>
그러니까 **Property List**는 이건 그냥 데이터 구조를 표현하기 위한 단어일 뿐이다.<br>
그런데 이 때 의문이 생길 수 있다.<br>
**AnyObject**는 구조체가 아니라 클래스로만 캐스팅이 되야만 하는데<br>
구조체인 String, Array, Dictionary, Double, Int 이것들이<br>
어떻게 AnyObject가 되어서 **Property List**에 들어갈 수 있는지 궁금해 할 수 있다.<br>

이거에 대한 해답은 **'브리징'**이다.<br>
이것들이 Objective-C로 자동으로 **브리징**되고<br>
Objective-C 클래스인 NSDictionary, NSArray, NSNumber로 다뤄져서<br>
AnyObject가 되도록 변경되는 것이다.<br>
**Property List**는 보이지 않게 전달이 된다.<br>
그 안을 본다고 해도 Dictionary, Array, String 같은게<br>
있다는 것만 알고 데이터가 무엇을 의미하는지는 아무것도 모른다.<br>
그냥 전달되기만 하는거야 이해하기 쉽게 **Property List**를 사용하는 iOS API를 보면 도움이 된다.<br>

---

## NSUserDefault
**NSUserDefault** 라는게 있다.<br>
**NSUserDefault**가 하는 일은 **Property List**를 받아서<br>
디스크에 영구적으로 기억되게 만든다.<br>
그래서 앱을 종료했다가 다시 켜서 봐도 그대로 있을 수 있다.<br>
그러니까 기본적으로 **Property List**의 데이터 베이스인 셈인 것이다.<br>
영어사전같이 영어 전체를 저정해야하는 큰 데이터에 사용하면 안되고<br>
'설정'같은 작은 것에만 사용해야 한다.<br>
제공되는 API로 간단하게 표현되는데<br>

```
setObject(AnyObject, forKey: String)
```

setObject로 **Property List**를 저장하고 다시 가져올 수도 있다.<br>
**NSUserDefault**는 그 안에 Dictionary, Array, String이<br>
있다는 것만 알지 구체적으로 무슨 데이터가 들었는지는 모른다.<br>
그럼 어떻게 **NSUserDefault**를 사용할까?<br>
클래스 메소드 혹은 타입 메소드를 공유해서 사용할 것을 새로 만들어서 사용한다.<br>
**NSUserDefault.standardUserDefualt()**라고 치면<br>
**standardUserDefault**의 공유되는 인스턴스를 만들 수 있다.<br>
그리고 특정한 **Property List**를 가져오고<br>
데이터를 수정하게 되면 자동으로 저장이 가능하다.

<br>
이 포스팅은 **[스탠포드 iOS 강의](https://www.inflearn.com/course/stanford-ios-한글자막-강의/)** 영상을
참고로 하여 작성된 포스팅입니다.