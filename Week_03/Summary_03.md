# Week 03 — 자료구조 및 기초 정렬 알고리즘 (Summary)

이 문서는 알고리즘의 기초가 되는 자료구조(리스트, 스택, 큐, 힙)와 다양한 정렬 알고리즘의 작동 원리, 수학적 분석, 그리고 파이썬 구현을 총망라합니다. 3주차 강의 자료의 모든 세부 내용을 누락 없이 포함하고 있습니다.

---

## 📝 목차
1. [기본 자료구조 리뷰 (DS Review)](#ds)
2. [정렬 알고리즘 개요 (Sorting Landscape)](#landscape)
3. [기초 정렬 알고리즘 ( $O(n^{2})$ )](#elementary)
4. [고급 정렬 알고리즘 ( $O(n \log\_{2}{n})$ )](#advanced)
5. [선형 시간 정렬 ( $O(n)$ )](#linear)
6. [정렬 알고리즘 성능 비교](#comparison)

---

<a id="ds"></a>
## 1. 기본 자료구조 리뷰 (DS Review) (#ds)

알고리즘의 효율적인 설계와 구현을 위해서는 데이터를 조직화하는 기본 자료구조에 대한 깊은 이해가 필수적입니다.

### 1.1 연결 리스트 (Linked List)
연결 리스트는 데이터와 다음 노드를 가리키는 포인터를 포함하는 **노드 (Node)** 들의 선형 시퀀스입니다.
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
## 2. 정렬 알고리즘 개요 (#landscape)

정렬은 데이터를 특정 순서(오름차순/내림차순)로 나열하는 과정입니다. 대부분의 알고리즘은 **$O(n \log n)$** 과 **$O(n^{2})$** 사이의 성능을 보입니다.

- **비교 기반 정렬의 하한**: 어떤 비교 기반 정렬도 최악의 경우 **$\Omega(n \log\_{2}{n})$** 보다 빠를 수 없습니다.
- **안정성 (Stability)**: 값이 같은 원소들의 상대적 순서가 유지되는가? (예: 삽입 정렬-Yes, 퀵 정렬-No)

---

<a id="elementary"></a>
## 3. 기초 정렬 알고리즘 ( $O(n^{2})$ ) (#elementary)

이 그룹은 모두 $T(n) = T(n-1) + \Theta(n)$ 이라는 공통적인 재귀 구조를 가집니다.

### 3.1 선택 정렬 (Selection Sort)
미정렬 부분에서 **최솟값** 을 찾아 맨 앞의 원소와 교체하는 방식입니다.
- **수학적 분석**: 비교 횟수는 $(n-1) + (n-2) + \dots + 1 = \frac{n(n-1)}{2}$ 입니다.
- **복잡도**: 최악/평균/최선 모두 **$\Theta(n^{2})$**.

```python
def selection_sort(arr):
    n = len(arr)
    for last in range(n - 1, 0, -1):
        max_idx = 0
        for i in range(1, last + 1):
            if arr[i] > arr[max_idx]: max_idx = i
        arr[max_idx], arr[last] = arr[last], arr[max_idx]
```

### 3.2 버블 정렬 (Bubble Sort)
인접한 두 원소를 비교하여 큰 값을 뒤로 계속 밀어내는 방식입니다. 최댓값이 마치 거품처럼 맨 뒤로 올라갑니다.
- **복잡도**: 최악/평균 **$\Theta(n^{2})$**, (최적화 시) 최선 $O(n)$.

```python
def bubble_sort(arr):
    n = len(arr)
    for last in range(n - 1, 0, -1):
        for i in range(last):
            if arr[i] > arr[i + 1]:
                arr[i], arr[i + 1] = arr[i + 1], arr[i]
```

### 3.3 삽입 정렬 (Insertion Sort)
정렬된 부분에 새로운 원소를 적절한 위치에 '삽입'합니다. 카드 정렬과 유사합니다.
- **귀납적 증명 (Loop Invariant)**: $A[1 \dots i-1]$ 이 정렬되어 있다면, $A[i]$ 를 삽입한 후 $A[1 \dots i]$ 도 정렬됩니다.
- **복잡도**: 최악/평균 $\Theta(n^{2})$, **최선(정렬된 상태) $\Theta(n)$**. 기초 정렬 중 가장 효율적입니다.

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key, j = arr[i], i - 1
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
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
