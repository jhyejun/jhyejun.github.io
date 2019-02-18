---
title: "[알고리즘] 프로그래머스 - 체육복 문제 풀이"
layout: post
hidden: false
date: 2019-02-02 10:51
tag:
- 알고리즘
- 프로그래머스
star: true
category: blog
author: jhyejun
description: algorithm gym suit solution
---

![체육복 문제](/assets/images/blog/algorithm-gym-suit-solution/problem.png){: width="100%" height="70%"}

**가장 먼저 도난 당하지 않은 학생들은 체육 수업을 받을 수 있기에**<br>
수업 받을 수 있는 학생들의 수를 세는 result 변수에 도난 당하지 않은 학생들의 수를 더 헀다.<br>

그 이후 여벌의 체육복을 가져온 친구들을 찾는다.<br>

**여벌의 체육복을 가져온 친구가 자신이 도난 당한거라면**<br>
(여벌의 체육복을 가져온 친구가 도난 당하면 총 두벌에서 한벌만 도난 당함)<br>
여벌의 체육복을 가진 친구들의 배열에서 빼고 result 변수에 1을 더한다.<br>

**혹은 여벌의 체육복을 가져온 친구의 앞번호 혹은 뒷번호 친구가 도난 당한 친구라면**<br>
여벌의 체육복을 가진 친구들의 배열에서 빼고 result 변수에 1을 더한다.<br>

변수명은 아무 생각 안하고 짰으니, 감안하고 봐주시길 바랍니다 ㅎㅎ...<br>
```
import Foundation

func solution(_ n:Int, _ lost:[Int], _ reserve:[Int]) -> Int {
    var result: Int = 0
    var extra: [Int] = reserve
        
    result += n - lost.count
        
    for i in 0 ..< lost.count {
        var has: Int?

        for j in 0 ..< extra.count {
            if lost[i] == extra[j] {
                has = j
                break
            } else if lost[i] - 1 == extra[j] || lost[i] + 1 == extra[j] {
                has = j
                break
            }
        }

        if let index = has {
            extra.remove(at: index)
            result += 1
        }
    }

    return result
}
```
