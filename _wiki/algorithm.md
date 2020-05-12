---
layout  : wiki
title   : algorithm
summary : 
date    : 2020-02-10 18:02:44 +0900
updated : 2020-05-12 10:58:07 +0900
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
    - 가능한 알고리즘이 보인다면 구현 세부 항목을 나누고 나눠서 적는다
    - 데이터구조, 변수 정리
    - 각 문장을 코드로 적는다

## 버블정렬

- 두 인접한 데이터를 비교해 앞 데이터가 뒤 데이터보다 크면 자리를 바꾸는 정렬 알고리즘
- [시각화 참고](https://visualgo.net/en/sorting?slide=1)

### 간단한 경우부터 복잡한 경우 순서대로 생각

- 데이터 2개일 떄, 3개일 때, 4개일 떄...

- 데이터가 네 개 일때 (데이터 갯수에 따라 복잡도가 떨어지는 것은 아니므로, 네 개로 바로 로직을 이해해보자.)
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


## Link

- [알고리즘 올인원](https://www.fastcampus.co.kr/dev_online_algo)
