# Week 06 — 동적 계획법 (Dynamic Programming)

이 문서는 큰 문제를 작은 부분 문제로 나누어 해결하고, 그 결과를 저장하여 재사용하는 **동적 계획법 (Dynamic Programming)** 의 원리, 설계 전략, 그리고 주요 사례(LCS, 배낭 문제, 모든 쌍 최단 경로 등)를 상세히 다룹니다.

---

## 📝 목차
1. [동적 계획법(Dynamic Programming) 개요](#intro)
2. [기초 DP 사례](#basic)
3. [최장 공통 부분 수열 (LCS)](#lcs)
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

---

<a id="basic"></a>
## 2. 기초 DP 사례 (#basic)

가장 직관적인 예시를 통해 동적 계획법이 어떻게 지수 시간의 문제를 선형 시간으로 단축시키는지 살펴봅니다.

### 2.1 피보나치 수열 (Fibonacci Numbers)
정의:  $f(n) = f(n-1) + f(n-2)$  (단,  $f(1) = f(2) = 1$ )

- **단순 재귀의 한계**: 동일한 값을 구하기 위해 수많은 중복 호출이 발생합니다. 예를 들어  $f(7)$  을 구할 때  $f(3)$  은 5번이나 계산됩니다. 시간 복잡도는  $O(2^{n})$  으로 매우 비효율적입니다.
- **DP 해결책**: 이미 계산된 값을 배열에 저장(Tabulation)하여 단 한 번씩만 계산합니다. 시간 복잡도는  $O(n)$  으로 획기적으로 줄어듭니다.

**[실행 과정 (Tabulation)]**

|  $i$  | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
|  $f(i)$  | 1 | 1 | 2 | 3 | 5 | 8 | **13** |

---

### 2.2 행렬 경로 문제 (Matrix Path)
$n \times n$  격자의 각 칸에 양수가 적혀 있을 때, 좌측 상단  $(1, 1)$  에서 우측 하단  $(n, n)$  까지 **이동 경로 상의 숫자 합을 최대화**하는 문제입니다. (단, 오른쪽 또는 아래로만 이동 가능)

- **점화식 도출**:  $(i, j)$  칸에 도달하는 최적의 합은 "위쪽 칸에서의 최적해"와 "왼쪽 칸에서의 최적해" 중 큰 값에 현재 칸의 숫자를 더한 것과 같습니다.

$$
c[i, j] = m_{ij} + \max(c[i-1, j],\ c[i, j-1])
$$

- **경계 조건**:  $c[i, 0] = c[0, j] = 0$  (계산을 위해 가상의 0번째 행/열을 둡니다.)
- **시간 복잡도**: 전체 칸의 개수인  $O(n^{2})$  에 비례합니다.

**[작동 로직 (Python)]**
```python
def matrix_path(m, n):
    c = [[0] * (n + 1) for _ in range(n + 1)]
    for i in range(1, n + 1):
        for j in range(1, n + 1):
            c[i][j] = m[i-1][j-1] + max(c[i-1][j], c[i][j-1])
    return c[n][n]
```

---

<a id="lcs"></a>
## 3. 최장 공통 부분 수열 (LCS) (#lcs)

**최장 공통 부분 수열 (Longest Common Subsequence, LCS)** 은 두 수열이 주어졌을 때, 두 수열 모두의 부분 수열이 되는 수열 중 가장 긴 것을 찾는 문제입니다. (연속적이지 않아도 됨)

### 3.1 점화식 (Recurrence Relation)
두 문자열  $X = \langle x\_{1}, x\_{2}, \dots, x\_{m} \rangle$  와  $Y = \langle y\_{1}, y\_{2}, \dots, y\_{n} \rangle$  에 대하여,  $c[i, j]$  를  $X\_{i}$  와  $Y\_{j}$  의 LCS 길이라 정의합니다.

$$
c[i, j] = \begin{cases}
0 & \text{if } i=0 \text{ or } j=0 \\
c[i-1, j-1] + 1 & \text{if } i, j \gt 0 \text{ and } x_{i} = y_{j} \\
\max(c[i-1, j], c[i, j-1]) & \text{if } i, j \gt 0 \text{ and } x_{i} \neq y_{j}
\end{cases}
$$

- **핵심 로직**:
    - 마지막 문자가 서로 같으면: 해당 문자를 제외한 앞부분의 LCS 길이에 1을 더합니다.
    - 마지막 문자가 서로 다르면: 한쪽 문자를 각각 제외해보고 더 큰 값을 선택합니다.

### 3.2 작동 과정 및 역추적 (Traceback)
DP 테이블  $c[m, n]$  을 모두 채운 후, 맨 오른쪽 아래 칸에서 시작하여 다음 규칙에 따라 거꾸로 올라가며 실제 LCS 문자열을 재구성합니다.

**[역추적 3원칙]**
1.  **Case 1 ( $x_{i} = y_{j}$ )**: 두 문자가 같으면 해당 문자를 LCS에 추가하고, 대각선 왼쪽 위  $c[i-1, j-1]$  로 이동합니다.
2.  **Case 2 ( $c[i-1, j] \ge c[i, j-1]$ )**: 두 문자가 다르고 위쪽 칸의 값이 크거나 같으면 위쪽  $c[i-1, j]$  로 이동합니다.
3.  **Case 3 ( $c[i-1, j] \lt c[i, j-1]$ )**: 두 문자가 다르고 왼쪽 칸의 값이 더 크면 왼쪽  $c[i, j-1]$  로 이동합니다.

**[LCS 테이블 및 역추적 시각화 (X: ABCB, Y: BDCA)]**

|  $c$  | 0 | B | D | C | A |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **0** | 0 | 0 | 0 | 0 | 0 |
| **A** | 0 | 0 | 0 | 0 | **1(↖A)** |
| **B** | 0 | **1(↖B)** | 1(←) | 1(←) | 1 |
| **C** | 0 | 1(↑) | 1 | **2(↖C)** | 2(←) |
| **B** | 0 | 1 | 1 | 2(↑) | 2 |

- **복잡도**: 테이블 구축에  $\Theta(mn)$ , 역추적에  $O(m+n)$  의 시간이 소요됩니다.

**[LCS 실제 문자열 추출 코드 (Python)]**
```python
def get_lcs_string(x, y, c):
    lcs_str = []
    i, j = len(x), len(y)
    while i \gt 0 and j \gt 0:
        if x[i-1] == y[j-1]:
            lcs_str.append(x[i-1])
            i -= 1; j -= 1
        elif c[i-1][j] \ge c[i][j-1]:
            i -= 1
        else:
            j -= 1
    return "".join(reversed(lcs_str))
```

