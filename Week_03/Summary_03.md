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
전체 원소 중 가장 큰(또는 작은) 값을 찾아 정해진 위치로 보내는 방식입니다.

**[작동 원리 시각화]**
```text
[ 3 | 1 | 4 | 2 ]  (Step 0: 초기 상태)
  ^---^ (1과 3 비교 후 1이 최소임을 확인)
[ 1 | 3 | 4 | 2 ]  (Step 1: 최소값 1을 맨 앞으로 이동, 정렬 부분: [1])
      ^-------^ (2와 3 비교 후 2가 최소임을 확인)
[ 1 | 2 | 4 | 3 ]  (Step 2: 다음 최소값 2를 이동, 정렬 부분: [1, 2])
```

- **개념**: 미정렬 부분에서 최대값을 찾아 맨 뒤(또는 최소값을 찾아 맨 앞)의 원소와 교체합니다.
- **복잡도 분석**: 데이터의 구성과 관계없이 항상  $(n-1) + (n-2) + \dots + 1$  회의 비교를 수행하므로  $\Theta(n^{2})$  입니다.

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
- **복잡도 분석**: 최악과 평균의 경우  $\Theta(n^{2})$  이며, 교환이 발생하지 않을 때 조기 종료하도록 최적화하면 최선의 경우  $\Theta(n)$  이 가능합니다.

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
- **복잡도 분석**:
    - **최악 (역순 정렬)**:  $\Theta(n^{2})$ 
    - **최선 (이미 정렬)**: 비교 한 번씩만 수행하므로  ** $\Theta(n)$ **. 거의 정렬된 데이터에서 매우 빠릅니다.

### 3.4 기초 정렬의 재귀적 구조 비교
모든 기초 정렬은 한 단계를 수행할 때마다 정렬해야 할 문제의 크기가  $n$  에서  $n-1$  로 줄어듭니다.

| 알고리즘 | 작업 시점 | 주요 작업 ($ \Theta(n) $) |
| :--- | :--- | :--- |
| **선택 정렬** | 재귀 호출 전 | 최대값 찾기 및 교환 |
| **버블 정렬** | 재귀 호출 전 | 인접 비교를 통한 최대값 이동 |
| **삽입 정렬** | 재귀 호출 후 | 정렬된 부분에 원소 삽입 |

```python
# 삽입 정렬의 재귀적 구조 예시
def recursive_insertion_sort(arr, n):
    if n <= 1: return
    recursive_insertion_sort(arr, n - 1) # (n-1)개를 먼저 정렬
    insert(arr, n) # 마지막 원소를 정렬된 부분에 삽입 (작업 시점: 후)
```

---

<a id="advanced"></a>
## 4. 고급 정렬 알고리즘 ( $O(n \log\_{2}{n})$ ) (#advanced)

### 4.1 병합 정렬 (Merge Sort)
반으로 나누고(Divide), 정렬하고(Conquer), 합치는(Combine) 분할 정복 알고리즘입니다.
- **점화식**: $T(n) = 2T(n/2) + \Theta(n) \implies \Theta(n \log\_{2}{n})$
- **특징**: 데이터 상태에 관계없이 항상 일정한 성능을 보장하지만, 병합을 위한 **추가 메모리 ($O(n)$)** 가 필요합니다.

```python
def merge_sort(arr, p, r):
    if p < r:
        q = (p + r) // 2
        merge_sort(arr, p, q)
        merge_sort(arr, q + 1, r)
        merge(arr, p, q, r)

def merge(arr, p, q, r):
    i, j, tmp = p, q + 1, []
    while i <= q and j <= r:
        if arr[i] <= arr[j]: tmp.append(arr[i]); i += 1
        else: tmp.append(arr[j]); j += 1
    while i <= q: tmp.append(arr[i]); i += 1
    while j <= r: tmp.append(arr[j]); j += 1
    for k in range(len(tmp)): arr[p + k] = tmp[k]
```

