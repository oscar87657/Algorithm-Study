# Week 05 — 그리디 알고리즘 (Greedy Algorithms)

이 문서는 매 순간 최선의 선택을 통해 전체 최적해를 구하는 **그리디 알고리즘 (Greedy Algorithm)** 의 원리, 설계 전략, 그리고 주요 사례(배낭 문제, 허프만 코딩, MST, 최단 경로 등)를 상세히 다룹니다.

---

## 📝 목차
1. [그리디 알고리즘(Greedy Algorithm) 개요](#intro)
2. [기초 그리디 알고리즘 사례](#basic)
3. [데이터 압축과 허프만 코딩](#huffman)
4. [그래프 내의 그리디 알고리즘](#graph)
5. [그리디 vs 동적 계획법](#comparison)

---

<a id="intro"></a>
## 1. 그리디 알고리즘(Greedy Algorithm) 개요

**그리디 알고리즘 (Greedy Algorithm)** 은 해결해야 할 전체 문제를 한 번에 해결하는 대신, 지금 이 순간 가장 최선이라고 생각되는 선택(**Local Optimal**)을 반복하여 전체 문제의 최적해(**Global Optimal**)를 구하는 방식입니다.

### 1.1 작동 원리
그리디 알고리즘은 전형적으로 다음과 같은 단계를 거칩니다.

**[그리디 알고리즘 프로세스 시각화]**
```text
      [ Current State ]
             |
      ( Greedy Choice ) <--- 매 순간 가장 좋아 보이는 것 선택
             v
      [ Subproblem ]    <--- 선택 후 남은 문제
             |
      ( Repeat until Done )
             v
      [ Final Solution ]
```

1.  **선택 (Selection)**: 현재 상태에서 최적이라고 판단되는 해를 골라 부분 해(Partial Solution)에 추가합니다.
2.  **적절성 검사 (Feasibility Check)**: 선택한 해가 문제의 제약 조건을 위반하지 않는지 확인합니다.
3.  **해결 검사 (Solution Check)**: 원래 문제가 해결되었는지 확인하고, 남은 부분이 있다면 1번 단계로 돌아가 반복합니다.

### 1.2 핵심 속성 (Two Key Properties)
그리디 알고리즘이 항상 최적해를 보장하기 위해서는 다음 두 가지 성질을 만족해야 합니다.

1.  **탐욕적 선택 속성 (Greedy-choice Property)**: 이전의 선택이 이후의 선택에 영향을 주지 않으며, 매 순간의 최적 선택이 결국 전체 최적해로 이어진다는 확신이 있어야 합니다.
2.  **최적 부분 구조 (Optimal Substructure)**: 문제에 대한 최적해가 부분 문제들에 대한 최적해를 포함하고 있어야 합니다. (분할 정복 및 동적 계획법과 공통되는 특징)

### 1.3 성공 조건 및 한계
- **장점**: 설계가 매우 직관적이고 구현이 쉬우며, 대개의 경우 계산 복잡도가 낮아 매우 빠릅니다.
- **한계**: 모든 문제에서 최적해를 보장하지는 않습니다. **국소적인 최적해(Local Optima)** 가 전체 최적해로 이어지지 않는 경우, 그리디 알고리즘은 잘못된 결과를 낼 수 있습니다.

**[그리디가 실패하는 경우 시각화 (최대 합 경로 찾기)]**
```text
          [ 10 ]
         /      \
      [ 2 ]    [ 5 ]  <-- 그리디는 여기서 5를 선택
      /   \    /   \
   [ 100 ] [1][1]  [1] <-- 실제 최적은 10 -> 2 -> 100 (112)
                           그리디 결과: 10 -> 5 -> 1 (16)
```
- **결론**: 그리디 알고리즘을 적용할 때는 반드시 해당 전략이 최적해를 보장하는지 수학적 증명이나 반례 확인이 필요합니다.

---

<a id="basic"></a>
## 2. 기초 그리디 알고리즘 사례

그리디 알고리즘이 실생활과 알고리즘 문제에서 어떻게 적용되는지 구체적인 사례를 통해 살펴봅니다.

### 2.1 동전 거스름돈 (Coin Change)
가장 큰 단위의 동전부터 최대한 많이 사용하는 방식입니다.

- **그리디 전략**: 현재 줄 수 있는 가장 큰 권종의 동전을 선택하여 남은 금액을 줄여나갑니다.
- **성공 조건**: 동전들 사이의 관계가 **배수 관계**일 때 최적해를 보장합니다. (예: 500원, 100원, 50원, 10원)
- **실패 사례 (Counter-example)**: 동전 단위가 500원, 400원, 100원이고 거스름돈이 800원인 경우.
    - **그리디 결과**: 500원 + 100원 + 100원 + 100원 (총 4개)
    - **최적해 (Optimal)**: 400원 + 400원 (총 2개)

**[작동 로직 (Python)]**
```python
def coin_change_greedy(amount, coins):
    coins.sort(reverse=True) # 큰 동전부터 정렬
    count = 0
    for coin in coins:
        count += amount // coin
        amount %= coin
    return count
```

### 2.2 분할 가능 배낭 문제 (Fractional Knapsack)
물건을 쪼개서 넣을 수 있는 배낭 문제로, 그리디 알고리즘으로 최적해를 구할 수 있습니다.

- **그리디 전략**: **단위 무게당 가치 ( $v\_{i} / w\_{i}$ )** 가 가장 높은 물건부터 순서대로 배낭에 담습니다.
- **비교 ($0-1$ 배낭 문제)**: 물건을 통째로 넣거나 말아야 하는 $0-1$ 배낭 문제는 그리디로 해결할 수 없으며, 동적 계획법이 필요합니다.

**[작동 원리 시각화]**
```text
Item 1: 60$ / 10kg ($6/kg)
Item 2: 100$ / 20kg ($5/kg)
Item 3: 120$ / 30kg ($4/kg)
배낭 용량: 50kg

1. Item 1 선택 (10kg 남음, 가치: 60$)
2. Item 2 선택 (30kg 남음, 가치: 160$)
3. Item 3은 20kg만 쪼개서 넣음 (남은 용량 0, 가치: 160 + 120 * (2/3) = 240$)
```

- **복잡도 분석**: 물건을 가치 비율 순으로 정렬하는 데  $O(n \log\_{2}{n})$  의 시간이 소요됩니다.

**[작동 로직 (Python)]**
```python
def fractional_knapsack(items, capacity):
    # 단위 무게당 가치(v/w) 기준으로 내림차순 정렬
    items.sort(key=lambda x: x[0]/x[1], reverse=True)
    
    total_value = 0
    for value, weight in items:
        if capacity >= weight:
            # 물건을 통째로 담을 수 있는 경우
            capacity -= weight
            total_value += value
        else:
            # 물건의 일부만 담는 경우 (Fractional)
            total_value += value * (capacity / weight)
            break
            
    return total_value
```

### 2.3 회의실 배정 (Activity Selection)
한 개의 회의실에서 가장 많은 회의를 열 수 있도록 스케줄을 짜는 문제입니다.

- **그리디 전략**: **종료 시간 (Finish Time)** 이 가장 빠른 회의부터 순서대로 선택합니다.
- **선택 기준의 정당성**: 회의가 빨리 끝날수록 다음 회의를 시작할 수 있는 여유 시간이 더 많이 확보되기 때문에 가장 많은 회의를 선택할 수 있게 됩니다. (시작 시간이나 짧은 시간 기준은 최적해를 보장하지 않음)

**[작동 로직 (Python)]**
```python
def activity_selection(meetings):
    # 종료 시간 기준으로 정렬
    meetings.sort(key=lambda x: x[1])
    
    last_finish_time = 0
    selected_meetings = []
    
    for start, finish in meetings:
        if start >= last_finish_time:
            selected_meetings.append((start, finish))
            last_finish_time = finish
            
    return selected_meetings
```

- **복잡도 분석**: 종료 시간 기준 정렬에  $O(n \log\_{2}{n})$ , 순차 선택에  $O(n)$  이 소요되어 총  $O(n \log\_{2}{n})$  입니다.

### 2.4 작업 스케줄링 (Job Scheduling with Deadlines)
각 작업에 마감 시간(Deadline)과 이익(Profit)이 주어졌을 때, 마감 시간 내에 작업을 완료하여 얻을 수 있는 총 이익을 극대화하는 문제입니다.

- **그리디 전략**: **이익 (Profit)** 이 가장 높은 작업부터 선택하여, 해당 작업의 **마감 시간에 가장 가까운 빈 시간대**에 배치합니다.
- **회의실 배정과의 차이**: 회의실 배정은 '개수'를 최대화하지만, 작업 스케줄링은 각 작업의 '가치(이익)'를 고려하여 총합을 최대화합니다.

**[작동 로직 (Python)]**
```python
def job_scheduling(jobs, t):
    # 이익 기준 내림차순 정렬
    jobs.sort(key=lambda x: x[2], reverse=True)
    
    result = [None] * t # 시간대별 할당 결과
    total_profit = 0
    
    for job_id, deadline, profit in jobs:
        # 마감 시간부터 역순으로 빈 자리가 있는지 확인
        for j in range(min(t, deadline) - 1, -1, -1):
            if result[j] is None:
                result[j] = job_id
                total_profit += profit
                break
                
    return result, total_profit
```

- **핵심 포인트**: 이익이 큰 작업을 먼저 처리하되, 가능한 한 마감 직전에 배치하여 앞쪽 시간대를 다른 작업을 위해 남겨두는 탐욕적 선택을 합니다.

---

<a id="huffman"></a>
## 3. 데이터 압축과 허프만 코딩 (Huffman Coding)

**허프만 코딩 (Huffman Coding)** 은 문자의 출현 빈도에 따라 각기 다른 길이의 부호를 부여하는 **가변 길이 부호화 (Variable-length Encoding)** 방식입니다.

### 3.1 접두사 코드 (Prefix Code)
어떤 문자의 부호가 다른 문자 부호의 앞부분(Prefix)이 되지 않도록 설계된 코드입니다. 이 성질 덕분에 부호 사이의 구분자 없이도 모호함 없이 해독(Decoding)이 가능합니다.

- **예시**: `A: 0`, `B: 10`, `C: 110` (접두사 속성 만족)

### 3.2 허프만 트리 구축 (Greedy Strategy)
허프만 코딩은 **"빈도수가 낮은 문자들부터 먼저 합친다"** 는 탐욕적 선택을 통해 최적의 이진 트리를 구성합니다.

**[트리 구축 과정 시각화]**
```text
Frequencies: {A: 45, B: 13, C: 12, D: 16, E: 9, F: 5}

1. 가장 낮은 빈도인 F(5)와 E(9)를 합침 -> New Node(14)
2. 다음 낮은 빈도인 C(12)와 위에서 만든 Node(14)를 합침... (반복)

최종 트리 형태:
          [ 100 ]
         /       \
      A(45):0    [ 55 ]:1
                /      \
             ...        ...
```

- **그리디 선택**: 매 단계마다 사용되지 않은 노드 중 빈도수가 가장 작은 두 노드를 선택하여 부모 노드를 만듭니다.
- **부호 할당**: 루트에서 왼쪽 가지는 `0`, 오른쪽 가지는 `1`을 부여하여 각 리프 노드(문자)까지의 경로를 부호로 사용합니다.

### 3.3 복잡도 및 효율성
- **시간 복잡도**: 우선순위 큐(Min-Heap)를 사용하여 빈도수가 낮은 노드를 추출하므로  $O(n \log\_{2}{n})$  이 소요됩니다.
- **공간 효율성**: 자주 나타나는 문자는 짧은 코드를, 드물게 나타나는 문자는 긴 코드를 할당받아 전체 파일 크기를 최소화합니다.

**[작동 로직 (Python)]**
```python
import heapq

class Node:
    def __init__(self, char, freq):
        self.char = char
        self.freq = freq
        self.left = None
        self.right = None
    # 우선순위 큐에서의 비교를 위해 __lt__ 정의
    def __lt__(self, other):
        return self.freq < other.freq

def build_huffman_tree(frequencies):
    heap = [Node(char, freq) for char, freq in frequencies.items()]
    heapq.heapify(heap)
    
    while len(heap) > 1:
        left = heapq.heappop(heap)
        right = heapq.heappop(heap)
        
        # 두 노드를 합쳐 새 부모 노드 생성 (그리디 선택)
        merged = Node(None, left.freq + right.freq)
        merged.left = left
        merged.right = right
        heapq.heappush(heap, merged)
        
    return heapq.heappop(heap)

def generate_codes(node, current_code, codes):
    if node is None: return
    if node.char is not None:
        codes[node.char] = current_code
    generate_codes(node.left, current_code + "0", codes)
    generate_codes(node.right, current_code + "1", codes)
```

---

<a id="graph"></a>
## 4. 그래프 내의 그리디 알고리즘

그래프의 가중치 간선이나 최단 경로를 구하는 문제에서도 그리디 전략은 매우 효율적인 해법을 제공합니다.

### 4.1 최소 신장 트리 (Minimum Spanning Tree, MST)
그래프 내의 모든 정점을 가장 적은 비용(가중치의 합)으로 연결하는 트리 구조를 찾는 문제입니다.

#### 1) Kruskal's Algorithm (간선 중심)
모든 간선을 가중치 순으로 정렬한 뒤, 사이클을 형성하지 않는 선에서 가장 작은 가중치의 간선부터 차례대로 선택하는 방식입니다.

- **그리디 선택**: 가중치가 가장 작은 간선을 우선적으로 선택합니다.
- **사이클 방지**: 선택한 간선이 기존의 연결 요소들과 사이클을 이루는지 확인하기 위해 **Union-Find** 자료구조를 사용합니다.
- **복잡도**: 간선 정렬에  $O(E \log\_{2}{E})$  가 소요됩니다. ( $E$ 는 간선의 수)

#### 2) Prim's Algorithm (정점 중심)
시작 정점에서부터 출발하여, 현재 연결된 트리에서 갈 수 있는 정점 중 가장 비용이 적은 간선으로 연결된 정점을 선택해 나가는 방식입니다.

- **그리디 선택**: 현재 트리에 인접한 간선 중 가중치가 가장 작은 간선을 선택합니다.
- **복잡도**: 우선순위 큐(Binary Heap)를 사용할 경우  $O(E \log\_{2}{V})$  가 소요됩니다. ( $V$ 는 정점의 수)

**[Kruskal vs Prim 비교]**

| 특징 | Kruskal's | Prim's |
| :--- | :--- | :--- |
| **접근 방식** | 간선(Edge) 중심 | 정점(Vertex) 중심 |
| **자료구조** | Union-Find, 정렬 | 우선순위 큐 (Priority Queue) |
| **유리한 경우** | 간선이 적은 희소 그래프 (Sparse) | 간선이 많은 밀집 그래프 (Dense) |

### 4.2 최단 경로 (Shortest Path - Dijkstra's Algorithm)
주어진 시작 정점에서 다른 모든 정점까지의 최단 거리를 찾는 알고리즘입니다.

- **그리디 전략**: 아직 방문하지 않은 정점들 중 **시작점으로부터의 거리가 가장 짧은 정점**을 매번 선택합니다.
- **정점 이완 (Edge Relaxation)**: 새로운 정점  $u$  를 거쳐서 정점  $v$  로 가는 거리가 기존에 알려진  $v$  까지의 거리보다 짧다면, 그 거리를 갱신합니다.
    -  $\text{if } d[u] + w(u, v) \lt d[v] \text{ then } d[v] = d[u] + w(u, v)$ 
- **제약 사항**: 모든 간선의 가중치가 **0 이상**이어야 합니다. (음수 가중치가 있을 경우 벨만-포드 알고리즘 필요)
- **복잡도**: 우선순위 큐(Min-Heap)를 사용할 경우  $O(E \log\_{2}{V})$  가 소요됩니다.

**[작동 로직 (Python)]**
```python
import heapq

def dijkstra(graph, start):
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    queue = [(0, start)]
    
    while queue:
        current_distance, current_node = heapq.heappop(queue)
        
        # 이미 처리된 노드라면 스킵
        if distances[current_node] < current_distance:
            continue
            
        for neighbor, weight in graph[current_node].items():
            distance = current_distance + weight
            
            # 더 짧은 경로를 찾은 경우 (Relaxation)
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(queue, (distance, neighbor))
                
    return distances
```

---

<a id="comparison"></a>
## 5. 그리디 vs 동적 계획법

그리디 알고리즘과 동적 계획법은 모두 최적 부분 구조를 가지는 문제를 해결하는 데 사용되지만, 접근 방식에서 뚜렷한 차이가 있습니다.

### 5.1 주요 차이점 비교

| 특징 | 그리디 알고리즘 (Greedy) | 동적 계획법 (DP) |
| :--- | :--- | :--- |
| **선택 방식** | 매 순간 최선의 선택 (**Local**) | 모든 부분 문제의 해를 고려 (**Global**) |
| **진행 방향** | 하향식 (**Top-down**) | 주로 상향식 (**Bottom-up**) |
| **결정 번복** | 한 번의 선택은 번복하지 않음 | 모든 가능성을 저장하고 재사용함 |
| **속도** | 비교적 빠름 | 비교적 느림 (범용적) |
| **최적성** | 탐욕적 선택 속성이 성립해야 보장 | 최적 부분 구조가 성립하면 항상 보장 |

---

### 5.2 알고리즘 성능 요약표

5주차에서 다룬 주요 알고리즘들의 성능과 특징을 정리합니다.

| 알고리즘 | 문제 유형 | 시간 복잡도 | 최적해 보장 여부 |
| :--- | :--- | :--- | :--- |
| **동전 거스름돈** | 최소 동전 개수 |  $O(k)$  ( $k$ 는 동전 종류) | 표준 권종일 때만 최적 |
| **분할 가능 배낭** | 최대 가치 합산 |  $O(n \log\_{2}{n})$  | 항상 최적 |
| **회의실 배정** | 최대 활동 개수 |  $O(n \log\_{2}{n})$  | 항상 최적 |
| **허프만 코딩** | 최적 접두사 코드 |  $O(n \log\_{2}{n})$  | 항상 최적 |
| **Kruskal (MST)** | 최소 신장 트리 |  $O(E \log\_{2}{E})$  | 항상 최적 |
| **Prim (MST)** | 최소 신장 트리 |  $O(E \log\_{2}{V})$  | 항상 최적 |
| **Dijkstra** | 단일 출발 최단 경로 |  $O(E \log\_{2}{V})$  | 가중치가 양수일 때만 최적 |

---

### 💡 멘토의 팁 (Greedy Strategy)
문제를 해결할 때 가장 먼저 **그리디 전략**을 시도해 보세요. 만약 해당 전략이 최적해를 보장한다는 것을 증명할 수 있다면, 복잡한 DP나 완전 탐색보다 훨씬 빠르고 간결한 해결책이 될 것입니다. 하지만 최적해가 보장되지 않는다면 즉시 **동적 계획법**으로 전환해야 합니다.
