---
title: "[Xcode] 날짜와 카운트를 이용한 빌드 번호 스크립트 2"
layout: post
hidden: false
date: 2019-04-05 03:18
tag:
- Xcode
- Build Number
- Script
star: true
category: blog
author: jhyejun
description: build number auto-modification script using date and count 2
---

이전 글 **[[버전과 빌드 번호의 차이란?]]({{ site.url }}/blog/build-number-script-using-date-and-count){:target="_blank"}** 에 있던<br>
**빌드 번호를 자동으로 수정해주는 스크립트**를 만들었었다.<br>

그런데 스크립트를 사용하다보니 이슈가 하나 생겼다.<br>
이슈는 카운트가 일의 자릿수에서 십의 자릿수로 넘어갈 때<br>
카운트 하나만 증가했지만, 빌드번호가 이전꺼보다는 숫자상으로는 크게 되었다.<br>
(예를 들어 201904059 보다 2019040510 이 숫자가 크다)<br>
**앱 빌드를 아카이브할 때 이전에 빌드했던 버전과 같은 버전일 때**<br>
**빌드번호를 비교해서 빌드번호가 이전 것보단 커야 아카이브를 할 수 있다.**<br>

이런 이슈로 인해 스크립트를 수정했다.<br>

```
#!/bin/bash
buildDay=$(/usr/libexec/PlistBuddy -c "Print buildDay" "$INFOPLIST_FILE")
buildCount=$(/usr/libexec/PlistBuddy -c "Print buildCount" "$INFOPLIST_FILE")
today=$(date +%Y%m%d)

if [ x$buildDay == x ]; then
buildDay=${today}
buildCount=1
printf -v zeroPadBuildCount "%03d" $buildCount
buildNumber=${buildDay}${zeroPadBuildCount}

/usr/libexec/PlistBuddy -c "Add :buildDay string $buildDay" "$INFOPLIST_FILE"
/usr/libexec/PlistBuddy -c "Add :buildCount string $buildCount" "$INFOPLIST_FILE"
/usr/libexec/PlistBuddy -c "Set :CFBundleVersion $buildNumber" "$INFOPLIST_FILE"

elif [ $buildDay != $today ]; then
buildDay=${today}
buildCount=1
printf -v zeroPadBuildCount "%03d" $buildCount
buildNumber=${buildDay}${zeroPadBuildCount}

/usr/libexec/PlistBuddy -c "Set :buildDay $buildDay" "$INFOPLIST_FILE"
/usr/libexec/PlistBuddy -c "Set :buildCount $buildCount" "$INFOPLIST_FILE"
/usr/libexec/PlistBuddy -c "Set :CFBundleVersion $buildNumber" "$INFOPLIST_FILE"

else
buildCount=$(($buildCount + 1))
printf -v zeroPadBuildCount "%03d" $buildCount
buildNumber=${buildDay}${zeroPadBuildCount}

/usr/libexec/PlistBuddy -c "Set :buildDay $buildDay" "$INFOPLIST_FILE"
/usr/libexec/PlistBuddy -c "Set :buildCount $buildCount" "$INFOPLIST_FILE"
/usr/libexec/PlistBuddy -c "Set :CFBundleVersion $buildNumber" "$INFOPLIST_FILE"

fi
```

수정된 내용은 카운트가 001 에서 999 까지 가도<br>
빌드번호의 자릿수는 같도록 수정하였다.<br>

<br>
여담이지만...<br>
Xcode 에서만 쓸 때는 빌드번호가 보기에 괜찮다고 생각했는데...<br>
앱스토어에서 빌드번호를 보니 보기가 좀 별로인 거 같다.<br>
계속 쓸지는 진지하게 고려해봐야겠다.<br>
