---
title: BitMasking
author: ggh-png
date: 2022-12-01 10:10:00 +0800
categories: [algorithm, Bitmasking, Combination, Combi, permu]
tags: [algorithm, combination, permutation, BitMasking]
render_with_liquid: false
---

# C++ 비트 마스킹

## 개yo

---

이번 포스팅에선 기본적인 비트 연산을 알아보고 직접 구현해 보고, 이를 이용한 조합**Combination**을 함께 알아보도록 하겠다.

## 비트 연산

---

먼저 비트연산에 대하여 알아 보도록 하겠다.  

우선 왼쪽 시프트 연산자 << 는 수를 왼쪽으로 이동한다는 의미다.

비트마스킹에서 보통 (1 << x)이 꼴로 많이 쓰인다.

> int a = 8 → 0110
> 

예를 들어 (1 << 2)는 2의 2승, (1 << 0)은 2의 0승을 상징한다.

| idx번째 비트끄기 | S &= ~(1 << idx) |
| --- | --- |
| idx번째에 대한 XOR 연산(0은 1로, 1은 0으로) | S ^= (1 << idx) |
| 최하위 켜져있는 idx 찾기 | idx = (S & -S) |
| n인 집합의 모든 비트를 켜기 | (1 << n) - 1 |
| idx번째 비트를 켜기 | S |= (1 << idx) |
| idx번째 비트가 있는지 확인하기 | if(S & (1 << idx)) |
- **최하위 비트찾기 idx = (S & -S)**

자 저 -1이 뭔지를 보자.

예를 들어 x = 0110 이라고 해보자. ~x = 1001이된다. (사실 그 위의 있는 비트도 뒤집히긴 하나 생략.)

여기서 ~ x + 1을 하게 되면 1010이 되고

이 ~x +1 & x를 하게 되면 가장 끝자리에 있는 켜져있는 비트를 뽑아낼 수 있다.

- x = (~x + 1) 의 특징을 가지므로
- x & x로 놓을 수 있다.

## ex code

```cpp
#include <iostream>

using namespace std; 

void t1()
{
	int n = 15; // 1111 
	int idx = 2;  
	n &= ~(1 << idx); // 1'1'11 -> 1'0'11 == 11
	cout << "1. idx번째 비트 끄기 : " << n << "\n";
}
void t2()
{
	int n = 11;  // 1011 
	int idx = 2;  
	n ^= (1 << idx); // 1'0'11 -> 1'1'11 == 15
	cout << "2. 0은 1로, 1은 0으로 XOR T2 : " << n << "\n";
}

void t3()
{
	int n = 6;  // 0110
	int idx = (n & -n);  // 0110 & 1110 == 0110
	cout << "3. 최하위 켜져있는 인덱스 T3: " << idx << "\n";
}
void t4()
{
	int n = 3;  // 000 - 100
	cout << "4. 크기가 n인 모든 집합의 모든 비트 켜기 T4 : " << (1 << n) - 1 << "\n";
}
void t5(){
	int n = 5;
	int idx = 1; // 0101 => 0111 7   
	n |= (1 << idx);  
	cout << "5. idx번째 불켜기 T5 : " << n << "\n";
}

void t6(){
	int n = 5; // 0101
	int idx = 1;   
	string a = n & (1 << idx) ? "yes" : "no";
	cout << "6. idx번째 비트가 있는지 확인하기 T6 : " << a << "\n";
}

int main() {   
	t1();
	t2();
	t3();
	t4();
	t5();
	t6(); 
    return 0;
}
/*
1. idx번째 비트 끄기 : 11
2. 0은 1로, 1은 0으로 XOR T2 : 15
3. 최하위 켜져있는 인덱스 T3: 2
4. 크기가 n인 모든 집합의 모든 비트 켜기 T4 : 7
5. idx번째 불켜기 T5 : 7
6. idx번째 비트가 있는지 확인하기 T6 : no
*/
```

### 비트마스킹

어떠한 특정원소를 찾을 때 어느정도의 시간복잡도는 자료구조 마다 다르다.

- 배열의 어떤 요소를 찾을 때 선형적인 시간 : O(N)

- sorted array에서 이분탐색으로 찾을 때는 O(logN)

- 해싱테이블에서는 O(1)

그렇다면 불리언 배열에서 무언가를 찾는 등의 연산은 어떻게 될까? 보통 불리언배열은 vector<bool>, bitset, set<int> 로 표현해서 처리하곤 한다. 여기서 find메서드를 쓰는 등을 통해 연산을 한다. 하지만 이러한 것들보다 비트연산을 쓰면 좀 더 가볍고 빠르게 된다.

**이렇게 불리언배열의 역할을 하는 "하나의 숫자"를 만들어서 "비트 연산자"를 통해 탐색, 수정 등의 작업을 하는 것을 비트마스킹이라고 한다.**

