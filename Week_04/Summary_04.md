# Week 04 — 분할 정복 알고리즘 (Divide and Conquer)

이 문서는 복잡한 문제를 더 작은 단위로 쪼개어 해결하는 **분할 정복 (Divide and Conquer)** 패러다임의 원리, 설계 전략, 그리고 주요 알고리즘(합병 정렬, 퀵 정렬, 선택 문제 등)을 상세히 다룹니다.

---

## 📝 목차
1. [분할 정복(Divide and Conquer) 개요](#intro)
2. [재귀 관계식과 마스터 정리 (TBA)](#analysis)
3. [대표적인 분할 정복 알고리즘 (TBA)](#algorithms)
4. [고급 분할 정복 알고리즘 (TBA)](#advanced)
5. [분할 정복의 한계 (TBA)](#limitations)

---

<a id="intro"></a>
## 1. 분할 정복(Divide and Conquer) 개요 (#intro)

**분할 정복 (Divide and Conquer)** 은 해결하기 어려운 큰 문제를 직접 해결하는 대신, 이를 작은 문제들로 나누어 각각 해결한 뒤 그 결과들을 합쳐서 원래 문제를 해결하는 알고리즘 설계 기법입니다.

### 1.1 작동 원리 (The 3-Step Process)
분할 정복은 전형적으로 다음과 같은 세 단계를 거치는 재귀적 구조를 가집니다.

**[분할 정복 프로세스 시각화]**
```text
      [ Original Problem (Size n) ]
                  |
        +---------+---------+
        | (Divide)          |
        v                   v
[ Subproblem 1 ]    [ Subproblem 2 ]  ... (Size n/b)
        | (Conquer)         |
        v                   v
[ Subsolution 1 ]   [ Subsolution 2 ] ...
        | (Combine)         |
        +---------+---------+
                  |
                  v
       [ Original Solution ]
```

1.  **분할 (Divide)**: 원래 문제를 더 작은 크기의 동일한 유형인 부분 문제들로 나눕니다.
2.  **정복 (Conquer)**: 각 부분 문제를 재귀적으로 해결합니다. 만약 부분 문제의 크기가 충분히 작다면(Base Case), 재귀 호출 없이 직접 해를 구합니다.
3.  **결합 (Combine)**: 부분 문제들의 해를 적절히 합쳐서 원래 문제의 최종 해를 얻습니다.

### 1.2 주요 특징
- **재귀적 구조 (Recursive Structure)**: 문제의 구조가 하위 단계에서도 동일하게 유지되므로 재귀(Recursion)를 사용하여 간결하게 구현할 수 있습니다.
- **하향식 접근 (Top-down Approach)**: 큰 문제에서 시작하여 작은 문제로 내려가며 해결합니다.
- **독립적인 부분 문제**: 분할된 부분 문제들은 서로 겹치지 않고 독립적입니다. 만약 부분 문제가 서로 겹친다면(Overlapping Subproblems), 분할 정복보다는 **동적 계획법 (Dynamic Programming)** 이 더 효율적입니다.

### 1.3 알고리즘 설계 시 고려사항
분할 정복 알고리즘의 효율성은 다음 세 가지 요소의 합으로 결정됩니다.
- 문제를 나누는 비용 (Divide cost)
- 재귀 호출을 통한 해결 비용 (Conquer cost)
- 해를 합치는 비용 (Combine cost)

이를 수식으로 표현하면 다음과 같은 **재귀 관계식 (Recurrence Relation)** 이 됩니다.

$$
T(n) = aT(n/b) + f(n)
$$

-  $a$ : 분할되는 부분 문제의 개수
-  $n/b$ : 각 부분 문제의 크기
-  $f(n)$ : 분할(Divide) 및 결합(Combine)에 소요되는 시간
