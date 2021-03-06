---
title: 프로그래머스 - level_2 - 가장 큰 수
author: ggh-png
date: 2022-03-27 10:30:00 +0800
categories: [PS, 프로그래머스]
tags: [C++, PS, 프로그래머스, 정렬]
render_with_liquid: false
---

# 프로그래머스 - level_2 - 가장 큰 수

### 문제

---

[코딩테스트 연습 - 가장 큰 수](https://programmers.co.kr/learn/courses/30/lessons/42746)

![가장 큰 수](https://user-images.githubusercontent.com/71277820/160277493-d8b13dd0-731e-4daf-9e06-7025e523c680.png)

#### SOL

---

- to_string으로 변환한 두 정수를 합한 값이 큰 값을 기준으로 정렬하기
    - ex) sum (”3”, “11”) ~> 311
    - sum (”11", “3”) ~> 113

### 구현

---

```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>
using namespace std;
// 앞자리 기준으로 
// 내림차순 정렬 
// 문자열에 저장 

bool compare(int a, int b)
{ 
    string tmp_1 = to_string(a) + to_string(b);
    string tmp_2 = to_string(b) + to_string(a);
    return  tmp_1 > tmp_2;
}
string solution(vector<int> numbers) {
    string answer = "";
    sort(numbers.begin(), numbers.end(), compare);
    for(auto el : numbers)
        answer += to_string(el);
    if(answer[0] == '0')
        answer = "0";
    return answer;
}
```

### Reference

---

[C++ STL sort ( )](https://ggh-png.github.io/posts/cpp-stl-sort/)