101은 1(0 0 1)과 4(1 0 0)의 합집합이며 이는 {0, 2}로 표현이 가능하고,

111은? {0, 1, 2}로 표현이 가능하다.

즉, 불리언배열을 만들어서 {0, 1, 0, 1} 이렇게 만들지 않고 **0101 이라는 하나의 수, 5** 등을 이용해 **하나의 숫자로 불리언 배열같은 역할**을 할 수 있다. 또한 앞서 배운 비트연산자를 통해 해당 요소가 포함되어있는지 안되어있는지 등을 쉽게 알 수 있다.

참고 : 왜 비트마스킹이라고 할까?

컴퓨터의 저장 공간인 메모리에는 0이나 1이 저장된다. 이러한 메모리의 용량을 표현하는 단위는 여러가지가 있는데 그 중 최소 단위를 비트(Bit)라고 하며 이 비트는 0 또는 1이라는 값을 가진다.  이를 기반으로 해당 비트를 켜거나(1로 만들거나) 끄거나(0으로 만들거나) 하는 "마스킹"하기 때문에 비트마스킹이라고 한다.

참고로 비트는 이진수의 **약어다**. 비트나 이진수나 똑같은 개념이라고 보면 된다. (BIT = Binary Digit)

비스마스킹을 이용한 경우의 수

비스마스킹은 경우의 수를 표현하는데도 잘 쓰이는데 예를 들어 {사과, 딸기, 포도, 배}의 모든 경우의 수는 어떻게 될까?

{사과}

{딸기, 사과}

..

총 16가지수가 나오게 되는데, 사과를 포함하거나 포함하지 않거나 해서 각각의 요소는 2개의 상태값을 가지기 때문에 2^4 = 16이 되는 것이다..

```cpp
#include <bits/stdc++.h>
using namespace std;  
const int n = 4;
int main() {   
	string a[n] = {"사과", "딸기", "포도", "배"};
	for(int i = 0; i < (1 << n); i++){
		string ret = "";
		for(int j = 0; j < n; j++){
			if(i & (1 << j)){
				ret += (a[j] + " ");
			}
		}
		cout << ret << '\n';
	} 
    return 0;
} 
/*

사과 
딸기 
사과 딸기 
포도 
사과 포도 
딸기 포도 
사과 딸기 포도 
배 
사과 배 
딸기 배 
사과 딸기 배 
포도 배 
사과 포도 배 
딸기 포도 배 
사과 딸기 포도 배 
*/
```

이렇게 모든 집합을 표현한 것을 볼 수 있다.

i는 0000, 0001, 0010을 상징한다.

j는 0, 1, 2, 3을 기반으로 (1 << 0), (1 << 1) 등으로 해당 번째의 비트가 켜져있냐 켜져있지 않냐를 통해 집합을 확인 한다.

즉 이러한 것을 통해 4C1, 4C2 ... 이러한 조합들의 모든 경우의 수를 한번의 for문 만으로 표현이 가능한 셈이다.

문제에서 4C1, 4C2 여러가지 조합 등 모든 경우의 수를 기반으로 로직을 짜야 하는 경우 유용하다.

비트마스킹을 이용한 매개변수 전달

사과라는 매개변수가 포함이 되어있고 이어서 사과 + 포도, 사과 + 배 이런식의 매개변수를 더하는 것을 구현하고 싶다면 이렇게 하면 된다.

```cpp
#include <bits/stdc++.h>
using namespace std;  
const int n = 4;
string a[4] = {"사과", "딸기", "포도", "배"};
void go(int num){
	string ret = "";	
	for(int i = 0; i < 4; i++){
		if(num & (1 << i)) ret += a[i] + " ";
	}
	cout << ret << '\n';
	return;
}
int main() {    
	for(int i = 1; i < n; i++){
		go(1 | (1 << i));
	} 
    return 0;
} 
/*
사과 딸기 
사과 포도 
사과 배
*/
```

이처럼 << 또는 & 등 비트연산자 등으로 불리언배열의 역할을 하는 비트마스킹을 알아보았다.

하지만 비트마스킹은 한계가 있습니다. 바로 31까지 가능하다. int형 숫자의 한계까지인 셈이다. long long은 어떠냐 하는 의견도 있는데 보통 2^ 30승정도만 해도... 10억이 되기 때문에 그 이후의 경우의 수를 센다는 것 자체가 이미 시간복잡도를 많이 초과하기 때문에 보통은 30 ~ 31 까지의 경우의 수만을 표현할 수 있다고 볼 수 있다.

### **Reference**

---

[9. C++ STL sort() 함수 다루기 ②](https://blog.naver.com/PostView.naver?blogId=ndb796&logNo=221228004692&parentCategoryNo=&categoryNo=&viewDate=&isShowPopularPosts=false&from=postView)

[[알고리즘 강의] 4주차. 비트마스킹](https://blog.naver.com/jhc9639/222310927725)