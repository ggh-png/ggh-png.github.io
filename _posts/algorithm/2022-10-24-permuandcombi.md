---
title: Combination and Permutation
author: ggh-png
date: 2022-10-24 10:10:00 +0800
categories: [algorithm, Combination, Combi, permu]
tags: [algorithm, combination, permutation]
render_with_liquid: false
---
# C++ 순열과 조합

## 개요

---

12명이 서로(2명씩) 인사하면 66가지의 경우에 수가 나오게 된다. 이 경우의 수는 순열과 조합으로 구할 수 있는데 

이번 포스팅에선 이 순열과 조합에 대해 알아 보고, 코드로 구현까지 해보도록 하겠다. 

  

## 순열

---

> 먼저 순열, permutation이란 순서가 정해진 임의의 집합을 다른 순서로 섞는 연산을 말한다.
> 
> 
> 즉 예를들어 { 1, 2, 3 } 의 요소가 있을 때  { 1, 3, 2 } 이런식으로  다른 순서로 섞는것을 연산 순열이라 한다.
> 
> 만약 n 개의 집합 중 n개를 고르는 순열의 개수를 구할 때 중복을 허용한다면 n!이 된다.  
> 위의 예를 들어 보자면 3개의 자연수{ 1, 2, 3 }를 이용해 만들 수 있는 3자리 자연수는 몇개 일까? 
> 
> [ 123,132, 213, 231, 312, 321 ]  6개의 순열이 나오게 된다. 
> 
> 또 만약 3개의 자연수{ 1, 2, 3 }를 이용해 만들 수 있는 1자리 자연수는 3개다.
> 
> 전자는 3개중 3개를 뽑는 것이기에 3!이 되고 후자는 3개중 1개를 뽑는 것이라 3이
> 되게 된다. 
> 
> **nPr = n! / (n - r)!**
> 
> n : 요소의 개수 r : 요소에서 필요로하는 개수 
> 
> 위와 같은 문제는 위 공식에 따라 몇개인지 결정이 됩니다. 예를 들어 3개 중 3개를 뽑는다면 3!/(3 -
> 3)! 이 되고 3개 중 1개를 뽑는다면 3! / (3 - 1)! 이 되는 것이다.
> 
> 이를 코드로 구현을 하면 비트 마스킹, next_permutation과 perv _permutation, 재귀 각각 세가지의 방법이 존재한다.  
> 

### next_permutation, perv_permutation - Permutation

---

```cpp
#include <cstdio>
#include <vector>
#include <algorithm>
#include <iostream>
using namespace std;

void printV(vector<int> &v)
{
	for(int i = 0; i < v.size(); i++)
			cout << v[i] << " ";
	cout << "\n";
}
int main()
{
    int a[3] = {1, 2, 3};
    vector<int> v;
    for(int i = 0; i < 3; i++)
        v.push_back(a[i]);
        //1, 2, 3부터 오름차순으로 순열을 뽑습니다.
    
    do
        printV(v);
    while(next_permutation(v.begin(), v.end()));

    cout << "-------------" << '\n';
    v.clear();
    for(int i = 2; i >= 0; i--)
        v.push_back(a[i]);
        //3, 2, 1부터 내림차순으로 순열을 뽑습니다.
    do
        printV(v);
    while(prev_permutation(v.begin(), v.end()));
    
    return 0;
}
```

#### 출력 

---

```
1 2 3
1 3 2
2 1 3
2 3 1
3 1 2
3 2 1
-------------
3 2 1
3 1 2
2 3 1
2 1 3
1 3 2
1 2 3
```

#### 사용법 

---

- 오름차순

```cpp
   do
        printV(v);
    while(next_permutation(v.begin(), v.end()));
```

- 내림차순

```cpp
    do
        printV(v);
    while(prev_permutation(v.begin(), v.end()));
```

### 재귀함수 - Permutation

---