### 4.2 퀵 정렬 (Quick Sort)
피벗(Pivot)을 기준으로 작은 값은 왼쪽, 큰 값은 오른쪽으로 나눕니다.
- **복잡도**: 평균 **$\Theta(n \log\_{2}{n})$**. 최악(피벗이 매번 최소/최대일 때) **$O(n^{2})$**.
- **In-place Partition**: 추가 리스트 없이 포인터 `i`, `j` 만 사용하여 자리를 바꿉니다.

```python
def quick_sort(arr, p, r):
    if p < r:
        q = partition(arr, p, r)
        quick_sort(arr, p, q - 1)
        quick_sort(arr, q + 1, r)

def partition(arr, p, r):
    pivot, i = arr[r], p - 1
    for j in range(p, r):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i + 1], arr[r] = arr[r], arr[i + 1]
    return i + 1
```

### 4.3 힙 정렬 (Heap Sort)
배열을 힙으로 만든 뒤 루트를 하나씩 추출합니다.
- **단계**: 힙 구축 ( `buildHeap` , $O(n)$ ) $\to$ 루트 추출 및 재정비 ( $n \times \log n$ ).
- **특징**: 추가 메모리가 필요 없는 **제자리 정렬 (In-place)** 이면서 항상 $O(n \log n)$ 을 유지합니다.

```python
def heap_sort(arr):
    n = len(arr)
    for i in range(n // 2 - 1, -1, -1): heapify(arr, n, i) # buildHeap
    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0] # swap
        heapify(arr, i, 0)

def heapify(arr, n, i):
    smallest = i
    l, r = 2 * i + 1, 2 * i + 2
    if l < n and arr[l] < arr[smallest]: smallest = l
    if r < n and arr[r] < arr[smallest]: smallest = r
    if smallest != i:
        arr[i], arr[smallest] = arr[smallest], arr[i]
        heapify(arr, n, smallest)
```

---

<a id="linear"></a>
## 5. 선형 시간 정렬 ( $O(n)$ ) (#linear)

비교를 하지 않고 데이터의 특성을 이용합니다.

### 5.1 계수 정렬 (Counting Sort)
각 숫자의 빈도수를 세어 위치를 결정합니다.
- **조건**: 데이터 범위 $k$ 가 $O(n)$ 일 때.
- **안정성**: 뒤에서부터 배치함으로써 안정 정렬을 유지합니다.

### 5.2 기수 정렬 (Radix Sort)
가장 낮은 자릿수부터 **안정 정렬** 을 반복하여 전체를 정렬합니다.
- **복잡도**: 자릿수 $k$ 가 상수일 때 **$\Theta(n)$**.

---

<a id="comparison"></a>
## 6. 정렬 알고리즘 성능 비교 (#comparison)

| 알고리즘 | 최선 | 평균 | 최악 | 공간 | 안정성 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **선택 정렬** | $O(n^{2})$ | $O(n^{2})$ | $O(n^{2})$ | $O(1)$ | No |
| **버블 정렬** | $O(n)$ | $O(n^{2})$ | $O(n^{2})$ | $O(1)$ | Yes |
| **삽입 정렬** | **$O(n)$** | $O(n^{2})$ | $O(n^{2})$ | $O(1)$ | Yes |
| **병합 정렬** | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | **$O(n)$** | Yes |
| **퀵 정렬** | $O(n \log n)$ | $O(n \log n)$ | **$O(n^{2})$** | $O(\log n)$ | No |
| **힙 정렬** | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(1)$ | No |
| **계수 정렬** | $O(n)$ | $O(n)$ | $O(n)$ | $O(k)$ | Yes |

---

## 💡 요약 및 시사점
1. 정렬 알고리즘은 **시간-공간-안정성** 사이의 트레이드오프(Trade-off)를 이해하는 것이 핵심입니다.
2. 데이터가 거의 정렬된 경우 **삽입 정렬**이, 추가 메모리 없이 최악을 방지하려면 **힙 정렬**이 유리합니다.
3. 비교 기반 정렬의 물리적 한계($n \log n$)를 극복하기 위해서는 데이터의 도메인 지식(범위, 자릿수)을 활용해야 합니다.
