---
layout  : wiki
title   : 
summary : 
date    : 2020-05-19 18:45:02 +0900
updated : 2020-05-19 18:52:57 +0900
tags    : 
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

## 1.1 피보나치 수열
피보나치 수열은 첫 번째와 두 번째 숫자를 제외한 모든 숫자가 이전 두 숫자를 합한 숫자를 나열한 수열

```
0, 1, 1, 2, 3, 5, 8, 13, 21 ...
```

n번째 피보나치 수의 값은 아래 수식으로 구할 수 있다.

```
fib(n) = fib(n - 1) + fib(n - 2)
```

### 1.1.1 재귀 함수
재귀 함수는 자기 자신을 호출하는 함수다.

```python
def fib1(n: int) -> int:
    return fib1(n - 1) + fib1(n - 2)
```

코드 실행하면 에러 발생한다. 이런 상황을 무한 재귀라 부른다.

### 1.1.2 재귀 함수의 기저 조건

```python
def fib2(n: int) -> int:
    in n < 2: # 기저 조건
        return n
    return fib2(n - 2) + fib2(n - 1) # 재귀 조건
```

> 검색신공
- **How** - [재귀함수](https://smile2x.tistory.com/entry/%EC%9E%AC%EA%B7%80%ED%95%A8%EC%88%98%EC%9D%98-%EC%9B%90%EB%A6%AC-%EB%B0%8F-%EB%8F%99%EC%9E%91) 는 반복문과 달리 스택에 쌓이고 그 동작하는 과정을 잘 정리해주셔서 링크로 남긴다.
- **Why** - 재귀함수의 대안은 반복문인데 반복문에 변수를 사용하면 오류 가능성이 생긴다. 바꿔 말해 Mutable state가 취할 수 있는 가능한 경우의 수가 생기는 셈이라고 한다.


```python
fib(4) -> fib2(3), fib2(2)
fib(3) -> fib2(2), fib2(1)
fib(2) -> fib2(1), fib2(0)
fib(2) -> fib2(1), fib2(0)
fib(1) -> fib2(1)
fib(1) -> fib2(1)
fib(1) -> fib2(1)
fib(0) -> fib2(0)
fib(0) -> fib2(0)
```

fib()2의 호출 횟수는 9다. 수열 요소 숫자가 증가할수록 호출 증가 횧수는 더 악화된다.


### 1.1.3 메모이제이션
메모이제이션은 계산 작업이 완료되면 결과를 저장하는 기술

```python
from typing import Dict
memo: Dict[int, int] = {0: 0, 1: 1} # 기저 조건

def fib3(n: int) -> int:
    if n not in memo:
        memo[n] = fib3(n - 1) + fib3(n - 2) # 메모이제이션
    return memo[n]
```
`fib2(20)`은 자신을 21,891번 호출하는 반면 fib3(20)은 39번만 호출한다. 메모이제이션은 초기 기저 조건인 0과 1을 미리 저장한 후, 그 후 계산되는 값을 계속 저장한다. 변수 memo에 미리 계산된(저장된) 요소가 있으면 다시 계산하지 않고 결과를 반환한다.

### 1.1.4 메모이제이션 데커레이터
`@functions.lru_cache()` 데커레이터를 사용해 fib4를 작성한다. 어떤 인자와 `fib4()`가 실행될 때마다 데커레이터는 계산된 반환값을 메모리에 캐싱(저장)한다. 이후 동일한 인자와 `fib4()`가 실행되면 캐시된 값을 검색해 반환한다.
```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fib4(n: int) -> int: # fib2()와 같음
    if n<2: # 기저 조건
        return n
    return fib4(n - 2) + fib4(n - 1) # 재귀 조건

if __name__ == "__main__":
    print(fib4(5))
    print(fib4(50))
```

maxsize속성은 가장 최근 호출을 캐시할 수 있는 크기. None은 캐시에 제한이 없다는 의미.


29p

## Links

[재귀함수](https://smile2x.tistory.com/entry/%EC%9E%AC%EA%B7%80%ED%95%A8%EC%88%98%EC%9D%98-%EC%9B%90%EB%A6%AC-%EB%B0%8F-%EB%8F%99%EC%9E%91)
 
