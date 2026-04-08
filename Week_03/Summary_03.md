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

알고리즘의 효율적인 구현을 위해 데이터의 조직화 방식인 자료구조를 정확히 이해하는 것이 중요합니다.

### 1.1 연결 리스트 (Linked List)
데이터와 다음 노드의 주소를 저장하는 포인터로 구성된 **노드 (Node)** 들의 연결체입니다.
- **특징**: 메모리 상에 연속적으로 배치되지 않아도 되며, 삽입과 삭제가 자유롭습니다.
- **시간 복잡도**: 특정 원소 탐색 시 첫 노드부터 순차적으로 접근해야 하므로 최악의 경우 ** $O(n)$ ** 이 소요됩니다.

```python
class Node:
    def __init__(self, data):
        self.data = data  # 데이터 저장
        self.next = None  # 다음 노드를 가리키는 포인터

class LinkedList:
    def __init__(self):
        self.head = None  # 리스트의 시작 노드

    def append(self, data):
        """리스트의 맨 뒤에 새로운 데이터를 추가합니다."""
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            return
        curr = self.head
        while curr.next:  # 마지막 노드까지 이동 (O(n))
            curr = curr.next
        curr.next = new_node

    def display(self):
        """리스트의 모든 데이터를 출력합니다."""
        curr = self.head
        while curr:
            print(curr.data, end=" -> ")
            curr = curr.next
        print("None")
```

### 1.2 스택 (Stack)
**LIFO (Last-In First-Out, 후입선출)** 원칙을 따르는 선형 자료구조입니다.
- **주요 연산**: `push` (데이터 삽입), `pop` (가장 최근 데이터 삭제 및 반환).
- **응용**: 재귀 알고리즘의 시스템 스택, 수식의 괄호 검사, 실행 취소(Undo).

```python
class Stack:
    def __init__(self):
        self.items = []

    def is_empty(self):
        return len(self.items) == 0

    def push(self, item):
        """데이터를 스택의 맨 위에 쌓습니다 (O(1))."""
        self.items.append(item)

    def pop(self):
        """스택 맨 위의 데이터를 제거하고 반환합니다 (O(1))."""
        if not self.is_empty():
            return self.items.pop()
        raise IndexError("Stack is empty")
```

### 1.3 큐 (Queue)
**FIFO (First-In First-Out, 선입선출)** 원칙을 따르는 선형 자료구조입니다.
- **주요 연산**: `enqueue` (데이터 삽입), `dequeue` (가장 먼저 들어온 데이터 삭제 및 반환).
- **응용**: 프로세스 관리(스케줄링), 네트워크 버퍼, 너비 우선 탐색(BFS).

```python
from collections import deque

class Queue:
    def __init__(self):
        # 파이썬의 리스트는 pop(0)이 O(n)이므로, O(1)인 deque를 사용합니다.
        self.items = deque()

    def is_empty(self):
        return len(self.items) == 0

    def enqueue(self, item):
        """데이터를 큐의 뒤쪽에 추가합니다 (O(1))."""
        self.items.append(item)

    def dequeue(self):
        """큐의 앞쪽 데이터를 제거하고 반환합니다 (O(1))."""
        if not self.is_empty():
            return self.items.popleft()
        raise IndexError("Queue is empty")
```

### 1.4 힙 (Heap)
**완전 이진 트리 (Complete Binary Tree)** 기반의 자료구조로, 부모 노드와 자식 노드 간의 일정한 대소 관계(힙 속성)를 유지합니다.
- **종류**:
    - **최대 힙 (Max Heap)**: 부모 노드의 키값이 자식보다 크거나 같음.
    - **최소 힙 (Min Heap)**: 부모 노드의 키값이 자식보다 작거나 같음.
- **배열 표현 (1번 인덱스 시작 기준)**:
    - $A[i]$ 의 왼쪽 자식 인덱스: ** $2i$ **
    - $A[i]$ 의 오른쪽 자식 인덱스: ** $2i + 1$ **
    - $A[i]$ 의 부모 노드 인덱스: ** $\lfloor i/2 \rfloor$ **
- **복잡도**: 삽입 및 삭제 연산 시 트리 높이에 비례하는 ** $O(\log\_{2}{n})$ ** 이 소요됩니다.

```python
class MinHeap:
    def __init__(self):
        self.heap = [0]  # 인덱스 계산 편의를 위해 0번은 사용하지 않음

    def insert(self, val):
        """새로운 값을 삽입하고 힙 속성을 유지합니다 (Up-Heap)."""
        self.heap.append(val)
        idx = len(self.heap) - 1
        # 부모와 비교하며 위로 올림
        while idx > 1 and self.heap[idx] < self.heap[idx // 2]:
            self.heap[idx], self.heap[idx // 2] = self.heap[idx // 2], self.heap[idx]
            idx //= 2

    def extract_min(self):
        """최솟값(루트)을 제거하고 힙을 재정비합니다 (Down-Heap)."""
        if len(self.heap) <= 1: return None
        if len(self.heap) == 2: return self.heap.pop()

        root = self.heap[1]
        self.heap[1] = self.heap.pop()  # 맨 마지막 노드를 루트로 이동
        self.heapify(1)
        return root

    def heapify(self, i):
        """특정 노드에서 아래로 내려가며 힙 속성을 복구합니다."""
        left, right = 2 * i, 2 * i + 1
        smallest = i
        n = len(self.heap) - 1

        if left <= n and self.heap[left] < self.heap[smallest]: smallest = left
        if right <= n and self.heap[right] < self.heap[smallest]: smallest = right

        if smallest != i:
            self.heap[i], self.heap[smallest] = self.heap[smallest], self.heap[i]
            self.heapify(smallest)
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
