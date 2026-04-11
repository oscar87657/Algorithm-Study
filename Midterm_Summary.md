# 📝 알고리즘 중간고사 요약 정리 (Week 01 ~ 06)

본 문서는 알고리즘 중간고사를 대비하여 1주차부터 6주차까지의 핵심 내용을 정리한 요약본입니다.

---

## 📑 목차

- [1. 알고리즘 개요 및 기초](#intro)
- [2. 알고리즘 설계 및 복잡도 분석](#complexity)
- [3. 기본 자료구조 및 정렬 알고리즘](#sorting)
- [4. 분할 정복 알고리즘](#divide_and_conquer)
- [5. 탐욕 알고리즘](#greedy)
- [6. 동적 계획법 (DP)](#dynamic_programming)

---

<a id="intro"></a>
## 1. 알고리즘 개요 및 기초

- **알고리즘의 정의**: 문제를 해결하기 위한 단계적인 절차.
- **Computational Thinking**: 문제 해결을 위한 논리적 사고 방식.
- **Python 기초**: 리스트 컴프리헨션, 슬라이싱 등 알고리즘 구현에 필요한 핵심 문법.

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
