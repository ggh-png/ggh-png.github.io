---
title: STACK & QUEUE
author: ggh-png
date: 2022-04-07 10:10:00 +0800
categories: [algorithm, 자료구조]
tags: [algorithm, stack, queue]
render_with_liquid: false
sitemap :
  changefreq : daily
  priority : 1.0
---

# STACK & QUEUE

### 개yo

---

이번 포스팅에선 컴퓨터 프로그램의 가장 기초가 되는 자료구조인 **스택(stack)과 큐(queue)**에 대해 알아보도록 하겠다. 

 

### 스택(stack)

---

> 스택(stack) 이것만 기억하자 ~> 택배의 상하차 - 선입 후출
> 

스택(stack)을 단순히 표현하자면 택배의 상하차와 같다고 볼 수 있는데 그 이유는 스택의 선입 후출이란 특징 때문이다. 

#### 특징 

1. 선입  
    - 빈 리스트 [  ]에 임의의 요소 값(3, 1, 4, 1, 5, 9, 2) 추가.
        - [ 3 ] .. [ 3, 1 ] .. [ 3, 1, 4] ....... [ 3, 1, 4, 1, 5, 9, 2 ]
2. 후출
    - 요소 값이 들어있는 리스트 [ 3, 1, 4, 1, 5, 9, 2 ]에서 마지막으로 들어온 요소를 1개씩 제거
        - [ 3, 1, 4, 1, 5, 9, 2 ] .. [ 3, 1, 4, 1, 5, 9 ] .. [ 3, 1, 4, 1, 5 ] ...... [ ]

### stack 구현 - stl stack

---

```cpp
#include<iostream>
#include<stack>
using namespace std; 

int main()
{
    stack<int> Stack_List;
    // 선입  
    Stack_List.push(3); 
    Stack_List.push(1); 
    Stack_List.push(4);
    // 후출 
    Stack_List.pop(); 
    Stack_List.push(1); 
    Stack_List.push(5);
    // 후출 
    Stack_List.pop(); 
    Stack_List.push(9); 
    Stack_List.push(2);
    while (!Stack_List.empty())
    {
        cout << Stack_List.top() << " ";
        Stack_List.pop();
    }
    
}
```

### stack 구현 - stl vector

---

```cpp
#include<iostream>
#include<vector>
using namespace std; 

int main()
{
    vector<int> VectorStack;
    // 선입  
    VectorStack.push_back(3); 
    VectorStack.push_back(1); 
    VectorStack.push_back(4);
    // 후출 
    VectorStack.pop_back(); 
    VectorStack.push_back(1); 
    VectorStack.push_back(5);
    // 후출 
    VectorStack.pop_back(); 
    VectorStack.push_back(9); 
    VectorStack.push_back(2);
    // 후출 
    VectorStack.pop_back();
    for(int i = VectorStack.size(); i >= 0; i--)
        cout << VectorStack[i] << " ";
    
}
```

#### 출력 

---

```
2 9 1 1 3
```

### Work Flow

---

1. [ 3, 1, 4 ] push
2. [ 3, 1 ] - [ 4 ] pop
3. [ 3, 1, 1, 5 ] - [ 1, 5 ] push
4. [ 3, 1, 1 ] - [ 5 ] pop
5. [ 3, 1,  1, 9, 2 ] - [ 9, 2 ] push 

### 큐(queue)

---

> 큐(queue) 이것만 기억하자 ~> 은행 창구 - 선입 선출
> 

스택과 다르게 큐(queue)는 선입 후출의 형태가 아닌 은행 창구과 같이 선입 선출의 구조를 띄고 있다. 

#### 특징 

1. 선입  
    - 빈 리스트 [  ]에 임의의 요소 값(3, 1, 4, 1, 5, 9, 2) 추가.
        - [ 3 ] .. [ 3, 1 ] .. [ 3, 1, 4] ....... [ 3, 1, 4, 1, 5, 9, 2 ]
2. 선출
    - 요소 값이 들어있는 리스트 [ 3, 1, 4, 1, 5, 9, 2 ]에서 먼저 들어온 요소를 1개씩 제거
        - [ 1, 4, 1, 5, 9, 2 ] .. [ 4, 1, 5, 9, 2 ] .. [ 1, 5, 9, 2 ] ...... [ ]

### Queue 구현 - stl queue

---

```cpp
#include<iostream>
#include<queue>
using namespace std; 

int main()
{
    queue<int> Queue_List;
    // 선입  
    Queue_List.push(3); 
    Queue_List.push(1); 
    Queue_List.push(4);
    // 선출 
    Queue_List.pop(); 
    Queue_List.push(1); 
    Queue_List.push(5);
    // 선출 
    Queue_List.pop(); 
    Queue_List.push(9); 
    Queue_List.push(2);
    while (!Queue_List.empty())
    {
        cout << Queue_List.front() << " ";
        Queue_List.pop();
    }
    return 0;
}
```

### Queue 구현 - stl vector

---

```cpp
#include<iostream>
#include<vector>
using namespace std; 

int main()
{
    vector<int> VectorQueue;
    // 선입  
    VectorQueue.push_back(3); 
    VectorQueue.push_back(1); 
    VectorQueue.push_back(4);
    // 선출 
    VectorQueue.erase(VectorQueue.begin()); 
    VectorQueue.push_back(1); 
    VectorQueue.push_back(5);
    // 선출 
    VectorQueue.erase(VectorQueue.begin()); 
    VectorQueue.push_back(9); 
    VectorQueue.push_back(2);

    for(auto el : VectorQueue)
        cout << el << " ";
    cout << endl;
    
}
```

#### 출력 

---

```
4 1 5 9 2
```

### Work Flow

---

1. [ 3, 1, 4 ] push
2. [ 1 , 4] - [ 3 ] pop
3. [ 1, 4, 1, 5] - [ 1, 5 ] push
4. [ 4, 1, 5 ] - [ 1 ] pop
5. [ 4, 1, 5, 9, 2 ] - [ 9, 2 ] push 

### **Reference**

---

[14. 큐(Queue)](https://m.blog.naver.com/ndb796/221230944729)