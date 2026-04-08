# Week 01 — 알고리즘 기초 및 오리엔테이션 (Summary)

이 문서는 알고리즘의 정의, 특징, 그리고 대표적인 기초 문제들을 통해 알고리즘적 사고방식을 정리합니다. 1주차 강의 자료(OT 및 이론)의 모든 내용을 포함하고 있습니다.

---

## 📝 목차
1. [강의 개요 및 로드맵](#syllabus)
2. [알고리즘의 정의 및 특징](#intro)
3. [역사 속의 알고리즘 (Euclid's GCD)](#history)
4. [탐색 알고리즘 (Search Algorithms)](#search)
5. [탐욕 알고리즘 (Greedy Algorithm)](#greedy)
6. [고전적 문제 해결 (Euler & Maze)](#classic)
7. [효율적인 문제 해결 (Divide & Conquer)](#divide)
8. [이진 인코딩 (Poisoned Wine)](#binary)

---

<a id="syllabus"></a>
## 1. 강의 개요 및 로드맵 (#syllabus)

### 1.1 성적 평가 방식 (Grading)
| 항목 | 비중 | 비고 |
| :--- | :--- | :--- |
| **과제 (Assignment)** | **10%** | 퀴즈(5%) + 홈워크(5%) |
| **중간고사 (Written)** | **30%** | 8주차, 수기 시험 |
| **기말고사 (Project)** | **30%** | 팀 프로젝트 (9~13주차) |
| **기말고사 (Written)** | **30%** | 15주차, 수기 시험 |

> **주의**: 전체 수업 시간의 **1/3** 이상 결석 시 성적 부여 불가.

### 1.2 강의 로드맵 (Roadmap)
- **1~2주차**: 알고리즘 기초 및 성능 분석 (Big-O)
- **3~4주차**: 분할 정복 (Divide and Conquer)
- **5~6주차**: 탐욕 알고리즘 (Greedy Algorithms)
- **7주차**: 정렬 및 선택 (Sorting & Selection)
- **9~10주차**: 그래프 알고리즘 및 최단 경로
- **11~12주차**: 동적 계획법 (Dynamic Programming)
- **13~14주차**: 문자열 매칭 및 NP-완전성

### 1.3 유용한 사이트
- **문제 풀이**: [Baekjoon](https://www.acmicpc.net/), [Programmers](https://programmers.co.kr/), [LeetCode](https://leetcode.com/)
- **시각화**: [VisuAlgo](https://visualgo.net/), [Data Structure Visualizations](https://www.cs.usfca.edu/~galles/visualization/Algorithms.html)

---

<a id="intro"></a>
## 2. 알고리즘의 정의 및 특징 (#intro)

**알고리즘** 이란 어떤 문제를 해결하기 위해 정해진 **유한한 단계** 의 절차를 의미합니다.

### 2.1 알고리즘의 기본 구조
```
[ Input ] (Problem)  -->  [ Algorithm ] (Finite Steps)  -->  [ Output ] (Result)
```

### 2.2 알고리즘과 자료구조
> **Algorithms + Data Structures = Programs**
>
> — Niklaus Wirth

- **자료구조**: 데이터를 어떻게 **저장** 할 것인가? (예: 영어 사전의 단어 배치)
- **알고리즘**: 문제를 어떻게 **해결** 할 것인가? (예: 단어 찾기 방법)
- 자료구조가 정렬되어 있는지에 따라 사용할 수 있는 알고리즘의 효율성이 달라집니다.

---

<a id="history"></a>
## 3. 역사 속의 알고리즘 (Euclid's GCD) (#history)

알고리즘이라는 단어는 9세기 페르시아 수학자 **알콰리즈미 (al-Khwarizmi)** 의 이름에서 유래되었습니다. 인류 최초의 알고리즘으로 알려진 것은 기원전 300년경 유클리드의 **최대공약수(GCD) 알고리즘** 입니다.

### 3.1 유클리드 호제법 (Euclid's Algorithm)
두 수의 최대공약수를 구하기 위해, 큰 수를 작은 수로 나눈 나머지를 반복적으로 취하는 방식입니다.

```python
def gcd(a, b):
    # b가 0이 될 때까지 반복
    while b != 0:
        a, b = b, a % b
    return a

# 예시: gcd(48, 18)
# 48, 18 -> 18, 12 (48%18=12) -> 12, 6 (18%12=6) -> 6, 0 (12%6=0) -> 결과: 6
```

---

<a id="search"></a>
## 4. 탐색 알고리즘 (Search Algorithms) (#search)

### 4.1 최댓값 찾기 (Finding the Maximum)
카드를 한 장씩 넘기며 지금까지 본 숫자 중 가장 큰 숫자를 기억하는 방식입니다. (**순차 탐색** 의 일종)

```python
def find_max(cards):
    # 1. 첫 번째 카드를 최댓값으로 기억
    max_val = cards[0]
    # 2. 다음 카드들을 하나씩 확인
    for i in range(1, len(cards)):
        # 3. 현재 카드가 더 크면 업데이트
        if cards[i] > max_val:
            max_val = cards[i]
    return max_val
```

### 4.2 이진 탐색 (Binary Search)
**정렬된 데이터** 에서 중앙값을 기준으로 탐색 범위를 절반씩 줄여나가는 전략입니다.

- **핵심 아이디어**: 데이터가 정렬되어 있다면 굳이 다 볼 필요가 없다!
- **효율성 비교 ($n$ 개 데이터 기준)**:

| 방식 | 정렬 필요 여부 | 최악의 경우 비교 횟수 |
| :--- | :---: | :--- |
| **순차 탐색** | No | $n$ 번 |
| **이진 탐색** | **Yes** | $\log\_{2}{n}$ 번 |

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid # 발견!
        elif arr[mid] < target:
            left = mid + 1 # 오른쪽 절반 탐색
        else:
            right = mid - 1 # 왼쪽 절반 탐색
    return -1 # 미발견
```

---

<a id="greedy"></a>
## 5. 탐욕 알고리즘 (Greedy Algorithm) (#greedy)

매 순간 **현 시점에서 가장 최선인 선택** 을 내리는 방식입니다. 이 선택이 전체적으로도 최선이길 희망하는 전략입니다.

### 5.1 동전 교환 문제 (Coin Change)
최소 개수의 동전으로 거스름돈을 주는 문제입니다. 한국 동전 단위(500, 100, 50, 10)처럼 큰 단위가 작은 단위의 배수일 때 최적의 해를 보장합니다.

```python
def coin_change(amount, coins=[500, 100, 50, 10]):
    result = []
    for coin in coins:
        count = amount // coin # 현재 단위로 줄 수 있는 최대 개수
        if count > 0:
            result.append((coin, count))
            amount -= coin * count # 남은 액수 계산
    return result
```

---

<a id="classic"></a>
## 6. 고전적 문제 해결 (Euler & Maze) (#classic)

### 6.1 오일러 경로 (Euler Path)
모든 간선을 정확히 한 번씩만 지나는 경로입니다. (한 붓 그리기)
- **핵심 전략**: 선택지가 있을 때, **브리지 (Bridge, 끊으면 그래프가 분리되는 간선)** 를 피하고 **사이클 (Cycle)** 이 있는 쪽을 먼저 택합니다.

### 6.2 미로 찾기 (Right-Hand Rule)
벽에 오른손을 대고 계속 따라가면 반드시 출구에 도달한다는 알고리즘입니다. 벽이 하나로 연결되어 있다는 기하학적 특징을 이용한 단순하고 우아한 해결책입니다.

---

<a id="divide"></a>
## 7. 효율적인 문제 해결 (Divide & Conquer) (#divide)

문제를 더 작은 단위로 나누어 해결하는 방식입니다.

### 7.1 가짜 동전 찾기 (Fake Coin)
$n$ 개의 동전 중 가벼운 가짜를 찾을 때, 한 개씩 비교하는 것보다 더미를 절반씩 나누어 저울에 다는 것이 훨씬 빠릅니다.

- **비교 횟수**:
    - 하나씩 비교: $n - 1$ 번
    - **절반씩 나누기**: $\log\_{2}{n}$ 번 (예: 1,024개 동전도 단 **10번** 만에 해결)

---

<a id="binary"></a>
## 8. 이진 인코딩 (Poisoned Wine) (#binary)

1,000개의 포도주 병 중 독이 든 1병을 최소 인원으로 1주일 만에 찾아내는 문제입니다.

- **원리**: 각 병에 이진수 번호(0~999)를 부여합니다.
- **방법**: $k$ 번째 비트가 1인 모든 병을 $k$ 번째 신하가 마십니다.
- **결과**: 죽은 신하들의 위치를 이진수로 읽으면 그것이 곧 독포도주의 번호가 됩니다.
- **필요 인원**: $2^{10} = 1024$ 이므로, 1,000병을 확인하는 데는 단 **10명** 의 신하만 필요합니다.

---

## 💡 요약 및 시사점
1. 알고리즘은 **유한성, 확정성, 효율성** 이 중요합니다.
2. **$\log\_{2}{n}$** 은 알고리즘 효율성 분석에서 가장 자주 등장하는 중요한 수치입니다.
3. 문제의 특성(정렬 여부, 배수 관계 등)에 따라 **탐욕법, 분할 정복, 인코딩** 등 적절한 전략을 선택해야 합니다.