```cpp
#include <cstdio>
#include <vector>
#include <algorithm>
#include <iostream>
using namespace std;

int a[3] = {1, 2, 3};
vector<int> v;

void printV(vector<int> &v)
{
    for(int i = 0; i < v.size(); i++)
        cout << v[i] << " ";
    cout << "\n";
}

void makePermutation(int n, int r, int depth)
{
    if(r == depth)
    {   
        printV(v);
        return;
    }
    for(int i = depth; i < n; i++)
    {
        swap(v[i], v[depth]);
        makePermutation(n, r, depth + 1);
        swap(v[i], v[depth]);
    }
    return;
}

int main()
{
    for(int i = 0; i < 3; i++)
        v.push_back(a[i]);
    makePermutation(3, 3, 0);
    return 0;
}
```

#### 출력 

---

```
1 2 3
1 3 2
2 1 3
2 3 1
3 1 2
3 2 1
```

### 재귀 - Permutation sol

---

- 순열을 만들고, 다시 돌려 놓는다.
    - 완전탐색 이용

```cpp
 void makePermutation(int n, int r, int depth)
{
    if(r == depth)
    {   
        printV(v);
        return;
    }
    for(int i = depth; i < n; i++)
    {
        swap(v[i], v[depth]);
        makePermutation(n, r, depth + 1);
        swap(v[i], v[depth]);
    }
    return;
}
```

## 조합 - Combination

---

> 순열, permutation이 순서가 정해진 임의의 집합을 다른 순서로 섞는 연산이라면, 조합 Combination 은 순서가 정해지지 않은 집합들의 모음을 뜻한다.
> 
> 
> **nCr = n! / r!(n-r)!**     
> 
> n : 요소의 개수 r : 요소에서 필요로하는 개수  
> 
> 공식은 위와 같으며 예를들어 서로 다른 5개의 구슬 중 3개의 구슬을 뽑는 상황에서의 조합의 결과는 다음과 같다. 
> 
> 5C3 = 5! / 3!(5 - 3)! = 10 
> 

### 재귀함수 - Combination

---

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// 서로다른 5개의 구슬이 들어있는 구슬망에서 3개의 구슬을
// 뽑는 조합 구현 
// nCr = 5C3

int n = 5;
int r = 3;
int arr[] = {1, 2, 3, 4, 5};

vector<int> v;

void CombiPrint(vector<int> v)
{
    for(auto &el : v)
        cout << el << " ";
    cout << endl;
}

void combi(int start, vector<int> v)
{
    // 3개의 구슬이 뽑아지면
    if(v.size() == r)
    {
        CombiPrint(v);
        return;
    }
    for(int i=start+1; i < n; i++)
    {
        v.push_back(arr[i]);
        combi(i, v);
        v.pop_back();
    }
    return;
}

int main()
{
    combi(-1, v);
    return 0;
}
```

#### 출력 

---

```
1 2 3 
1 2 4 
1 2 5 
1 3 4 
1 3 5 
1 4 5 
2 3 4 
2 3 5 
2 4 5 
3 4 5
```

### 재귀 - Combination 완전 탐색

---

```cpp
void combi(int start, vector<int> v)
{
    // 3개의 구슬이 뽑아지면
    if(v.size() == r)
    {
        CombiPrint(v);
        return;
    }
    for(int i=start+1; i < n; i++)
    {
        v.push_back(arr[i]);
        combi(i, v);
        v.pop_back();
    }
    return;
}
```

벡터에 요소를 넣고 3개의 구슬을 뽑게되면 리턴하여 함수를 종료 하게 한다.

1차적으로 종료된 함수는 처음으로 뽑은 구슬을 다시 빼게 되면서 완전탐색을 할 수 있도록 구현 한다. 

### 다중 for문 - r 값이 작을 경우 (3이하)

---

```cpp
#include <iostream>

using namespace std;

int main()
{
    int a[] = {1, 2, 3, 4, 5, 6};
    for(int i = 0; i < 6; i++)
        for(int j = 0; j < i; j++)
            for(int k = 0; k < j; k++)
                cout << a[i] << " " << a[j] << " " << a[k] << endl; 
    return 0;
}
```

#### SOL

---

r 값 즉, 집합에서 요소를 뽑아야 되는 개수가 3이하로 작다면 위와 같이 다중 for문으로 구할 수 있다. 

다중 for문의 장점은 재귀를 이용한 조합을 구현하는 것 보다 더 쉽게 구현 할 수 있다는 장점이 있다.

 

### **Reference**

---

[큰돌의 터전 : 네이버 블로그](https://blog.naver.com/jhc9639/221440212889)