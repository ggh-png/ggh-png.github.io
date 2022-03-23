---
title: C++ STL sort ( )
author: ggh-png
date: 2022-03-23 10:10:00 +0800
categories: [C++, STL]
tags: [C++, 정렬, STL]
render_with_liquid: false
---

# C++ STL sort ( )

### 개yo

---

지금까지의  포스팅에선 여러가지 정렬 알고리즘에 대해 알아 보았다. 정렬 알고리즘은 CS의 오래된 연구분야로 이미 뛰어난 관련 라이브러리가 각 언어마다 존재 하기에 직접 구현을 할 필요가 없다. 해서 이번 포스팅에선 **C++ STL의 정렬 함수인 sort( )** 에 대해 알아보도록 하겠다.  

 

### STL sort ( ) 사용법 - 두개의 인자만 사용

---

> 기본적으로 sort ( )는 C++의 STL인 algorithm 헤더에 포함되어 있어 아래와 같이 선언 후 사용하면 된다.
> 

```cpp
#include<iostream>
#include<algorithm>

using namespace std;

int main()
{
    int arr[] = {3,1,4,1,5,9,2};
    sort(arr, arr+7);
    for(int i=0; i < 7; i++)
        cout << arr[i] << " ";
    cout << endl;    
}
```

> sort( )는 최소 두개의 인자 값을 받는데 이는 즉 정렬을 할 **리스트(배열)의 메모리 주소**를 뜻한다.
> 
> 
> `sort(시작 메모리 주소,  끝 메모리 주소)` 와 같이 작성해주면 아래와 같이 오름차순으로 배열이 정렬이 된다. 
> 

#### 출력 

```
1 1 2 3 4 5 9
```

### STL sort ( ) 사용법 - 세개의 인자 사용

---

> 위에서도 나타냈듯이 sort( )는 sort(시작점, 끝점) 이렇게 두개의 인자 값을 가지고도 오름차순으로 배열을 정렬할 수 있다. sort( )은 세번째 인자값을 넣을 수 있는데 이 세번 째 인자 값은 배열을 정렬하는 과정에서 조건을 뜻하는데 아래와 같이 사용 가능하다.
> 
> 
> `sort(시작점, 끝점, 조건)` 
> 

```cpp
#include<iostream>
#include<algorithm>

using namespace std;
// 조건 내림차순으로 정렬 하여라 
bool compare(int a, int b)
{
    return a > b; 
}

int main()
{
    int arr[] = {3,1,4,1,5,9,2};
    sort(arr, arr+7, compare);

    for(int i=0; i < 7; i++)
        cout << arr[i] << " ";
    cout << endl;    
}
```

> 위 코드를 보면 알 수 있듯이 sort( )의 세번째 인자는 bool값으로 받게된다. 위 compare함수는 사용자가 원하는 조건에 따라 변형될 수 있고, 위 코드는 내림차순으로 배열을 정렬하도록 변형한 형태다.
> 
> 
> 따라서 2개의 인자값을 넣어준다면 기본 디폴트 값으로 오름차순으로 정렬되는 것이고, 사용자 정의에 따라서 여러 조건으로 정렬이 가능한 것이다. 
> 

### STL sort ( ) 사용법 - 실무에서의 사용 (Class)

---

> 어떤 경우에 우리가 실무에서 정렬이 필요할까??
> 
> 
> 예를들어 데이터가 객체로 정리되어 있어 사용자 정의로 여러개의 변수를 포함하는 경우에는 단순히 데이터의 정렬 기법으론 정렬할 수 없다. 
> 
> ex) (학생, 점수)의 객체에서 점수별로 정리하는 것 
> 
> 따라서 이런 경우에는 아래와 같이 특정한 변수를 기준으로 정렬하는 방식으로 해결할 수 있다. 
> 

```cpp
#include<iostream>
#include<algorithm>

using namespace std;

class Student
{
public:
    string m_name;
    int m_score;
    Student(string name, int score)
    {
        this->m_name = name;    // 멤버 변수 반환 
        this->m_score = score;  // 멤버 변수 반환 
    }
    // 비교 연산자 오버로딩 
    bool operator < (Student &student)
    {
        return this->m_score < student.m_score;
    }
};

int main()
{
    Student student[] = 
    {
        Student("A 쿤", 90),
        Student("B 쿤", 60),
        Student("C 쿤", 93),
        Student("D 쿤", 40),
        Student("E 쿤", 70),
    };

    sort(student, student+5);

    for(int i=0; i < 5; i++)
        cout << student[i].m_name << " ";
    cout << endl;    
}
```

> 위 코드는 class로 사용자 정의 자료형을 만들어 학생의 점수를 기준으로 학생의 이름을 출력하는 코드다.  때문에 자료형이  Student(string, int)형으로 순서대로 이름과 점수가 들어가게 된다.
> 
> 
> 이처럼 하나의 변수만을 정렬하는것이 아닌 사용자의 정의에 때라서 정렬하는 경우에 위와 같은 방법을 쓴다. 
> 

