---
title: 프로그래머스 - level_2 - 기능개발
author: ggh-png
date: 2022-04-19 10:30:00 +0800
categories: [PS, 프로그래머스]
tags: [C++, PS, 프로그래머스, 정렬]
render_with_liquid: false
sitemap :
  changefreq : daily
  priority : 1.0
---

# 프로그래머스 - level_2 - 기능개발

### 문제

---

[코딩테스트 연습 - 기능개발](https://programmers.co.kr/learn/courses/30/lessons/42586#)

![https://user-images.githubusercontent.com/71277820/164022839-ce5fa0e9-d7e8-40d1-93bf-67c5e546d63d.png](https://user-images.githubusercontent.com/71277820/164022839-ce5fa0e9-d7e8-40d1-93bf-67c5e546d63d.png)

### 문제 개념

---

- 프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.
    - progresses [ 95, 90, 99, 99, 80, 99 ] - 진도
    - speeds [ 1,  1,  1,  1,  1,  1 ] - 개발속도
        - progresses[0] = 95, speed[0] = 1 일때
            - 서비스 반영까지 5일 소요.
- 또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.
    - 서비스 반영 소요일을 day라고 할 때
    - day[ 5, 10, 1, 1, 20, 1 ] 이며 앞에 있는 기능보다 먼저 개발될 경우 함께 배포되어
        - 소요일은 [ 5, 10, 20 ]일이 걸린다.
- 먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.
    - 따라서 소요일당 배포된 기능의 개수를 구하면
        - 5일째 - 1개 배포
        - 10일째 - 3개 배포
        - 20일재 - 2개 배포
            - 즉 answer[ 1, 3, 2]인 것이다.

### 구현

---

```cpp
#include <string>
#include <vector>
#include <stack>
#include <iostream>

using namespace std;

vector<int> solution(vector<int> progresses, vector<int> speeds) 
{
    vector<int> day(progresses.size(), 0);
    vector<int> answer;
    stack<int> s;
    // 소요일 
    for(int i=0; i < progresses.size(); i++)
    {   // 작업이 완료가 될 때 까지
        int days=0;
        while (!(progresses[i] >= 100))
        {
            progresses[i] += speeds[i];
            day[i]++;
        }
    }

    int tmp=0;
    int max = day[0];
    for(int i=0; i < day.size(); i++)
    {   // stack이 비어있거나 과거 >= 현재일 경우 
        if(s.empty() || max >= day[i])
        {   // stack에 push
            s.push(day[i]);
            // 마지막일 경우 pop and answer push
            if(i == day.size()-1)
            {
                while (!s.empty())
                {
                    s.pop();
                    tmp++;
                }
                answer.push_back(tmp);
            }
        }  
        // stack이 비어있지 않고, 과거 < 현재일 경우 
        else
        {
            // maxDay init
            max = day[i]; 
            // stack pop
            while (!s.empty())
            {
                s.pop();
                tmp++;
            }
            answer.push_back(tmp);
            // 현재 값 push 
            s.push(day[i]);
            // tmp zero init
            tmp = 0;
            // 마지막일 경우 pop and answer push
            if(i == day.size()-1)
            {
                while (!s.empty())
                {
                    s.pop();
                    tmp++;
                }
                answer.push_back(tmp);
            }
        }
    }
    return answer;
}
```

#### SOL

---

- 소요일 탐색
    
    ```cpp
    for(int i=0; i < progresses.size(); i++)
    {   // 작업이 완료가 될 때 까지
        int days=0;
        while (!(progresses[i] >= 100))
        {
            progresses[i] += speeds[i];
            day[i]++;
        }
    }
    ```
    
- 앞에 있는 기능보다 먼저 개발될 경우 함께 배포.
    
    ```cpp
    if(s.empty() || max >= day[i])
    {   // stack에 push
        s.push(day[i]);
        // 마지막일 경우 pop and answer push
        if(i == day.size()-1)
        {
            while (!s.empty())
            {
                s.pop();
                tmp++;
            }
            answer.push_back(tmp);
        }
    }
    ```
    
- 반례
    
    ```cpp
    else
    {
        // maxDay init
        max = day[i]; 
        // stack pop
        while (!s.empty())
        {
            s.pop();
            tmp++;
        }
        answer.push_back(tmp);
        // 현재 값 push 
        s.push(day[i]);
        // tmp zero init
        tmp = 0;
        // 마지막일 경우 pop and answer push
        if(i == day.size()-1)
        {
            while (!s.empty())
            {
                s.pop();
                tmp++;
            }
            answer.push_back(tmp);
        }
    }
    ```
    

### 구현 - 짤막코드..

---

```cpp
#include <string>
#include <vector>
#include <iostream>
using namespace std;

vector<int> solution(vector<int> progresses, vector<int> speeds) {
    vector<int> answer;

    int day;
    int max_day = 0;
    for (int i = 0; i < progresses.size(); ++i)
    {
        day = (99 - progresses[i]) / speeds[i] + 1;

        if (answer.empty() || max_day < day)
            answer.push_back(1);
				// 앞에 있는 기능보다 먼저 개발될 경우 함께 배포.
        else
            ++answer.back();

        if (max_day < day)
            max_day = day;
    }

    return answer;
}
```

### Reference & 다른 PS 모음집

---

[https://github.com/ggh-png/PS](https://github.com/ggh-png/PS)

[STACK & QUEUE](https://ggh-png.github.io/posts/queue&stack/)