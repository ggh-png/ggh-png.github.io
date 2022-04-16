---
title: C++ STL lower_bound ( ) 
author: ggh-png
date: 2022-04-04 00:00:00 +0800
categories: [C++, STL]
tags: [C++, 정렬, STL]
render_with_liquid: false
sitemap :
  changefreq : daily
  priority : 1.0
---


# C++ STL lower_bound ( )

### 개yo

---

이번 포스팅에선 리스트에서 내가 원하는 key값의 인덱스 값을 구할 수 있는 STL lower_bound ( )에 대해 알아보고 구현도 해보도록 하겠다. 

### Binary Search을 이용하여 lower_bound ( ) 구현

---

> lower_bound ( )는 기본적으로 이진탐색(Binary Search)를 기반으로 만들어졌기에 아래와 같이 구현하였다.
> 

```cpp
#include<iostream>
#include<vector>
using namespace std;

int lower_bound(vector<int> &index, int k, int begin, int end)
{
    if(begin > end) return - 1;
    int m = (begin + end) / 2;
    if(index[m] == k) return m;
    else if(index[m] > k) return lower_bound(index, k, begin, m-1);
    else return lower_bound(index, k, m+1, end);
}

int main(){
    vector<int> arr = {1, 2, 4, 5, 6, 7, 9};

    cout << "lower_bound(6) : " << lower_bound(arr, 6, 0, arr.size())<< endl;
    cout << "lower_bound(4) : " << lower_bound(arr, 4, 0, arr.size())<< endl;
    cout << "lower_bound(7) : " << lower_bound(arr, 7, 0, arr.size())<< endl;
    cout << "lower_bound(9) : " << lower_bound(arr, 9, 0, arr.size())<< endl;
    return 0;
}
```

#### 출력 

```
lower_bound(6) : 4
lower_bound(4) : 2
lower_bound(7) : 5
lower_bound(9) : 6
```

#### 사용법 

---

- lower_bound(검색할 리스트, key, 시작점, 끝점)
    - 반환 값은 리스트의 key이 저장되어 있는 인덱스 값이다.
    

### STL lower_bound ( )

---

> 기본적으로 lower_bound( )는 C++의 STL인 algorithm 헤더의 내잠함수이기에 담음과 같이 선언 후 사용한다.
> 
> - lower_bound( )는 찾으려는 key 값과 같거나 보다 큰 가장 작은 수의 index(주소)값을 찾는다.
>     - 때문에 오름차순으로 정렬된 리스트에서 사용된다.

```cpp
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

int main(){
    vector<int> arr = {1, 2, 4, 5, 6, 7, 9};
 
    cout << "lower_bound(6) : " 
         << lower_bound(arr.begin(), arr.end(), 6) - arr.begin() << endl;

    cout << "lower_bound(4) : " 
         << lower_bound(arr.begin(), arr.end(), 4) - arr.begin() << endl;

    cout << "lower_bound(7) : " 
         << lower_bound(arr.begin(), arr.end(), 7) - arr.begin() << endl;
         
    cout << "lower_bound(9) : " 
         << lower_bound(arr.begin(), arr.end(), 9) - arr.begin() << endl;
    return 0;
}
```

#### 출력 

---

```
lower_bound(6) : 4
lower_bound(4) : 2
lower_bound(7) : 5
lower_bound(9) : 6
```

### lower_bound 사용법

---

- 용도
    - 찾으려는 key 값보다 같거나 큰 수를 찾을 때 사용함.
- 사용 조건
    - 탐색을 진행할 리스트가 오름차순으로 정렬되어 있어야 함.
        - 같거나 큰 수를 찾기 때문.
- lower_bound(리스트의 시작점, 리스트의 끝점, 찾을 key)
    - 반환형은 **Iterator(주소 값)** 이기에 리스트의 첫 번째 주소를 빼면 N번째 인덱스인지 알 수 있다.
    - index = lower_bound(리스트의 시작점, 리스트의 끝점, 찾을 key) - 리스트의 시작점
    

### STL upper_bound ( ) 사용

---

> upper_bound( ) 역시 C++의 STL인 algorithm 헤더의 내잠함수이기에 담음과 같이 선언 후 사용한다.
> 
> - upper_bound( )는 찾으려는 key 값을 초과하는 첫번째 index를 구한다.
>     - 때문에 오름차순으로 정렬된 리스트에서 사용된다.

```cpp
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

int main(){
    vector<int> arr = {1, 2, 4, 5, 6, 7, 9};
 
    cout << "upper_bound(6) : " 
         << upper_bound(arr.begin(), arr.end(), 6) - arr.begin() << endl;

    cout << "upper_bound(4) : " 
         << upper_bound(arr.begin(), arr.end(), 4) - arr.begin() << endl;

    cout << "upper_bound(7) : " 
         << upper_bound(arr.begin(), arr.end(), 7) - arr.begin() << endl;
         
    cout << "upper_bound(9) : " 
         << upper_bound(arr.begin(), arr.end(), 9) - arr.begin() << endl;
    return 0;
}
```

#### 출력 

---

```
upper_bound(6) : 5
upper_bound(4) : 3
upper_bound(7) : 6
upper_bound(9) : 7
```

### upper_bound 사용법

---

- 용도
    - 찾으려는 key 값을 초과하는 최초의 값의 인덱스를 찾기 위해 사용한다.
- 사용 조건
    - 탐색을 진행할 리스트가 오름차순으로 정렬되어 있어야 함.
        - key 값을 초과하는 첫번째 인덱스 값을 찾기 때문.
- upper_bound(리스트의 시작점, 리스트의 끝점, 찾을 key)
    - 반환형은 **Iterator(주소 값)** 이기에 리스트의 첫 번째 주소를 빼면 N번째 인덱스인지 알 수 있다.
    - index = upper_bound(리스트의 시작점, 리스트의 끝점, 찾을 key) - 리스트의 시작점
    

### **Reference**

---

[https://chanhuiseok.github.io/posts/algo-55/](https://chanhuiseok.github.io/posts/algo-55/)

[C++ Lower Bound-STL 사용하는 방법](https://inpages.tistory.com/136)