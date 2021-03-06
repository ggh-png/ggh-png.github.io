---
title: 프로그래머스 - level_2 - H-index
author: ggh-png
date: 2022-03-27 10:40:00 +0800
categories: [PS, 프로그래머스]
tags: [C++, PS, 프로그래머스, 정렬]
render_with_liquid: false
---

# 프로그래머스 - level_2 - H-index

### 문제

---

[코딩테스트 연습 - H-Index](https://programmers.co.kr/learn/courses/30/lessons/42747)

![hindex](https://user-images.githubusercontent.com/71277820/160284670-7eff394f-0a29-473c-bc54-46307933ffac.png)

#### SOL

---

- 내림차순 정렬
    - ex) [ 3,1,4,1,5]
    - [ 5,3,2,1,1 ]
- 인자 값과 인덱스 값이 엇갈리는 지점 탐색
    - 5 ~> 1
    - 3 ~> 2 - H index
    - 2 ~> 3 - 엇갈림
- 반례
    - ex) [ 12, 9, 9, 9 ] - 동일한 수가 연속적으로 나오거나, 인덱스 값이 엇갈리지 않는경우
        - H - index : citations.size()

### 구현

---

```cpp
#include <string>
#include <vector>
#include <iostream>
#include <algorithm>

using namespace std;
// H index를 구하는법 
// 아마 어떤 요소를 기준으로 요소 이상의 값의 개수가 최댓값이 될 수 있는 기준요소를 구하는거겠지? 

int solution(vector<int> citations) 
{
    int answer = 0;
    int i;
    sort(citations.rbegin(), citations.rend());

    for(i=0; i < citations.size(); i++)
    {   
        if(citations[i] < i+1)
            return answer = i;
        
        else if(citations[i] == i+1)
            return answer = i+1;     
    }
            
    answer = i;    
    return answer;
}
```

### Reference

---

[Guides: 1-6. h-Index: Home](https://hanyang-kr.libguides.com/c.php?g=717952)