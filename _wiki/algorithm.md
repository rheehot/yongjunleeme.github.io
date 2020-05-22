---
layout  : wiki
title   : algorithm
summary : 
date    : 2020-02-10 18:02:44 +0900
updated : 2020-05-22 16:51:00 +0900
tags    : 
toc     : true
public  : true
parent  : index 
latex   : false
---
* TOC
{:toc}

- 알고리즘 연습방법

    - 연습장과 펜을 준비
    - 간단한 경우부터 복잡한 순서대로 생각
    - 가능한 알고리즘이 보인다면 구현 세부 항목을 나눠서 적는다
    - 데이터구조, 변수 정리
    - 각 문장을 코드로 적는다

## 기본정렬-버블정렬

- 두 인접한 데이터를 비교해 앞 데이터가 뒤 데이터보다 크면 자리를 바꾸는 정렬 알고리즘
- [시각화 참고](https://visualgo.net/en/sorting)
- [시각화 참고2](https://en.wikipedia.org/wiki/Bubble_sort)

### 간단한 경우부터 복잡한 경우 순서대로 생각

- 데이터 2개일 떄, 3개일 때, 4개일 떄...

- 데이터가 네 개 일때 (데이터 개수에 따라 복잡도가 떨어지는 것은 아니므로, 네 개로 바로 로직을 이해해보자.)
- 예: data_list = [1, 9, 3, 2]
- 1차 로직 적용
    - 1 와 9 비교, 자리바꿈없음 [1, 9, 3, 2]
    - 9 와 3 비교, 자리바꿈 [1, 3, 9, 2]
    - 9 와 2 비교, 자리바꿈 [1, 3, 2, 9]
- 2차 로직 적용
    - 1 와 3 비교, 자리바꿈없음 [1, 3, 2, 9]
    - 3 과 2 비교, 자리바꿈 [1, 2, 3, 9]
    - 3 와 9 비교, 자리바꿈없음 [1, 2, 3, 9]
- 3차 로직 적용
    - 1 과 2 비교, 자리바꿈없음 [1, 2, 3, 9]
    - 2 과 3 비교, 자리바꿈없음 [1, 2, 3, 9]
    - 3 과 9 비교, 자리바꿈없음 [1, 2, 3, 9]

### 특이점 찾아보기

- n개의 리스트가 있는 경우 최대 n-1번의 로직을 적용한다.
- 로직을 1번 적용할 때마다 가장 큰 숫자가 뒤에서부터 1개씩 결정된다.
- 로직이 경우에 따라 일찍 끝날 수도 있다. 따라서 로직을 적용할 때 한 번도 데이터가 교환된 적이 없다면 이미 정렬된 상태이므로 더 이상 로직을 반복 적용할 필요가 없다.

### 세부 항목 나눠서 적기

1. for num in range(len(data_list)) 반복
2. swap = 0 (교환이 되었는지를 확인하는 변수를 두자)
3. 반복문 안에서, for index in range(len(data_list) - num - 1) n - 1번 반복해야 하므로
4. 반복문 안의 반복문에서, if data_list[index] > data_list[index + 1]이면
5. data_list[index], data_list[index + 1] = data_list[index + 1], data_list[index]
6. swap += 1
7. 반복문 안에서, if swap == 0이면, break 끝

### 코드 작성

```python
def bubblesort(data):
    for index in range(len(data) - 1):
        swap = False
        for index2 in range(len(data) - index - 1):
            if data[index2] > data[index2 + 1]:
                data[index2], data[index2 + 1] = data[index2 + 1], data[index2]
                swqp = True
        if swap == False:
            break
    return data
```

```python
improt random
data_list = random.sample(range(100), 50)
print(bubblesort(data_list))
```

### 시간복잡도 분석

- 반복문이 두 개 O( 𝑛2 )
    - 최악의 경우,  𝑛(𝑛−1) / 2 
- 완전 정렬이 되어 있는 상태라면 최선은 O(n)

## 기본정렬-삽입정렬

- 2번째 인덱스(1)부터 시작
- 해당 인덱스 값과 그 앞 인덱스 값부터 앞쪽으로 비교해서 값이 작으면 값을 바꾼다
- [그림으로 보기1](https://visualgo.net/en/sorting?slide=1)
- [그림으로 보기2](https://commons.wikimedia.org/wiki/File:Insertion-sort-example.gif)

### 간단한 경우부터 복잡한 경우 순서대로 생각

- 예: data_list = [9, 3, 2, 5]
    - 처음 한번 실행하면, key값은 9, 인덱스(0) - 1 은 0보다 작으므로 끝: [9, 3, 2, 5]
    - 두 번째 실행하면, key값은 3, 9보다 3이 작으므로 자리 바꾸고, 끝: [3, 9, 2, 5]
    - 세 번째 실행하면, key값은 2, 9보다 2가 작으므로 자리 바꾸고, 다시 3보다 2가 작으므로 끝: [2, 3, 9, 5]
    - 네 번째 실행하면, key값은 5, 9보다 5이 작으므로 자리 바꾸고, 3보다는 5가 크므로 끝: [2, 3, 5, 9]

### 세부 항목 나눠서 적기

1. for stand in range(len(data_list)) 로 반복
2. key = data_list[stand]
3. for num in range(stand, 0, -1) 반복
    - 내부 반복문 안에서 data_list[stand] < data_list[num - 1] 이면,
        - data_list[num - 1], data_list[num] = data_list[num], data_list[num - 1]

### 코드 작성

```python
def insertion_sort(data):
    for index in range(len(data) - 1):
        for index2 in range(index + 1, 0, -1):
            if data[index2] < data[index2 - 1]:
                data[index2], data[index2 - 1] = data[index2 - 1], data[index2]
            else:
                break
    return data
    
import random

data_list = random.sample(range(100), 50)
print (insertion_sort(data_list))
```

### 시간복잡도 분석

- 반복문이 두 개 O( 𝑛2 )
- 최악의 경우,  𝑛∗(𝑛−1) / 2 
- 완전 정렬이 되어 있는 상태라면 최선은 O(n)

## 기본정렬-선택정렬 (최솟값)

### 선택정렬이란? - 최솟값을 선택한다

- 주어진 데이터 중 최솟값을 찾아서 맨 앞에 위치한 값과 교체
- 맨 앞의 위치한 값을 뺀 나미저 데이터를 동일한 방법으로 반복
- [그림으로 보기1](https://visualgo.net/en/sorting?slide=1)
- [그림으로 보기2](https://en.wikipedia.org/wiki/Selection_sort) - selection_sort 선택


### 간단한 경우부터 복잡한 경우 순서대로 생각

- 데이터가 두 개 일때
    - 예: dataList = [9, 1]
        - data_list[0] > data_list[1] 이므로 data_list[0] 값과 data_ list[1] 값을 교환
- 데이터가 세 개 일때
    - 예: data_list = [9, 1, 7]
        - 처음 한번 실행하면, 1, 9, 7 이 됨
        - 두 번째 실행하면, 1, 7, 9 가 됨
- 데이터가 네 개 일때
    - 예: data_list = [9, 3, 2, 1]
        - 처음 한번 실행하면, 1, 3, 2, 9 가 됨
        - 두 번째 실행하면, 1, 2, 3, 9 가 됨
        - 세 번째 실행하면, 변화 없음

### 세부 항목 나눠서 적기

1. for stand in range(len(data_list) - 1) 로 반복
2. lowest = stand 로 놓고,
3. for num in range(stand, len(data_list)) stand 이후부터 반복
4. 내부 반복문 안에서 data_list[lowest] > data_list[num] 이면,
5. lowest = num
6. data_list[num], data_list[lowest] = data_list[lowest], data_list[num]

### 코드 구현

```python
def selection_sort(data):
    for stand in range(len(data) - 1): # 2중 for문
        lowest = stand
        for index in range(stand + 1, len(data)):
            if data[lowest] > data[index]:
                lowest = index
        data[lowest], data[stand] = data[stand], data[lowest]
    return data
```

```python
import random

data_list = random.sample(range(100), 10) # 0~99까지 랜덤수를 10개 생성

selection_sort(data_list)
-> [9, 12, 13, 24, 53, 55, 69, 80, 87, 98]
```

### 시간 복잡도 분석

```python
반복문이 두 개 O( 𝑛2 )
    실제로 상세하게 계산하면,  𝑛∗(𝑛−1) / 2
```

## 재귀

```python
def factorial(num):
    if num > 1:
        return num * factorial(num - 1)
    else:
        return num
```

### 시간, 공간복잡도

- 시간 복잡도/공간 복잡도는 O(n-1) 이므로 결국, 둘 다 O(n)
    - 일종의 n-1번 반복문을 호출한 것과 동일
    - factorial() 함수를 호출할 때마다, 지역변수 n 이 생성됨

### 예제

- 재귀 함수를 활용해서 완성해서 1부터 num까지의 곱이 출력되게 만드세요

```python
def multiple(num):
    if num <= 1:
        return num
    return num * multiple(num - 1)
```

- 숫자가 들어 있는 리스트가 주어졌을 때, 리스트의 합을 리턴하는 함수를 만드세요

```python
import random 
data = random.sample(range(100), 10)
print(data)

def sum_list(data):
    if len(data) <= 1:
        return data[0]
    return data[0] + sum_list(data[1:])

print(sum_list(data))
```

#### 회문(palindrome)

- 회문(palindrome)은 순서를 거꾸로 읽어도 제대로 읽은 것과 같은 단어와 문장을 의미함

```python
def palindrome(string):
    if len(strung) <= 1: # 가운데 있는 회문 중간 글자까지 왔다는 것을 의미하므로
        return True
    
    if string[0] == string[-1]:
        return palindrome(string[1:-1])
    else:
        return False
```

#### 문제

1, 정수 n에 대해
2. n이 홀수이면 3 X n + 1 을 하고,
3. n이 짝수이면 n 을 2로 나눕니다.
4. 이렇게 계속 진행해서 n 이 결국 1이 될 때까지 2와 3의 과정을 반복합니다.

예를 들어 n에 3을 넣으면,

`
3
10
5
16
8
4
2
1
`

이렇게 정수 n을 입력받아, 위 알고리즘에 의해 1이 되는 과정을 모두 출력하는 함수를 작성하세요.

```python
def func(n):
    print (n)
    if n == 1:
        return n
    
    if n % 2 == 1:
        return (func((3 * n) + 1))
    else:
        return (func(int(n / 2)))
```


## Link

- [알고리즘 올인원](https://www.fastcampus.co.kr/dev_online_algo)
