---
title: "[Swift] Method, Property"
layout: post
hidden: false
date: 2018-03-27 23:52
image: "/assets/images/markdown.jpg"
tag:
- Swift
star: true
category: blog
author: jhyejun
description: explain Method, Property
---

### 메소드 (Method)
메소드 정의할 때 파라미터를 외부 이름과 내부 이름으로 구분해 정의할 수 있다.<br>
순서는 외부 이름이 먼저 오고 내부 이름이 그 뒤에 온다.<br>
내부 이름은 메소드 내에서 사용될 지역 변수 이름이고,<br>
외부 이름은 메소드를 호출할 때 사용한다.<br> 

```
func foo(externalFirst first: Int, externalSecond second: Double) {
    var sum = 0.0
    for _ in 0..<first { sum += second }
}

func bar() {
    let result = foo(externalFirst: 100, externalSecond: 5.0)
}
```

외부 이름을 전혀 사용하고 싶지 않다면 외부이름으로 '_' 을 사용하면 된다.

```
func foo(_ first: Int, externalSecond second: Double) {
    var sum = 0.0
    for _ in 0..<first { sum += second }
}

func bar() {
    let result = foo(100, externalSecond: 5.0)
}
```

첫번째 파라미터에는 외부 이름에 언더바가 기본으로 설정되어 있어서<br>
함수를 새로 만들 때 언더바를 넣어줄 필요가 없다.<br>
<br>

첫번째 파라미터를 제외한 다른 파라미터들은 외부 이름 기본값이 내부 이름이라서<br>
외부 이름을 지정하지 않으면 기본값인 내부 이름으로 지정 된다.<br>
두번째 파라미터부터 외부 이름을 지울 수 있고,<br>
첫번째 파라미터의 외부 이름을 추가해 줄 수 있지만<br>
이는 스위프트에 반하는 표현이다.<br>
첫번째 파리미터 이후로 이어지는 파라미터들을 없애는 건 정말 좋지 않다.<br>
<br>

메소드는 오버라이드를 할 수 있다.<br>
오버라이드는 상위 클래스에서 정의된 메소드와 프로퍼티를 다시 정의한다는 걸 뜻한다.<br>
쉽게 말하면 **덮어쓰기**다.<br>
사용하는 방법은 var 나 func 앞에 'override'를 키워드를 달아주면 된다.<br>
<br>

또한 프로퍼티와 메소드는 final로 표시할 수 있다.<br>
final 키워드는 아무도 서브클래싱을 하지 못하게 하는 것인데<br>
**서브클래싱**은 하위 클래스에서 오버라이드 등을 통해 수정하는 걸 뜻한다.<br>
따라서 서브클래싱을 못하게 되면 오버라이드도 할 수 없다.<br>
범위를 넓혀서 클래스 전체에 final을 붙이면 클래스 전체를 서브클래싱 할 수 없게 된다.<br>

---

### 프로퍼티 (Property)

프로퍼티는 스위프트에서 정말 유용하다.<br>
유용한 이유 중 하나는 프로퍼티의 변화를 알 수 있기 때문이다.

```
var someStoredProperty: Int = 42 {
    willSet { newValue is the new value } // 설정되기 전 호출
    didSet { oldValue is the old value } // 설정된 후 호출
}

override var inheritedProperty {
    willSet { newValue is the new value } // 설정되기 전 호출
    didSet { oldValue is the old value } // 설정된 후 호출
}
```

위의 코드는 계산 프로퍼티가 아니라 저장 프로퍼티 코드이다.<br>
willSet은 프로퍼티라 설정되기 전에 호출되고<br>
didSet은 프로퍼티가 설정된 후에 호출된다.<br>

willSet의 newValue는 willSet에서 새로 설정될 값이 들어가고<br>
didSet의 oldValue는 didSet이 실행되면 전에 갖고 있던 값이 들어온다.<br>

저장 프로퍼티나 상속된 프로퍼티에서 사용할 수 있고<br>
계산 프로퍼티에서도 할 수 있다.<br>

willSet과 didSet으로 많이 사용하는 것은 UI를 업데이트 하는 것이다.<br>
컨트롤러에 메소드가 있고 UI가 보이는 방식을 바꿀 컨트롤러에 어떤 값을 설정하면<br>
뷰에게 스스로 다시 그려내라고 요청할 수 있디.<br>
<br>

**lazy initialization(늦은 초기화)**<br>
이건 스위프트에서 약간 속이는 코드표현인데<br>
var가 늦게 초기화되도록 선언할 수 있다.<br>

```
// nice if CaculatorBrain used lots of resources
lazy var brain = CaculatorBrain()

lazy var someProperty: Type = {
    // construct the value of someProperty here
    return <the constructed value>
}()

lazy var myProperty = self.initializeMyProperty()
```

그 말은 변수의 값(CaculatorBrain)을<br>
누군가가 변수에게 요청하기 전까진 할당되지 않는다.<br>
누가 그 변수의 프로퍼티나 메소드를 사용하려 하면<br>
그 때가 되어서야 초기화 되는 것이다.<br>
**음식점으로 치면 주문이 들어와야 조리를 시작하는 것**과 같다.<br>
<br>

세번째 코드(myProperty)는<br>
내 프로퍼티를 직접 메소드를 호출해서 초기화 하려는 건데<br>
보통 lazy 키워드 없이는 잘못된 코드이다<br>

여기에서 lazy가 없으면 안되는 이유는<br>
전체적으로 초기화되기 전까지는 자신에게 메시지를 보내거나<br>
모든 프로퍼티에 접근할 수 없기 때문이다.<br>
하지만 lazy를 통해서는 할 수 있다.<br>
lazy에 대해 설명했다시피<br>
모두 초기화가 되고 난 뒤 내 프로퍼티에 접근하기 전까지는<br>
실제로 실행되지 않을 거이기 때문에<br>
lazy를 통해서는 세번째 코드가 작동한다.<br>
<br>

이게 속이는 코드라고 하는 이유는<br>
lazy가 늦게라도 초기화된다고 해도<br>
모든 변수는 초기화돼야한다는 규칙을 따라 초기화되고 있는 것이기 떄문<br>
lazy는 자주 초기화 함수를 작성하지 않아도 되도록 해주고<br>
init 메소드가 필요하지 않을 수도 있다.<br>
늦게라도 초기화를 할 테니까<br>

lazy는 var만 사용할 수 있다.<br>
lazy는 변수간 의존성에서 생기는 문제를 극복하는 데에도 쓰일 수 있다.<br>
다른 변수에 의존하고 있는 변수 하나가 있다면<br>
둘 중 하나를 lazy로 만들어서 의존성에서 생기는 문제를 극복할 수 있다.<br>
lazy로 만든 변수가 미리 초기화된 변수를 부를거기 때문이다.<br>

<br>
이 포스팅은 **[스탠포드 iOS 강의](https://www.inflearn.com/course/stanford-ios-한글자막-강의/){:target="_blank"}** 영상을
참고로 하여 작성된 포스팅입니다.