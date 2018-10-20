---
title: "[Xcode] 날짜와 카운트를 이용한 빌드 번호 스크립트"
layout: post
hidden: true
date: 2018-10-19 22:39
tag:
- Xcode
- Build Number
- Script
star: true
category: blog
author: jhyejun
description: build number auto-modification script using date and count
---



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