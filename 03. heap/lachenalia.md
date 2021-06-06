# Heap
## 더 맵게: 42626
### 문제 설명
* input: scoville(각 음식의 스코빌 지수 리스트), K(원하는 최소 스코빌 지수)
* output: 음식을 섞은 횟수
* 모든 음식의 스코빌 지수가 K 이상이 될 때까지 음식을 섞는다.
* 가장 스코빌 지수가 작은 음식이 a, 두번째로 작은 음식이 b일 때,
  두 음식을 섞어 스코빌 지수가 a + 2b 인 음식을 만든다.

### 접근방법
1. 가장 스코빌 지수가 작은 음식(A)을 찾아 K보다 큰지 확인한다.
   
   1-1. K보다 크면 cnt를 반환한다.

2. A 다음으로 스코빌 지수가 작은 음식(B)을 찾아 새로운 스코빌 지수를 계산하여 리스트에 더한다.
> 이때 python의 heapq 라이브러리를 이용하면 가장 작은 원소를 찾는데에 드는 시간을 줄일 수 있다.
> <br>heapq 라이브러리는 자동으로 정렬하여 가장 작은 원소부터 꺼내주는 자료구조의 한 종류이다.
### Code
```
import heapq
def solution(scoville, K):
    answer = 0
    heapq.heapify(scoville)
    while len(scoville) > 1 :
        a = heapq.heappop(scoville)
        if a >= K:
            return answer
        b = heapq.heappop(scoville)
        heapq.heappush(scoville, a + 2*b)
        answer += 1
    if scoville.pop() >= K:
        return answer
    return -1
```

## 디스크 컨트롤러: 42627
### 문제 설명
* input: jobs(하드디스크에 들어온 요청 리스트, [요청시간, 소요시간]의 구조로 되어있다.)
* output: 대기시간 + 소요시간의 평균이 가장 적게 되도록 처리했을 때 평균값
* 소수점 이하의 수는 버린다.

### 접근방법
1. 요청시간 순으로 jobs를 정렬한다.
2. 들어오는 요청 순으로, 현재 시간보다 이전에 들어온 요청들을 모두 wait 이라는 이름의 큐에 저장한다.
3. wait 큐에서 소요시간이 가장 짧은 작업부터 실행한다. (현재시간 증가)
4. 작업이 남지 않을 때까지 2-3을 반복한다.
> wait 큐는 python 의 heapq 를 이용한다.

### Code
```
import heapq
def solution(jobs):
    n = len(jobs)
    jobs.sort(reverse=True)
    wait = []
    current_time = 0
    total_time = 0
    while jobs:
        j = jobs.pop()
        while wait and j[0] > current_time:
            work = heapq.heappop(wait)
            total_time = total_time + (len(wait) * work) + work
            current_time += work
        if j[0] <= current_time:
            total_time += current_time - j[0]
        else:
            current_time = j[0]
        heapq.heappush(wait, j[1])
    while wait:
        work = heapq.heappop(wait)
        total_time = total_time + (len(wait) * work) + work
    return total_time // n
```

## 이중우선순위큐: 42628
### 문제 설명
* input: operations(연산의 리스트)
  - 연산은 "알파벳 숫자" 형식으로 되어 있다.
  - 알파벳 I는 Insert로 숫자를 큐에 추가한다.
  - 알파벳 D는 Delete로 뒤의 숫자가 1인 경우 최댓값을, -1인 경우 최솟값을 큐에서 제거한다.
* output: 최종 큐의 최댓값, 최솟값 순서로 리스트에 담아 반환한다.
* 큐에 값이 없는 경우 D 연산은 하지 않는다.

### 접근방법
각 연산을 순서대로 실행하며 heapq에 담는다. 
heapq는 최솟값만을 보장하므로, 최댓값을 제거해야하는 경우 최댓값을 따로 구해서 리스트에서 제거한다.

### Code
```
import heapq

def solution(operations):
    heap = []
    for op in operations:
        o = op.split()
        if o[0] == "I":
            heapq.heappush(heap, int(o[1]))
        elif o[1] == '-1' and len(heap) > 0:
            heapq.heappop(heap)
        elif o[1] == '1' and len(heap) > 0:
            heap.remove(max(heap))
    if len(heap) == 0:
        return [0, 0]
    return [max(heap), heap[0]]
```