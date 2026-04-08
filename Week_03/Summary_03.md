# Week 03 — 자료구조 및 기초 정렬 알고리즘 (Summary)

이 문서는 알고리즘의 기초가 되는 자료구조(리스트, 스택, 큐, 힙)와 다양한 정렬 알고리즘의 작동 원리, 수학적 분석, 그리고 파이썬 구현을 총망라합니다. 3주차 강의 자료의 모든 세부 내용을 누락 없이 포함하고 있습니다.

---

## 📝 목차
1. [기본 자료구조 리뷰 (DS Review)](#ds)
2. [정렬 알고리즘 개요 (Sorting Landscape)](#landscape)
3. [기초 정렬 알고리즘 (O(n^2))](#elementary)
4. [고급 정렬 알고리즘 (O(n log n))](#advanced)
5. [선형 시간 정렬 (O(n))](#linear)
6. [정렬 알고리즘 성능 비교](#comparison)

---

<a id="ds"></a>
## 1. 기본 자료구조 리뷰 (DS Review) (#ds)

알고리즘의 효율적인 설계와 구현을 위해서는 데이터를 조직화하는 기본 자료구조에 대한 깊은 이해가 필수적입니다.

### 1.1 연결 리스트 (Linked List)
연결 리스트는 데이터와 다음 노드를 가리키는 포인터를 포함하는 **노드 (Node)** 들의 선형 시퀀스입니다.

**[구조 시각화]**
```text
[ Data | Next ] ---> [ Data | Next ] ---> [ Data | None ]
(Head Node)          (Intermediate)       (Tail Node)
```

- **구조와 특징**: 각 노드는 데이터 필드와 링크 필드로 구성되며, 메모리 상에 비연속적으로 존재할 수 있어 삽입과 삭제가 유연합니다.
- **성능 분석**: 특정 위치의 원소를 찾거나 탐색하는 연산은 첫 노드부터 순차적으로 방문해야 하므로 최악의 경우  $O(n)$  의 시간이 소요됩니다.
- **핵심 구조**:
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None # 다음 노드를 가리키는 참조
```

### 1.2 스택 (Stack)
스택은 한쪽 끝에서만 데이터의 삽입과 삭제가 일어나는 **LIFO (Last-In, First-Out, 후입선출)** 방식의 자료구조입니다.

**[구조 시각화]**
```text
|   Data C   |  <- Top (Last In, First Out)
|------------|
|   Data B   |
|------------|
|   Data A   |
+------------+
```

- **주요 연산**: 데이터를 쌓는  `push()`  와 가장 위의 데이터를 꺼내는  `pop()`  연산으로 구성됩니다.
- **응용 사례**: 함수 호출의 시스템 스택(Recursion 관리), 수식 계산(역폴란드 표기법), 웹 브라우저의 뒤로 가기 기능 등에서 사용됩니다.
- **간결한 구현 예시**:
```python
stack = []
stack.append(item) # push: O(1)
stack.pop()        # pop: O(1)
```

### 1.3 큐 (Queue)
큐는 리스트의 한쪽 끝에서는 삽입이, 반대쪽 끝에서는 삭제가 일어나는 **FIFO (First-In, First-Out, 선입선출)** 방식의 자료구조입니다.

**[구조 시각화]**
```text
Exit (Front)                       Enter (Rear)
     <--- [ Data A ] [ Data B ] [ Data C ] <---
          (First In)            (Last In)
```

- **주요 연산**: 뒤쪽(Rear)에 데이터를 추가하는  `enqueue()`  와 앞쪽(Front)에서 데이터를 꺼내는  `dequeue()`  연산이 핵심입니다.
- **응용 사례**: 운영체제의 프로세스 스케줄링, 네트워크 패킷 버퍼링, 너비 우선 탐색(BFS)의 방문 노드 관리 등에 필수적입니다.
- **간결한 구현 예시**:
```python
from collections import deque
queue = deque()
queue.append(item) # enqueue: O(1)
queue.popleft()    # dequeue: O(1)
```

### 1.4 힙 (Heap)
힙은 **완전 이진 트리 (Complete Binary Tree)** 의 일종으로, 부모와 자식 노드 간의 키값 대소 관계를 유지하는 자료구조입니다.

**[구조 시각화 (Min Heap)]**
```text
        [ 10 ]             (Root / Minimum)
       /      \
    [ 20 ]   [ 30 ]        (Parent >= Child)
    /    \
 [ 40 ] [ 50 ]             (Complete Binary Tree)
```

- **힙 속성 (Heap Property)**:
    - **최대 힙 (Max Heap)**: 모든 노드에 대해  $\text{key(parent)} \ge \text{key(child)}$  를 만족합니다.
    - **최소 힙 (Min Heap)**: 모든 노드에 대해  $\text{key(parent)} \le \text{key(child)}$  를 만족합니다.
- **배열을 이용한 구현**: 힙은 포인터 없이 배열만으로 효율적으로 저장 가능합니다. (1번 인덱스 시작 기준)
    -  $A[i]$  의 왼쪽 자식:  $A[2i]$ 
    -  $A[i]$  의 오른쪽 자식:  $A[2i + 1]$ 
    -  $A[i]$  의 부모 노드:  $A[\lfloor i/2 \rfloor]$ 
- **복잡도**: 삽입과 삭제(추출) 시 트리의 높이에 비례하는  $O(\log\_{2}{n})$  의 시간을 보장합니다.
- **핵심 연산 (Python 내장 라이브러리)**:
```python
import heapq
heap = []
heapq.heappush(heap, item) # 최소 힙 삽입: O(log n)
min_val = heapq.heappop(heap) # 최솟값 추출: O(log n)
```

---

<a id="landscape"></a>
## 2. 정렬 알고리즘 개요 (Sorting Landscape) (#landscape)

**정렬 (Sorting)** 은 데이터를 특정 기준(오름차순 또는 내림차순)에 따라 재배열하는 과정입니다. 알고리즘의 효율성은 주로 비교 횟수와 이동 횟수로 측정됩니다.

### 2.1 정렬 알고리즘의 분류
대부분의 정렬 알고리즘은 성능에 따라 세 가지 범주로 나뉩니다.

| 범주 | 알고리즘 | 시간 복잡도 |
| :--- | :--- | :--- |
| **기초 정렬 (Elementary)** | 선택, 버블, 삽입 정렬 |  $O(n^{2})$  |
| **고급 정렬 (Advanced)** | 병합, 퀵, 힙 정렬 |  $O(n \log\_{2}{n})$  |
| **특수 정렬 (Linear-time)** | 계수, 기수 정렬 |  $O(n)$  |

### 2.2 알고리즘을 바라보는 두 가지 관점
1.  **흐름 중심 (Flow-based)**: 알고리즘이 단계별로 어떻게 실행되는지 그 과정을 추적합니다.
2.  **관계 중심 (Relational)**: 각 단계에서 데이터의 상태가 어떻게 변하는지, 어떤 성질(Invariant)이 유지되는지 관찰하여 더 깊은 통찰을 얻습니다.

---

<a id="elementary"></a>
## 3. 기초 정렬 알고리즘 (O(n^2)) (#elementary)

이 파트의 정렬 알고리즘들은 모두  $T(n) = T(n-1) + \Theta(n)$  이라는 공통된 재귀적 구조를 가집니다.

### 3.1 선택 정렬 (Selection Sort)
전체 원소 중 최대값을 찾아 정해진 위치(맨 뒤)로 보내는 방식입니다.

**[작동 원리 시각화]**
```text
[ 3 | 1 | 4 | 2 ]  (Step 0: 초기 상태)
  ^---^ (1과 3 비교 후 1이 최소임을 확인)
[ 1 | 3 | 4 | 2 ]  (Step 1: 최소값 1을 맨 앞으로 이동, 정렬 부분: [1])
      ^-------^ (2와 3 비교 후 2가 최소임을 확인)
[ 1 | 2 | 4 | 3 ]  (Step 2: 다음 최소값 2를 이동, 정렬 부분: [1, 2])
```

- **개념**: 미정렬 부분에서 최대값을 찾아 맨 뒤의 원소와 교체합니다.
- **복잡도 분석**: 데이터의 구성과 관계없이 항상  $(n-1) + (n-2) + \dots + 1$  회의 비교를 수행하므로  $\Theta(n^{2})$  입니다.
- **간결한 구현**:
```python
def selection_sort(arr):
    for last in range(len(arr) - 1, 0, -1):
        max_idx = 0
        for i in range(1, last + 1):
            if arr[i] > arr[max_idx]: max_idx = i
        arr[max_idx], arr[last] = arr[last], arr[max_idx]
```

### 3.2 버블 정렬 (Bubble Sort)
인접한 두 원소를 비교하여 정렬 순서가 맞지 않으면 서로 교환하는 방식입니다.

**[작동 원리 시각화]**
```text
[ 3 | 1 | 4 | 2 ] -> [ 1 | 3 | 4 | 2 ] (3, 1 교환)
[ 1 | 3 | 4 | 2 ] -> [ 1 | 3 | 4 | 2 ] (3, 4 유지)
[ 1 | 3 | 4 | 2 ] -> [ 1 | 3 | 2 | 4 ] (4, 2 교환)
결과: 최대값 4가 맨 뒤로 "거품처럼" 이동함.
```

- **개념**: 한 번의 패스(Pass)를 거치면 가장 큰 값이 배열의 끝으로 이동합니다.
- **복잡도 분석**: 최악과 평균의 경우  $\Theta(n^{2})$  이며, 최선의 경우  $\Theta(n)$  입니다.
- **간결한 구현**:
```python
def bubble_sort(arr):
    n = len(arr)
    for last in range(n - 1, 0, -1):
        for i in range(last):
            if arr[i] > arr[i + 1]:
                arr[i], arr[i + 1] = arr[i + 1], arr[i]
```

### 3.3 삽입 정렬 (Insertion Sort)
이미 정렬된 부분에 새로운 원소를 적절한 위치에 '삽입'하는 방식입니다.

**[작동 원리 시각화]**
```text
[ 1 | 3 | 4 | 2 ]  (Key: 2)
[ 1 | 3 | 4 | 4 ]  (2보다 큰 4를 오른쪽으로 시프트)
[ 1 | 3 | 3 | 4 ]  (2보다 큰 3을 오른쪽으로 시프트)
[ 1 | 2 | 3 | 4 ]  (적절한 위치에 Key: 2 삽입 완료)
```

- **귀납적 정당성 (Inductive Proof)**:
    - **기본 단계 (Base Case)**: 원소가 하나인 배열  $A[1]$  은 이미 정렬되어 있습니다.
    - **귀납 가정**:  $A[1 \dots k]$  가 정렬되어 있다고 가정합니다.
    - **귀납 단계**:  $A[k+1]$  을 정렬된  $A[1 \dots k]$  의 적절한 위치에 삽입하면  $A[1 \dots k+1]$  도 정렬된 상태가 됩니다.
- **복잡도 분석**: 최악은  $\Theta(n^{2})$ , 최선(이미 정렬)은  $\Theta(n)$  입니다.
- **간결한 구현**:
```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key, j = arr[i], i - 1
        while j \ge 0 and arr[j] \gt key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
```

### 3.4 기초 정렬의 재귀적 구조 비교
모든 기초 정렬은 한 단계를 수행할 때마다 문제의 크기가  $n$  에서  $n-1$  로 줄어듭니다.

| 알고리즘 | 작업 시점 | 주요 작업 ( $\Theta(n)$ ) |
| :--- | :--- | :--- |
| **선택 정렬** | 재귀 호출 전 | 최대값 찾기 및 교환 |
| **버블 정렬** | 재귀 호출 전 | 인접 비교를 통한 최대값 이동 |
| **삽입 정렬** | 재귀 호출 후 | 정렬된 부분에 원소 삽입 |

```python
# 삽입 정렬의 재귀적 구조 예시
def recursive_insertion_sort(arr, n):
    if n \le 1: return
    recursive_insertion_sort(arr, n - 1) # (n-1)개를 먼저 정렬
    insert(arr, n) # 마지막 원소를 정렬된 부분에 삽입
```

---

<a id="advanced"></a>
## 4. 고급 정렬 알고리즘 (O(n log n)) (#advanced)

고급 정렬 알고리즘은 대부분 **분할 정복 (Divide and Conquer)** 패러다임을 사용하며, 비교 기반 정렬의 하한선에 근접한  $O(n \log\_{2}{n})$  의 시간 복잡도를 달성합니다.

### 4.1 병합 정렬 (Merge Sort)
문제를 더 이상 나눌 수 없을 때까지 반으로 나누고, 정렬하며 다시 합치는 방식입니다.

**[분할 정복 과정 시각화]**
```text
      [ 38, 27, 43, 3, 9, 82, 10 ]      (Divide)
      /             \
[ 38, 27, 43 ]     [ 3, 9, 82, 10 ]     (Split)
    /      \          /       \
[ 38 ] [ 27, 43 ] [ 3, 9 ] [ 82, 10 ]   (Recursive Base)
    \      /          \       /
[ 27, 38, 43 ]     [ 3, 9, 10, 82 ]     (Merge/Conquer)
      \             /
      [ 3, 9, 10, 27, 38, 43, 82 ]      (Combine)
```

- **특징**: 데이터의 초기 상태와 관계없이 항상  $\Theta(n \log\_{2}{n})$  을 보장하지만, 병합 과정에서  ** $O(n)$  의 추가 메모리** 가 필요합니다.
- **점화식**:  $T(n) = 2T(n/2) + \Theta(n)$ 
- **간결한 구현**:
```python
def merge_sort(arr):
    if len(arr) \le 1: return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    while i \lt len(left) and j \lt len(right):
        if left[i] \le right[j]:
            result.append(left[i]); i += 1
        else:
            result.append(right[j]); j += 1
    result.extend(left[i:]); result.extend(right[j:])
    return result
```

### 4.2 퀵 정렬 (Quick Sort)
기준 원소인 **피벗 (Pivot)** 을 선택하여 이를 기준으로 작은 값과 큰 값을 분할하는 방식입니다.

**[파티션 과정 시각화 (Pivot: 15)]**
```text
[ 31, 8, 48, 73, 11, 3, 20, 15 ] (Pivot: 15)
  ^--- i (작은 값들의 경계)
[ 8, 11, 3, | 15 |, 31, 48, 20, 73 ]
 (Small)    (Pivot)     (Large)
```

- **복잡도 분석**:
    - **평균**: 피벗이 균형 있게 나눌 경우  $\Theta(n \log\_{2}{n})$ .
    - **최악**: 이미 정렬된 배열에서 피벗을 끝 값으로 선택할 경우  $O(n^{2})$ .
- **핵심 특징**: 추가 메모리를 거의 쓰지 않는 **제자리 정렬 (In-place)** 이며, 평균적으로 가장 빠릅니다.
- **간결한 구현**:
```python
def quick_sort(arr):
    if len(arr) \le 1: return arr
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x \lt pivot]
    mid = [x for x in arr if x == pivot]
    right = [x for x in arr if x \gt pivot]
    return quick_sort(left) + mid + quick_sort(right)
```

### 4.3 힙 정렬 (Heap Sort)
배열을 **최대 힙 (Max Heap)** 으로 구성한 뒤, 루트(최대값)를 하나씩 꺼내 배열의 뒤부터 채우는 방식입니다.

**[작동 원리 시각화]**
```text
1. Build-Heap (배열을 힙으로)    2. Extract-Max (루트 추출 및 교체)
      [ 9 ] (Root)               [ 3 ] (New Root) <--- [ 9 ] (Sorted)
     /     \                    /     \
  [ 7 ]   [ 8 ]     --->     [ 7 ]   [ 8 ]      --->  Heapify...
  /   \                      /
[ 3 ] [ 6 ]                [ 6 ]
```

- **상세 복잡도 분석**:
    1.  **Build-Heap Phase**: 무작위 배열을 힙으로 만드는 과정은 단순 계산 시  $O(n \log\_{2}{n})$  으로 보이나, 실제로는 각 노드의 높이에 비례하는 작업의 합이므로  **$O(n)$**  에 가능합니다.
    2.  **Sorting Phase**: 루트를 추출하고 남은 원소들에 대해  `heapify`  를 수행하는 과정이  $n - 1$  번 반복됩니다. 각  `heapify`  는 트리의 높이인  $O(\log\_{2}{n})$  이 소요됩니다.
- **최종 복잡도**: 모든 경우에 대해  $\Theta(n + n \log\_{2}{n}) = \Theta(n \log\_{2}{n})$  을 보장합니다.
- **간결한 구현**:
```python
import heapq

def heap_sort(arr):
    h = []
    for value in arr:
        heapq.heappush(h, value) # Min-Heap 삽입 (O(log n))
    return [heapq.heappop(h) for _ in range(len(h))] # 추출 (O(n log n))
```

---

<a id="linear"></a>
## 5. 선형 시간 정렬 (O(n)) (#linear)
비교 기반 정렬의 하한인  $\Omega(n \log\_{2}{n})$  을 넘어서기 위해, 데이터의 특성(자릿수, 범위)을 이용합니다.

### 5.1 계수 정렬 (Counting Sort)
원소의 빈도수를 직접 세어 위치를 결정하는 정렬 방식입니다.

**[작동 원리 시각화]**
```text
Input:  [ 2, 1, 2, 0 ]  (n=4, k=2)
Count:  [ 0:1, 1:1, 2:2 ] (각 숫자의 개수 기록)
Prefix: [ 0:1, 1:2, 2:4 ] (누적 합: 해당 숫자가 끝날 위치)
Output: [ 0, 1, 2, 2 ]   (위치에 맞게 데이터 배치)
```

- **상세 복잡도 및 제약**:
    - **시간 복잡도**: 데이터 개수  $n$  과 데이터의 범위  $k$  에 대해  $\Theta(n + k)$  가 소요됩니다.
    - **공간 복잡도**: 범위  $k$  만큼의 카운트 배열이 필요하므로  $O(k)$  의 추가 메모리가 필요합니다.
- **주요 특징**: 데이터의 범위  $k$  가  $O(n)$  일 때만 선형 시간 정렬로서 의미가 있습니다. 또한, 누적 합을 이용하여 뒤에서부터 배치하면 **안정 정렬 (Stable Sort)** 을 유지할 수 있습니다.
- **간결한 구현**:
```python
def counting_sort(arr, k): # k: 데이터의 최대값
    count = [0] * (k + 1)
    for x in arr: count[x] += 1
    result = []
    for i in range(k + 1):
        result.extend([i] * count[i]) # O(n + k)
    return result

def stable_counting_sort(arr, exp):
    # 각 자릿수 정렬을 위한 안정 계수 정렬 구현 생략 (핵심 개념 위주)
    pass
```

### 5.2 기수 정렬 (Radix Sort)
낮은 자릿수부터 차례대로 **안정 정렬 (Stable Sort)** 을 수행하여 전체를 정렬합니다.

**[작동 원리 시각화]**
```text
[ 170, 045, 075, 090 ] (초기 데이터, d=3)
Step 1: 1의 자리 정렬 -> [ 170, 090, 045, 075 ] (안정 정렬 유지)
Step 2: 10의 자리 정렬 -> [ 045, 170, 075, 090 ]
Step 3: 100의 자리 정렬 -> [ 045, 075, 090, 170 ] (완료)
```

- **상세 복잡도 분석**:
    - 데이터의 개수를  $n$ , 자릿수를  $d$ , 각 자릿수에서 사용하는 기수(Radix)의 범위를  $k$  라고 할 때, 총 복잡도는  $\Theta(d(n + k))$  입니다.
    - 만약  $d$  와  $k$  가  $n$  에 비해 매우 작은 상수라면  $\Theta(n)$  에 수렴합니다.
- **핵심 조건**: 각 자릿수별 정렬 단계에서 반드시 **안정 정렬 (Stable Sort)** 을 사용해야 합니다. 그렇지 않으면 이전 단계의 정렬 결과가 무효화됩니다.
- **간결한 구현**:
```python
def radix_sort(arr):
    max_val = max(arr)
    exp = 1 # 1, 10, 100... 자릿수
    while max_val // exp \gt 0:
        # 각 자릿수에 대해 계수 정렬(안정 정렬) 수행
        arr = stable_counting_sort(arr, exp)
        exp *= 10
    return arr
```

---

<a id="comparison"></a>
## 6. 정렬 알고리즘 성능 비교 (#comparison)

| 알고리즘 | 최선 | 평균 | 최악 | 공간 | 안정성 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **선택 정렬** |  $O(n^{2})$  |  $O(n^{2})$  |  $O(n^{2})$  |  $O(1)$  | No |
| **버블 정렬** |  $O(n)$  |  $O(n^{2})$  |  $O(n^{2})$  |  $O(1)$  | Yes |
| **삽입 정렬** |  ** $O(n)$ ** |  $O(n^{2})$  |  $O(n^{2})$  |  $O(1)$  | Yes |
| **병합 정렬** |  $O(n \log\_{2}{n})$  |  $O(n \log\_{2}{n})$  |  $O(n \log\_{2}{n})$  |  ** $O(n)$ ** | Yes |
| **퀵 정렬** |  $O(n \log\_{2}{n})$  |  $O(n \log\_{2}{n})$  |  ** $O(n^{2})$ ** |  $O(\log\_{2}{n})$  | No |
| **힙 정렬** |  $O(n \log\_{2}{n})$  |  $O(n \log\_{2}{n})$  |  $O(n \log\_{2}{n})$  |  $O(1)$  | No |
| **계수 정렬** |  $O(n + k)$  |  $O(n + k)$  |  $O(n + k)$  |  $O(k)$  | Yes |
| **기수 정렬** |  $O(nk)$  |  $O(nk)$  |  $O(nk)$  |  $O(n + k)$  | Yes |

---

## 💡 요약 및 시사점
1.  **트레이드오프**: 병합 정렬은 성능이 일정하지만 공간을 더 사용하고, 퀵 정렬은 평균적으로 빠르지만 최악의 경우가 존재합니다.
2.  **안정성 (Stability)**: 값이 같은 원소의 순서가 중요한 경우 삽입, 병합 정렬 등을 고려해야 합니다.
3.  **데이터 특성 활용**: 데이터가 거의 정렬된 경우 **삽입 정렬** 이, 범위가 제한적인 경우 **계수 정렬** 이 가장 효율적입니다.
