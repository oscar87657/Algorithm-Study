# Week 06 — 동적 계획법 (Dynamic Programming)

이 문서는 큰 문제를 작은 부분 문제로 나누어 해결하고, 그 결과를 저장하여 재사용하는 **동적 계획법 (Dynamic Programming)** 의 원리, 설계 전략, 그리고 주요 사례(LCS, 배낭 문제, 모든 쌍 최단 경로 등)를 상세히 다룹니다.

---

## 📝 목차
1. [동적 계획법(Dynamic Programming) 개요](#intro)
2. [기초 DP 사례 (TBA)](#basic)
3. [최장 공통 부분 수열 (LCS) (TBA)](#lcs)
4. [0-1 배낭 문제 (0-1 Knapsack) (TBA)](#knapsack)
5. [모든 쌍 최단 경로 (Floyd-Warshall) (TBA)](#floyd)
6. [편집 거리 (Edit Distance) (TBA)](#edit)
7. [성능 요약 및 알고리즘 비교 (TBA)](#comparison)

---

<a id="intro"></a>
## 1. 동적 계획법(Dynamic Programming) 개요 (#intro)

**동적 계획법 (Dynamic Programming, DP)** 은 복잡한 문제를 더 작은 하위 문제로 나누고, 그 하위 문제의 해를 저장하여 다시 계산하는 낭비를 방지함으로써 효율적으로 최적해를 찾는 알고리즘 설계 패러다임입니다.

### 1.1 기본 원리
단순 재귀는 동일한 계산을 반복적으로 수행하여 성능이 기하급수적으로 저하될 수 있습니다. DP는 **"과거의 계산 결과를 기억한다"** 는 아이디어를 통해 이를 해결합니다.

1.  작은 부분 문제들을 먼저 해결합니다.
2.  해결된 해를 테이블(메모리)에 **저장**합니다.
3.  더 큰 문제를 해결할 때 저장된 해를 가져와 사용합니다.
4.  최종적으로 원래 문제의 해를 구합니다.

### 1.2 핵심 속성 (Two Key Properties)
DP를 적용하기 위해서는 문제의 구조가 다음 두 가지 성질을 만족해야 합니다.

1.  **최적 부분 구조 (Optimal Substructure)**: 큰 문제의 최적해가 작은 부분 문제들의 최적해들로부터 구성될 수 있는 성질입니다.
2.  **중복되는 부분 문제 (Overlapping Subproblems)**: 동일한 작은 문제들이 반복해서 나타나는 성질입니다. 분할 정복과 달리, DP는 이 중복된 해를 한 번만 계산하고 재사용합니다.

**[분할 정복 vs DP 시각화]**
```text
[Divide & Conquer]           [Dynamic Programming]
       (A)                          (A)
      /   \                        /   \
    (B)   (C)                    (B)---(C)  <-- 공유되는
    / \   / \                    / \   / \      부분 문제
  (D) (E)(F) (G)               (D) (E) (F) (G)
  (독립적인 구조)               (중복/공유 구조)
```

### 1.3 구현 전략 (Implementation Strategies)

#### 1) 하향식 (Top-Down): 메모이제이션 (Memoization)
재귀 호출을 사용하며, 계산된 결과를 메모리에 기록하여 중복 계산을 피하는 방식입니다. 필요한 부분 문제만 해결한다는 장점이 있습니다.

#### 2) 상향식 (Bottom-Up): 타뷸레이션 (Tabulation)
가장 작은 부분 문제부터 순서대로 테이블을 채워나가는 반복문 기반의 방식입니다. 시스템 스택 오버헤드가 없으며 모든 부분 문제를 체계적으로 해결합니다.

**[구현 방식 비교]**
```python
# 1. Top-Down (Memoization)
def fib_top_down(n, memo):
    if n \le 2: return 1
    if memo[n] != 0: return memo[n]
    memo[n] = fib_top_down(n-1, memo) + fib_top_down(n-2, memo)
    return memo[n]

# 2. Bottom-Up (Tabulation)
def fib_bottom_up(n):
    table = [0] * (n + 1)
    table[1] = table[2] = 1
    for i in range(3, n + 1):
        table[i] = table[i-1] + table[i-2]
    return table[n]
```