### STL sort ( ) 사용법 - 숏 코딩 빠른개발 (Pair)

---

> 위에서 설명한 클래스(Class)를 사용하여 사용자 정의 자료형을 만들어 특정한 변수를 기준으로 정렬을 할 순 있지만 클래스를 하나하나 정의를 해 주어야 하는 방식은 장기적으로 관리를 하는 차원에선 좋지만 프로그래밍 속도 측면에선 마냥 좋다곤 볼 수 없다. 따라서 이런 경우에는 Vector 라이브러리의 Pair를 사용하여 아래와 같이 해결할 수 있다.
> 

```cpp
#include<iostream>
#include<algorithm>
#include<vector>

using namespace std;

int main()
{
    vector<pair<int, string>> Student_ScoreList;
    Student_ScoreList.push_back(pair<int, string>(90, "A 쿤"));
    Student_ScoreList.push_back(pair<int, string>(50, "B 쿤"));
    Student_ScoreList.push_back(pair<int, string>(60, "C 쿤"));
    Student_ScoreList.push_back(pair<int, string>(20, "D 쿤"));
    Student_ScoreList.push_back(pair<int, string>(94, "E 쿤"));

    sort(Student_ScoreList.begin(), Student_ScoreList.end());

    for(int i=0; i < Student_ScoreList.size(); i++)
        cout << Student_ScoreList[i].second << " ";
    cout << endl;
}
```

> vector의 pair는 한 쌍의 데이터를 처리할 수 있도록 해주는 자료구조라고 할 수 있다.
> 
> 
> 사용법은 아래와 같다. 
> 
> 선언
> 
> - vector<pair<자료형_1, 자료형_2>> Vector;
> 
> 입력 
> 
> - Vector.push_back(pair<자료형_1, 자료형_2>(요소_1, 요소_2));
> 
> 출력 
> 
> - 요소_1 출력
>     - Vector[N].first
> - 요소_2 출력
>     - Vector[N].second

### 예시문제

---

> ***학생명단의 구조가 (이름, 점수, 생년월일) 일때 점수를 기준으로 순서대로 정렬하여라 단. 점수가 같은 경우에는 나이가 더 높은 순위로 정렬 하여라.***
> 
> 
> 
> 학생 1: A 쿤/90점/1996-12-22
> 
> 학생 2: B 쿤/97점/1993-05-18
> 
> 학생 3: C 쿤/95점/1993-03-03
> 
> 학생 4: D 쿤/90점/1997-12/07
> 
> 학생 5: E 쿤/88점/1999-03-02
> 

> 위 문제를 보면 기준이 두개라는 것을 알 수 있다. 이를 해결하기 위해선 먼저 성적을 기준으로 정렬한 뒤 동일한 점수를 가진 학생은 나이순으로 한번 더 정렬을 하면 된다.
> 

```cpp
#include<iostream>
#include<algorithm>
#include<vector>

using namespace std;

bool compare(pair<string, pair<int, int>> a,
             pair<string, pair<int, int>> b)
{   // 학생의 점수가 같은 경우 나이가 많은 순으로 
    if(a.second.first == b.second.first)
        return a.second.second < b.second.second;
    else
        return a.second.first > b.second.first;
}

int main()
{
    vector<pair<string, pair<int, int>>> sl;
    sl.push_back(pair<string, pair<int, int>>("A 쿤", make_pair(90, 19961222)));
    sl.push_back(pair<string, pair<int, int>>("B 쿤", make_pair(97, 19930518)));
    sl.push_back(pair<string, pair<int, int>>("C 쿤", make_pair(95, 19930303)));
    sl.push_back(pair<string, pair<int, int>>("D 쿤", make_pair(90, 19971207)));
    sl.push_back(pair<string, pair<int, int>>("E 쿤", make_pair(88, 19990302)));

    sort(sl.begin(), sl.end(), compare);

    for(int i=0; i < sl.size(); i++)
        cout << sl[i].first << " ";
    cout << endl;
}
```

> vector의 pair는 2중으로 위와 같이 사용할 수 있는데 사용법은
> 
> 
> 선언
> 
> - vector<pair<자료형_1, pair<자료형_2, 자료형_3>>> V;
> 
> 입력 
> 
> - V.push_back(pair<자료형_1, <자료형_2, 자료형_3>> (요소_1, make_pair<요소_2, 요소_3)));
> 
> 출력 
> 
> - 요소_1 출력
>     - Vector[N].first
> - 요소_2 출력
>     - Vector[N].second.first
> - 요소_3 출력
>     - Vector[N].second.second

### **Reference**

---

[9. C++ STL sort() 함수 다루기 ②](https://blog.naver.com/PostView.naver?blogId=ndb796&logNo=221228004692&parentCategoryNo=&categoryNo=&viewDate=&isShowPopularPosts=false&from=postView)