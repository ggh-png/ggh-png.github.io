---
title: big(O) notation
author: ggh-png
date: 2022-03-14 14:10:00 +0800
categories: [algorithm, 자료구조]
tags: [algorithm]
render_with_liquid: false
---

# big (O) notation

### 개yo

---

프로그래밍에 있어 가장 고려해야되는 점이 있다고 하자면, 우린 프로그램의 효율성... 즉 프로그램이 처리되는 속도와, 메모리를 사용하는 공간 정도라고 할 수 있다. 이런 속도, 공간의 정도를 비교하는 수식으로 표현하는 방식이 있는데 이것을 우린  `big(O)` 표기법이라고 한다. 이번 포스팅에선 `big(O)` 에 대해 알아보도록 하겠다.  

 

### big(O) notation

---

> big(O) notation
> 
> 
> 빅오 표기법이라고 하는걸 우린 왜 사용할까...?
> 

빅오 표기법을 단순하게 표현 하자면 코드의 효율성을 수식으로 나타내어 주는 표기법이다. 

알고리즘의 효율성은 input값의 개수 N 이라고 할 때 연산을 하는 횟수를 가지고 판별한다. 

따라서 이를 가지고 시간과 공간의 효율성을 따질 수 있는데 이를 각각 `시간 복잡도` , `공간 복잡도` 라고 하며, big(O)의 크기가 클 수록 효율성이 떨어진다는것을 알 수 있다. 

#### 특징 

1. 상수 무시 
    - 빅오 표기법에서 데이터 입력값(N)이 충분히 크다고 가정을 하고 있다.  따라서  알고리즘의 효율성 또한 데이터 입력값(N)의 크기에 따라 영향 받기에 상수 부분은 무시한다.
    
    > ex) O(2N) → O(N)
    > 
2. 영향력이 없는 항 무시
    - 빅오 표기법은 데이터 입력값(N)의 크기에 따라 영향을 받기 때문에 가장 영향력이 큰 항에 이외에 영향력이 없는 항들은 무시한다.
    
    > ex) O(N^2 + 3N + 2) → O(N^2)
    > 

### big(O) Functions

---

> 아래의 그래프는 주로 쓰이는 빅 오 표기들의 그래프를 나타낸 것이다.


![bigO](https://user-images.githubusercontent.com/71277820/158517098-ef5617c9-47b2-4344-b53d-e7227ebdda47.png)

#### 슉 슈슉

 위 그래프에 있는 식들은 big(O)를 가지고 주로 쓰이는 함수들이며, 순서대로 알아보도록 하겠다. 

### 1. O(1) : Constant Time

> 입력 데이터의 크기와 상관없이 언제나 일정한 시간이 걸린다.
> 
> 
>  아래의 코드는 O(1)을 구현한 것이다.  
> 

```cpp
Function(int arr[N])
{
	return (arr[0] == 0) ? true : false;
}
```

### 2. O(N) : Linear Search

> 입력 데이터의 크기와 비례한 만큼 시간이 늘어난다.
> 
> 
> 아래의 코드는 O(N)을 구현한 것이다. 
> 

```cpp
Function(int arr[N])
{
	for(int i=0; i < N; i++)
		cout << i << endl;
}
```

### 3. O(N^2) : Quardratic Time

> 입력 데이터의 크기에 따라 시간이 지수 그래프를 그리며 증가함
> 
> 
> 아래의 코드는 O(N^2)을 구현한 것이다. 
> 

```cpp
Function(int arr[N])
{
	for(int i=0; i < N; i++)
		for(int j=0; i < N; i++)
			cout << i + j << endl;
}
```

### 4. O(N*M)

> 특징은 3의 식과 비슷하지만  다르며, 대체적으로 O(N^2)보단 시간이 덜 증가한다.
> 
> 
> 아래의 코드는 O(N*M)을 구현한 것이다. 
> 

```cpp
Function(int arr[N], M)
{
	for(int i=0; i < N; i++)
		for(int j=0; i < M; i++)
			cout << i + j << endl;
}
```

### 5. O(N^3)

> O(N^2)와 특징은 같으며, 처리시간이 보다 더 급격하게 늘어난다.
> 
> 
> 아래 코드는 O(N^3)을 구현한 것이다. 
> 

```cpp
Function(int arr[N])
{
	for(int i=0; i < N; i++)
		for(int j=0; j < N; j++)
			for(int k=0; k < N; j++)
			cout << i + j + k << endl;
}
```

### 6. O(2^N)

> 보통 일반적으로 재귀 함수를 이용해  피보나치 수열을 만드는데 사용된다.
> 
> 
> 특징으로는 매번 함수가 호출될 때마다 두번씩 호출하며, 트리의 높이만큼 반복한다. 
> 
> 아래의 코드는 재귀함수를 이용하여 피보나치 수열을 구현한 것이다.
> 

```cpp
Function(int r[n]) 
{
	if(n <= 0) return 0;
	else if(n == 1) return r[n] = 1;
	return r[n] = Function(n - 1, r[n]) + Function(n - 2, r[n]); 
}
```

### 7. O(log N) : Logarithmic Time

> 주로 이진검색(binary search) 알고리즘에 사용되며, 검색되는 데이터가 절반씩 줄어드는 특징을 가지고 있으며, 앞서 설명한 알고리즘 중 가장 시간 복잡도가 적다.
> 
> 
> 단. 정렬되지 않은 배열에는 사용할 수 없다. 
> 
> 아래의 코드는 O(log N)를 이용하여 이진검색을 구현한 것이다.
> 

```cpp
Function(int k, arr[], begin, end)// k : 피봇값이 아닐까...?
{
	if(begin > end) return -1;
	int m = (begin + end) / 2; // m : 배열의 중앙 index
	if(arr[m] == k) return m;
	// key값이 중간 값 보다 작은 경우 : 처음부터 중간까지 
	else if(arr[m] > k) return Function(k, arr[], brgin, m-1);
	// key값이 중간 값 보다 큰 경우 : 중간부터 끝까지 
	else return Function(k, arr, m+1, end);
}
```

> 아래는 정렬되지 않은 9개의 요소를 가지고 있는 데이터를 이진검색(binary search)을 사용한 위 코드의 과정이라 볼 수 있다.
> 
> 
> 2진트리의 모습을 띄고 있으며, 이진검색이기 때문에 시간복잡도는 O(log N)이다. 
> 
> 따라서 데이터 N의 값은 9이기 때문에 시간 복잡도의 값은 O(log 8) ~> O(3)이라 할 수 있다. 
> 

```
 1 2 3 4 5 6 7
 
 1 2 3 4 | 5 6 7             ~>  1단계
 
 1 2 | 3 4 | 5 6 | 7         ~>  2단계 
 
 1 | 2 | 3 | 4 | 5 | 6 | 7   ~>  3단계 

```

### **Reference**

---

> 본 포스팅은 엔지니어 대한민국님의 유튜브를 참고하여 작성하였습니다.
>