# Week 05 — 탐욕 알고리즘 (Greedy Algorithms)

이 문서는 매 순간 최선의 선택을 통해 전역적인 최적해를 구하는 탐욕 알고리즘의 원리와 대표적인 사례들을 정리합니다. 5주차 강의 자료의 모든 상세 내용을 포함하고 있습니다.

---

## 📝 목차
1. [탐욕 알고리즘의 개념과 조건](#fundamentals)
2. [탐욕 알고리즘의 실패 사례](#failure)
3. [배낭 문제 (Knapsack Problem)](#knapsack)
4. [스케줄링 문제 (Scheduling)](#scheduling)
5. [허프만 코딩 (Huffman Coding)](#huffman)
6. [최소 신장 트리 (MST) - Kruskal & Prim](#mst)
7. [최단 경로 (Dijkstra)](#dijkstra)
8. [Greedy vs Dynamic Programming](#comparison)

---

<a id="fundamentals"></a>
## 1. 탐욕 알고리즘의 개념과 조건 (#fundamentals)

탐욕 알고리즘은 **최적화 문제 (Optimization Problem)** 를 해결하는 한 방법으로, 매 단계에서 **지금 당장 가장 좋아 보이는 선택** 을 내립니다.

### 1.1 성립 조건 (정당성 증명)
탐욕 알고리즘이 항상 최적해를 보장하려면 다음 두 가지 속성이 증명되어야 합니다.
1. **탐욕적 선택 속성 (Greedy-choice Property)**: 지역적인 최적 선택이 결국 전역적인 최적해로 이어짐.
2. **최적 부분 구조 (Optimal Substructure)**: 전체 문제의 최적해가 하위 문제들의 최적해로 구성됨.

---

<a id="failure"></a>
## 2. 탐욕 알고리즘의 실패 사례 (#failure)

### 2.1 동전 거스름돈 (비표준 단위)
- **표준 단위 (500, 100, 50, 10)**: 각 단위가 배수 관계이므로 탐욕법이 항상 최적해를 보장함.
- **비표준 단위 (160원 동전 추가)**: 200원을 만들 때,
    - 탐욕법: 160 + 10 * 4 = **5개**
    - 최적해: 100 + 100 = **2개**
- **결론**: 동전 단위 간에 배수 관계가 성립하지 않으면 탐욕법은 실패하며, **동적 계획법 (DP)** 을 써야 합니다.

---

<a id="knapsack"></a>
## 3. 배낭 문제 (Knapsack Problem) (#knapsack)

### 3.1 분할 가능 배낭 문제 (Fractional Knapsack)
물건을 쪼갤 수 있는 경우입니다. **무게당 가치 (Value/Weight)** 가 높은 순으로 담으면 항상 최적해를 얻습니다.
- **복잡도**: 정렬 비용 $O(n \log n)$ 이 지배적임.

```python
def fractional_knapsack(items, capacity):
    # 1. 가성비(가치/무게) 기준으로 내림차순 정렬
    items.sort(key=lambda x: x[1]/x[0], reverse=True)
    
    total_value = 0
    for weight, value in items:
        if capacity >= weight:
            capacity -= weight
            total_value += value
        else:
            # 2. 남은 용량만큼 물건을 쪼개서 담음
            total_value += value * (capacity / weight)
            break
    return total_value
```

---

<a id="scheduling"></a>
## 4. 스케줄링 문제 (Scheduling) (#scheduling)

### 4.1 활동 선택 문제 (Activity Selection)
한 강의실에서 겹치지 않게 가장 많은 수업을 배정하는 문제입니다.
- **전략**: **종료 시간 (Finish Time) 이 가장 빠른 순** 으로 선택.

```python
def activity_selection(activities):
    # 종료 시간 기준으로 정렬
    activities.sort(key=lambda x: x[1])
    
    count = 0
    last_finish_time = 0
    for start, finish in activities:
        if start >= last_finish_time:
            count += 1
            last_finish_time = finish
    return count
```

---

<a id="huffman"></a>
## 5. 허프만 코딩 (Huffman Coding) (#huffman)

데이터 압축을 위해 가변 길이 부호를 할당하는 방식입니다. 자주 등장하는 문자에는 짧은 비트를, 드물게 등장하는 문자에는 긴 비트를 할당합니다.
- **접두어 속성 (Prefix Property)**: 어떤 부호도 다른 부호의 앞부분이 되지 않도록 하여 모호함 없이 해독 가능하게 함.
- **복잡도**: 우선순위 큐를 사용하므로 **$O(n \log n)$**.

```python
import heapq

def huffman_coding(frequencies):
    # 1. 모든 문자를 최소 힙에 삽입
    heap = [[weight, [char, ""]] for char, weight in frequencies.items()]
    heapq.heapify(heap)
    
    while len(heap) > 1:
        # 2. 빈도가 가장 낮은 두 노드를 꺼내 합침
        lo = heapq.heappop(heap)
        hi = heapq.heappop(heap)
        for pair in lo[1:]: pair[1] = '0' + pair[1]
        for pair in hi[1:]: pair[1] = '1' + pair[1]
        heapq.heappush(heap, [lo[0] + hi[0]] + lo[1:] + hi[1:])
    
    return sorted(heapq.heappop(heap)[1:], key=lambda p: (len(p[-1]), p))
```

---

<a id="mst"></a>
## 6. 최소 신장 트리 (MST) (#mst)

그래프의 모든 정점을 가장 적은 비용으로 연결하는 트리입니다.

### 6.1 Kruskal 알고리즘
- **전략**: 모든 간선을 가중치 순으로 정렬하고, **사이클을 형성하지 않는** 선에서 가장 작은 간선부터 선택.
- **도구**: 사이클 확인을 위해 **Union-Find** 자료구조 사용.
- **복잡도**: $O(m \log m)$ (간선 정렬 비용).

### 6.2 Prim 알고리즘
- **전략**: 임의의 시작점에서 출발하여, 현재 연결된 트리에서 **가장 가까운 정점** 을 하나씩 추가.
- **복잡도**: $O(n^{2})$ 또는 $O(m \log n)$.

---

<a id="dijkstra"></a>
## 7. 최단 경로 (Dijkstra) (#dijkstra)

특정 출발점에서 다른 모든 정점까지의 최단 거리를 구합니다. (단, 음수 가중치 불가)
- **전략**: 아직 확정되지 않은 정점 중 거리가 가장 짧은 것을 선택하고, 그 정점을 거쳐가는 경로를 갱신(Relaxation).

```python
import heapq

def dijkstra(graph, start):
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    queue = [(0, start)]
    
    while queue:
        current_distance, current_node = heapq.heappop(queue)
        
        if distances[current_node] < current_distance:
            continue
            
        for neighbor, weight in graph[current_node].items():
            distance = current_distance + weight
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(queue, (distance, neighbor))
    return distances
```

---

<a id="comparison"></a>
## 8. Greedy vs Dynamic Programming (#comparison)

| 특징 | 탐욕 알고리즘 (Greedy) | 동적 계획법 (DP) |
| :--- | :--- | :--- |
| **선택 방식** | 매 순간 최선의 지역적 선택 | 모든 하위 문제의 해를 고려 |
| **작동 방향** | Top-down | 주로 Bottom-up |
| **중복 계산** | 없음 (한 번의 선택으로 끝) | 있음 (메모이제이션으로 해결) |
| **정확성 보장** | 탐욕적 속성 증명 시에만 | 최적 부분 구조 만족 시 항상 |

---

## 💡 요약 및 시사점
1. 탐욕 알고리즘은 **가장 직관적이고 빠르지만**, 모든 문제에 적용할 수는 없습니다.
2. **배수 관계**가 성립하지 않는 동전 문제나 **0-1 배낭 문제**처럼 탐욕법이 실패하는 경우를 구별하는 능력이 중요합니다.
3. 허프만 코딩, MST, Dijkstra 등은 탐욕적 선택이 항상 전역적 최적해를 보장하는 우아하고 강력한 사례들입니다.
