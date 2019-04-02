---
title: "[Xcode] 날짜와 카운트를 이용한 빌드 번호 스크립트"
layout: post
hidden: false
date: 2018-10-20 22:39
tag:
- Xcode
- Build Number
- Script
star: true
category: blog
author: jhyejun
description: build number auto-modification script using date and count
---

이전 글 [[버전과 빌드 번호의 차이란?]]({{ site.url }}/blog/what-is-difference-between-version-and-build-number){:target="_blank"} 에서 올렸던<br>
**빌드 번호를 자동으로 수정해주는 스크립트**를 만들었다.<br>
사용법은 **[Xcode Build Phases] -> [Run Script]** 에 코드를 넣으면<br>
**빌드할 때마다 빌드 번호가 자동으로 수정**된다.<br>

```
#!/bin/bash
buildDay=$(/usr/libexec/PlistBuddy -c "Print buildDay" "$INFOPLIST_FILE")
buildCount=$(/usr/libexec/PlistBuddy -c "Print buildCount" "$INFOPLIST_FILE")
today=$(date +%Y%m%d)

if [ x$buildDay == x ]; then
    buildDay=${today}
    buildCount=1
    buildNumber=${buildDay}${buildCount}

    /usr/libexec/PlistBuddy -c "Add :buildDay string $buildDay" "$INFOPLIST_FILE"
    /usr/libexec/PlistBuddy -c "Add :buildCount string $buildCount" "$INFOPLIST_FILE"
    /usr/libexec/PlistBuddy -c "Set :CFBundleVersion $buildNumber" "$INFOPLIST_FILE"

elif [ $buildDay != $today ]; then
    buildDay=${today}
    buildCount=1
    buildNumber=${buildDay}${buildCount}

    /usr/libexec/PlistBuddy -c "Set :buildDay $buildDay" "$INFOPLIST_FILE"
    /usr/libexec/PlistBuddy -c "Set :buildCount $buildCount" "$INFOPLIST_FILE"
    /usr/libexec/PlistBuddy -c "Set :CFBundleVersion $buildNumber" "$INFOPLIST_FILE"

else
    buildCount=$(($buildCount + 1))
    buildNumber=${buildDay}${buildCount}

    /usr/libexec/PlistBuddy -c "Set :buildDay $buildDay" "$INFOPLIST_FILE"
    /usr/libexec/PlistBuddy -c "Set :buildCount $buildCount" "$INFOPLIST_FILE"
    /usr/libexec/PlistBuddy -c "Set :CFBundleVersion $buildNumber" "$INFOPLIST_FILE"

fi
```

이 스크립트는 info.plist 에서 buildDay 와 buildCount 항목을 가져온다.<br>

첫번째는 **buildDay 항목이 없거나 값이 없을 경우**<br>
**buildDay 항목은 오늘 날짜로 설정하고, buildCount 는 0 으로 설정한 후 항목을 추가**한다.<br>
**빌드 번호는 (buildDay)(buildCount) 를 합쳐서 빌드 번호를 적용**한다.<br>

두번째는 **buildDay 항목의 값이 오늘 날짜 값과 다를 경우**<br>
**buildDay 항목을 오늘 날짜 값으로 설정하고, buildCount 는 1을 증가시킨 후 적용**한다.<br>
**이번에도 빌드 번호는 (buildDay)(buildCount) 를 합쳐서 빌드 번호를 적용**한다.<br>

세번째는 두 가지 경우에 해당 안되는 경우인데<br>
**사실상 buildDay 항목의 값은 오늘 날짜일 경우이다.**<br>
**buildCount 만 1을 증가시킨 후 적용**한다.<br>
**이번에도 빌드 번호는 (buildDay)(buildCount) 를 합쳐서 빌드 번호를 적용**한다.<br>

만약 10월 20일에 빌드를 10번 돌렸다하면 빌드 번호는 **2018102010** 으로 세팅된다.<br>


이걸 자동화하기 위해서 이 스크립트를 만들었다는게 기분이 좋고<br>
처음으로 쉘스크립트로 코드를 짜서 시간이 좀 소요되긴 했지만<br>
만들고 나니 확실히 편해진 것 같다.<br>
