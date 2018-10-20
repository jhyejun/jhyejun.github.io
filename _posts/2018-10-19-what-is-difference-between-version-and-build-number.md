---
title: "[Xcode] 버전과 빌드 번호의 차이란?"
layout: post
hidden: false
date: 2018-10-19 23:52
tag:
- Xcode
- Version
- Build Number
star: true
category: blog
author: jhyejun
description: what is the difference between version and build number
---

오늘은 앱을 출시할 때 중요한 설정인 **버전**과 **빌드 번호**를 알아봅시다.<br>

![Xcode Begin Setting Image](/assets/images/blog/what-is-difference-between-version-and-build-number/xcode_begin_setting.png){: width="70%" height="50%"}

Xcode 프로젝트를 새로 만들게 되면 이미지와 같이 **버전**과 **빌드 번호**가 세팅되어있습니다.<br>
**버전**과 **빌드 번호**를 어떻게 적용하는지 **제가 생각하는 기준에서 정리**하겠습니다.<br>

![Xcode Current Setting Image](/assets/images/blog/what-is-difference-between-version-and-build-number/xcode_current_setting.png){: width="70%" height="50%"}

현재 제가 제 개인 프로젝트에 적용되어 있는 **버전**과 **빌드 번호** 입니다.<br>

---

일단 **버전은 1.0.0** 으로 되어있는데<br>
**첫번째 자리의 1**은 **메이저한 업데이트**가 변경될 때 숫자가 올라갑니다.<br>
쉽게 말하면 게임에서 대규모 패치 같은 경우에서 올라간다고 생각하면 될 것 같습니다.<br>

**두번째 자리의 0**은 메이저한 업데이트는 아니지만<br>
좀 규모가 있는 업데이트 일 때 숫자가 올라갑니다.<br>
**기능같은 것들 리뉴얼하는 업데이트**일 때 숫자가 올라갑니다.<br>

**마지막 자리의 0**은 **자잘한 수정이나, 버그 수정**같은 경우에 숫자가 올라갑니다.<br>
별 거 아닌데, 업데이트를 해야할 때 올린다고 생각하면 될 것 같습니다.<br>

---

**빌드 번호**는 개인 취향에 따라 달라지는데<br>
제가 적용하는 방식을 설명드리겠습니다.<br>
제가 적용하는 방식은 **빌드 번호**를 두개의 파트로 나눠서 이해할 수 있습니다.<br>
**앞에 8자리는 빌드를 마지막으로 돌렸던 날짜**고<br>
그 뒤에 나오는 나머지 숫자들은 **빌드를 마지막으로 돌렸던 날짜 때 빌드 돌렸던 횟수**이다.<br>

예시 이미지에 나오는 빌드 번호인 **201810191**를 예로 들어서 설명하면,<br>
**앞 8자리인 20181019**는 빌드를 마지막으로 돌렸던 날짜가 2018년 10월 19일인거고<br>
**8 자리 뒤에 나온 숫자 1**은 2018년 10월 19일에 빌드 돌린 횟수가 1번이라는 뜻이다.<br>

**빌드 번호는 개발하는 개발자가 프로그램을 트래킹**하기 위해서 사용하는 것이기 때문에<br>
개인 취향 껏 잘 구분될 수 있게 하면 될 것 같습니다.<br>

**그럼 오늘은 여기서 끄읕**