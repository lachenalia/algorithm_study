# Stack/Queue
## 기능개발: 42586
### 문제 설명
* input: progresses(작업의 현재 진행률), speeds(작업의 속도)
* output: 각 배포마다의 배포되는 기능 갯수 리스트
* 선행 작업이 완료되어야 배포된다.

### 접근방법
progresses와 speeds를 zip 연산으로 묶어서 함께 계산한다.
첫번째 작업부터 소요시간을 구하고, 작업의 소요시간이 현재 시간보다 짧은 경우 cnt를 1 더한다.
작업의 소요시간이 현재 시간보다 긴 경우, 지금까지 더해진 cnt를 answer에 추가하고, 현재 시간을 늘린 후 cnt를 1로 초기화한다.

이때, 소요시간을 구할 때에 (100-p)가 아닌 (p-100)을 통해 음수로 구한다.
음수로 몫 연산(//)을 하면 정수로 내림하므로 이후 마이너스 부호를 붙여 양수로 변환하면 올림이 된다.

### Code
```
def solution(progresses, speeds):
    answer = []
    time = 0
    cnt = 0
    for p, s in zip(progresses, speeds):
        d = -((p-100) // s) # 정수로 올림하기 위해 음수를 이용하여 계산한다.
        if time == 0 or time < d:
            time = d
            if cnt:
                answer.append(cnt)
            cnt = 1
        else:
            cnt += 1
    answer.append(cnt)
    return answer
```
## 프린터: 42587
### 문제 설명
* input: priorities(인쇄 목록의 중요도), location(내가 요청한 문서의 위치)
* output: 내가 요청한 문서의 인쇄 순서
* 현재 인쇄 예정인 문서보다 중요도가 높은 문서가 존재하면 현재 문서는 대기줄의 맨 뒤로 이동한다.

### 접근방법
가장 중요도가 높은 문서가 0번째로 오도록 리스트를 잘라서 이어붙인다.


### Code
```
def solution(priorities, location):
    answer = 0
    while priorities:
        m = max(priorities) # 가장 높은 중요도 값
        if priorities[0] == m: # 만약 현재 문서가 가장 중요하면
            if location == 0:
                return answer + 1
            del priorities[0] # 인쇄 처리
            location -= 1
            answer += 1
        else:
            m_idx = priorities.index(m)
            priorities = priorities[m_idx:] + priorities[:m_idx]
            location = location - m_idx
            if location < 0:
                location += len(priorities)
    return answer
```
## 다리를 지나는 트럭: 42583
### 문제 설명
* input
   - bridge_length(int): 다리의 길이
   - weight(int): 다리가 견딜 수 있는 하중
   - truck_weights(list of int): 대기중인 트럭의 무게
* output
   - (int) 모든 트럭이 지나가는데 걸리는 시간
  
### 접근방법
다리를 의미하는 Queue를 만들어 사용한다. (초기값은 다리길이만큼의 0으로 채워져 있다.)

다리에 있는 트럭의 무게에 다리를 지나려는 트럭의 무게를 더한 값이 다리가 견딜 수 있는 하중보다 작은 경우
트럭의 무게를 push 하고 그렇지 않은 경우 0을 push 하며 시간을 증가시킨다.
마지막 트럭이 다리에 올라간 이후 다리 길이만큼의 시간을 더해 반환한다. 

### Code
```
from collections import deque

def solution(bridge_length, weight, truck_weights):
    bridge = deque([0] * bridge_length)
    truck_weights = truck_weights[::-1]
    bridge_weight = 0
    time = 0
    while truck_weights:
        t = truck_weights.pop()
        bridge_weight -= bridge.pop()
        while bridge_weight + t > weight:
            bridge.appendleft(0)
            bridge_weight -= bridge.pop()
            time += 1
        bridge.appendleft(t)
        bridge_weight += t
        time += 1
    return time + bridge_length
```
## 주식가격: 42584
### 문제 설명
* input: prices(주식 가격의 변동 리스트)
* output: 주식 가격이 떨어지지 않은 기간 리스트
* 각 시간마다의 주식 가격이 떨어지지 않은 기간을 구해 인덱스에 맞게 반환한다.

### 접근방법
각 인덱스 별로 주식 가격이 떨어지는 순간을 찾아 해당 순간까지 몇 초간 가격이 떨어지지 않았는지를 구한다.

### Code
> Stack을 이용하는 방법
```
def solution(prices):
    stack = []
    n = len(prices)
    answer = [-1] * n
    for i in range(n):
        if not stack or prices[stack[-1]] <= prices[i]:
            stack.append(i)
        else:
            while stack and prices[stack[-1]] > prices[i]:
                s = stack.pop()
                answer[s] = i - s
            stack.append(i)

    while stack:
        s = stack.pop()
        answer[s] = n - s - 1
    return answer
```
> list의 index를 이용하여 가격이 떨어지는 순간과의 차를 계산하는 방법
```
def solution(prices):
    n = len(prices)
    answer = [-1] * n
    for i in range(n):
        for j in range(i+1, n):
            if prices[i] > prices[j]:
                answer[i] = j-i
                break
        if answer[i] == -1:
            answer[i] = n - i - 1
    return answer
```