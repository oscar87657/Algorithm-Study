# 📝 알고리즘 중간고사 요약 정리 (Week 01 ~ 06)

본 문서는 알고리즘 중간고사를 대비하여 1주차부터 6주차까지의 핵심 내용을 정리한 요약본입니다.

---

## 📑 목차

- [1. 알고리즘 개요 및 기초](#intro)
    - [1.1. 알고리즘의 정의 및 조건](#intro_1)
    - [1.2. 효율성 분석: 시간 및 공간 복잡도](#intro_2)
    - [1.3. 점근적 표기법 (Asymptotic Notation)](#intro_3)
    - [1.4. Python 실전 최적화 및 특수 문법](#intro_4)
- [2. 알고리즘 설계 및 복잡도 분석](#complexity)
- [3. 기본 자료구조 및 정렬 알고리즘](#sorting)
- [4. 분할 정복 알고리즘](#divide_and_conquer)
- [5. 탐욕 알고리즘](#greedy)
- [6. 동적 계획법 (DP)](#dynamic_programming)

---

<a id="intro"></a>
## 1. 알고리즘 개요 및 기초

<a id="intro_1"></a>
### 1.1. 알고리즘의 정의 및 조건

알고리즘은 특정 문제를 해결하기 위해 정의된 **유한한 단계의 절차**를 의미합니다. 단순히 코드를 짜는 것을 넘어, 다음의 조건을 충족해야 합니다.

- **입력 (Input)**: 외부에서 제공되는 0개 이상의 데이터.
- **출력 (Output)**: 적어도 1개 이상의 결과 생성.
- **명확성 (Definiteness)**: 각 단계는 모호함 없이 명확하게 정의되어야 함.
- **유한성 (Finiteness)**: 알고리즘은 반드시 유한한 단계 내에서 종료되어야 함.
- **유효성 (Effectiveness)**: 모든 연산은 종이와 펜으로 수행 가능할 만큼 기본적이어야 함.

<a id="intro_2"></a>
### 1.2. 효율성 분석: 시간 및 공간 복잡도

알고리즘을 평가할 때 가장 중요한 척도는 **자원 사용의 효율성**입니다.

- **시간 복잡도 (Time Complexity)**: 입력 크기 $n$ 에 따른 기본 연산(비교, 대입, 연산 등)의 횟수를 측정합니다. 실제 실행 시간은 하드웨어 성능에 의존하므로 연산 횟수의 증가율을 분석하는 것이 핵심입니다.
- **공간 복잡도 (Space Complexity)**: 알고리즘 실행에 필요한 메모리 양을 의미합니다. 최근 하드웨어의 발전으로 시간 복잡도가 더 중요하게 여겨지지만, 대용량 데이터 처리 시에는 여전히 중요한 고려 사항입니다.

<a id="intro_3"></a>
### 1.3. 점근적 표기법 (Asymptotic Notation)

입력 크기가 무한히 커질 때 알고리즘의 실행 시간이 어떻게 변하는지를 나타냅니다.

- **Big-O ($O(\cdot)$)**: **"아무리 늦어도 이보다는 빠르다"** 는 상한(Upper bound)을 의미합니다. 최악의 경우를 대비해야 하는 시스템 설계에서 가장 많이 사용됩니다.
- **Big-Omega ($\Omega(\cdot)$)**: **"아무리 빨라도 이보다는 느리다"** 는 하한(Lower bound)을 의미합니다.
- **Big-Theta ($\Theta(\cdot)$)**: 상한과 하한이 일치하는 경우로, 평균적인 실행 시간을 정확히 설명할 때 사용됩니다.

<a id="intro_4"></a>
### 1.4. Python 실전 최적화 및 특수 문법

알고리즘 시험 및 실습에서 Python의 성능을 극대화하기 위한 필수 기법들입니다.

- **빠른 입출력**: `sys.stdin.readline` 을 사용하여 대량의 입력 데이터를 처리할 때 `input()` 보다 수십 배 빠른 속도를 보장합니다.
- **리스트 컴프리헨션 (List Comprehension)**: `[i for i in range(100) if i % 2 == 0]` 와 같이 선언적 방식으로 리스트를 생성하여 가독성과 속도를 모두 챙길 수 있습니다.
- **Deque 활용**: `collections.deque` 는 양방향 큐로, 리스트의 `pop(0)` ($O(n)$) 과 달리 `popleft()` 를 $O(1)$ 에 수행할 수 있어 큐 구현 시 필수적입니다.
- **재귀 제한**: Python의 기본 재귀 깊이는 약 1,000회로 제한되어 있으므로, 깊은 재귀를 사용하는 경우 `sys.setrecursionlimit(10**6)` 과 같은 설정이 필요합니다.

<a id="complexity"></a>
## 2. 알고리즘 설계 및 복잡도 분석

- **점근적 분석 (Asymptotic Analysis)**: 입력 크기 $n$ 이 무한히 커질 때의 효율성 분석.
- **Big-O 표기법 ($O(\cdot)$)**: 상한 (Worst-case) 분석.
- **Big-Omega 표기법 ($\Omega(\cdot)$)**: 하한 (Best-case) 분석.
- **Big-Theta 표기법 ($\Theta(\cdot)$)**: 평균적 (Average-case) 분석.
- **시간 복잡도와 공간 복잡도**: 알고리즘 실행 시간 및 메모리 사용량 분석.

<a id="sorting"></a>
## 3. 기본 자료구조 및 정렬 알고리즘

- **선형 자료구조**: 배열 (Array), 스택 (Stack), 큐 (Queue).
- **기초 정렬 알고리즘**:
    - **버블 정렬 (Bubble Sort)**: 인접한 원소 비교 및 교환.
    - **선택 정렬 (Selection Sort)**: 최솟값을 찾아 맨 앞으로 이동.
    - **삽입 정렬 (Insertion Sort)**: 정렬된 부분에 적절한 위치 삽입.
- **복잡도 비교**: 각 정렬 알고리즘의 최악/평균 시간 복잡도 분석.

<a id="divide_and_conquer"></a>
## 4. 분할 정복 알고리즘 (Divide and Conquer)

- **설계 패러다임**: 분할 (Divide) $\rightarrow$ 정복 (Conquer) $\rightarrow$ 결합 (Combine).
- **합병 정렬 (Merge Sort)**: $O(n \log\_{2}{n})$ 의 안정적인 정렬 알고리즘.
- **퀵 정렬 (Quick Sort)**: 피벗 (Pivot)을 이용한 분할.
- **재귀와 점화식**: 마스터 정리 (Master Theorem)를 이용한 시간 복잡도 계산.

<a id="greedy"></a>
## 5. 탐욕 알고리즘 (Greedy Algorithms)

- **핵심 속성**:
    - **탐욕적 선택 속성 (Greedy Choice Property)**: 현재 단계에서의 최선이 전역 최선으로 이어짐.
    - **최적 부분 구조 (Optimal Substructure)**: 문제의 최적해가 부분 문제의 최적해를 포함함.
- **주요 문제**:
    - **활동 선택 문제 (Activity Selection)**: 종료 시간이 빠른 순서대로 선택.
    - **허프만 코딩 (Huffman Coding)**: 빈도 기반 가변 길이 인코딩.

<a id="dynamic_programming"></a>
## 6. 동적 계획법 (Dynamic Programming)

- **핵심 개념**: 중복되는 부분 문제 해결을 위해 결과를 저장 (Memoization/Tabulation).
- **주요 문제**:
    - **피보나치 수열 (Fibonacci)**: 재귀 vs DP 성능 비교.
    - **최장 공통 부분 수열 (LCS)**: 문자열 비교 및 테이블 작성.
    - **배낭 문제 (0/1 Knapsack Problem)**: 물건을 쪼갤 수 없는 경우의 최적화.

---
