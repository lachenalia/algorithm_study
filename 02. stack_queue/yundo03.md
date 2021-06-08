# 코딩테스트 연습_코딩테스트 고득점 Kit_스택/큐(stack/queue)
## 문제: 기능개발
### 입출력 예
|progresses	|speeds	|return   |
|-------------|-----------|---------|
|[93, 30, 55]|	[1, 30, 5]|	[2, 1]|
|[95, 90, 99, 99, 80, 99]|	[1, 1, 1, 1, 1, 1]|	[1, 3, 2]|
### 입출력 예 설명
#### 입출력 예 #1
첫 번째 기능은 93% 완료되어 있고 하루에 1%씩 작업이 가능하므로 7일간 작업 후 배포가 가능합니다.
두 번째 기능은 30%가 완료되어 있고 하루에 30%씩 작업이 가능하므로 3일간 작업 후 배포가 가능합니다.
하지만 이전 첫 번째 기능이 아직 완성된 상태가 아니기 때문에 첫 번째 기능이 배포되는 7일째 배포됩니다.
세 번째 기능은 55%가 완료되어 있고 하루에 5%씩 작업이 가능하므로 9일간 작업 후 배포가 가능합니다.   

따라서 7일째에 2개의 기능, 9일째에 1개의 기능이 배포됩니다.   

#### 입출력 예 #2
모든 기능이 하루에 1%씩 작업이 가능하므로, 작업이 끝나기까지 남은 일수는 각각 5일, 10일, 1일, 1일, 20일, 1일입니다.   
어떤 기능이 먼저 완성되었더라도 앞에 있는 모든 기능이 완성되지 않으면 배포가 불가능합니다.   

따라서 5일째에 1개의 기능, 10일째에 3개의 기능, 20일째에 2개의 기능이 배포됩니다.   

### 접근방법
- progresses리스트의 index번호 순서대로 기능이 완료가 되어야 한다.
- 한번에 여러개의 기능이 배포 가능하다.

### solution.py
<pre>
def solution(progresses, speeds):
    answer = []
    
    while progresses:
        c = 0
        for i in range(len(progresses)):
            progresses[i] += speeds[i]
            
        if progresses[0] >= 100:
            while progresses:
                if progresses[0] >= 100:
                    del progresses[0]
                    del speeds[0]
                    c += 1
                else:
                    break
            answer.append(c)
            
    return answer
</pre>

## 문제: 프린터
### 입출력 예제
|priorities	|location	|return|
|-----------|---------|------|
|[2, 1, 3, 2]|	2|	1|
|[1, 1, 9, 1, 1, 1]|	0|	5|

### 입출력 예 설명
#### 예제 #1   
문제에 나온 예와 같습니다.

#### 예제 #2   
6개의 문서(A, B, C, D, E, F)가 인쇄 대기목록에 있고 중요도가 1 1 9 1 1 1 이므로 C D E F A B 순으로 인쇄합니다.   

### 접근방법
- 접두어를 구하지만 접두어로 찾기 않고 각각의 번호의 앞부터(index 0부터) phone_book각각의 값에 대입해 찾는 방식

### solution.py
<pre>
def solution(priorities, location):
    answer = 0
    pri = list((i, v) for i, v in enumerate(priorities))

    while pri:
        i,v = pri.pop(0)
        if max(priorities) > v:
            pri.append((i,v))
        else:
            answer += 1
            priorities.remove(v)
            if i == location:
                break
                
    return answer
################ 시도했다가 실패 ################
# def solution(priorities, location):
#     pri = list((v, i) for i, v in enumerate(priorities))
#     print(pri)
# 
#     p = pri[:]
#     p_max = max(priorities)
# 
#     b = p[:priorities.index(p_max)]
#     del p[:priorities.index(p_max)]
# 
#     p.extend(b)
#     print(p)
# 
#     b = p[1:]
#     b = sorted(b, key=lambda x: -x[0])
#     del p[1:]
# 
#     p.extend(b)
#     print(p)
# 
#     for i in range(len(priorities)):
#         if p[i][0] == priorities[location] and p[i][1] == location:
#             print(i+1)
#             return i + 1
#####################################################
</pre>

## 문제: 다리를 지나는 트럭
### 입출력 예
|bridge_length|	weight|	truck_weights	return|
|-------------|-------|---------------------|
|2	|10|	[7,4,5,6]|	8|
|100|	100|	[10]|	101|
|100|	100|	[10,10,10,10,10,10,10,10,10,10]|	110|
   
### 문제 설명
|경과 시간|	다리를 지난 트럭|	다리를 건너는 트럭|	대기 트럭|
|--------|----------------|-------------------|---------|
|0|	[]|	[]|	[7,4,5,6]|
|1~2|	[]|	[7]|	[4,5,6]|
|3|	[7]|	[4]|	[5,6]|
|4|	[7]|	[4,5]|	[6]|
|5|	[7,4]|	[5]|	[6]|
|6~7|	[7,4,5]|	[6]|	[]|
|8|	[7,4,5,6]|	[]|	[]|


### 접근방법
- 리스트로 최대한 문제설명에 가깝게 구현
- 수학적(공식)으로 접근하면 더 빨리 풀 수 있을것 같다.

### solution.py
<pre>
def solution(bridge_length, weight, truck_weights):
    time = 0
    bridge = [0] * bridge_length

    while bridge:
        time += 1   
        bridge.pop(0)
        if len(truck_weights):
            if sum(bridge) + truck_weights[0] <= weight:
                pop_t = truck_weights.pop(0)
                bridge.append(pop_t)
            else:
                bridge.append(0)

    return time
</pre>   

## 문제: 주식가격
### 입출력 예
|prices|	return|
|------|--------|
|[1, 2, 3, 2, 3]|	[4, 3, 1, 1, 0]|

### 입출력 예 설명
- 1초 시점의 ₩1은 끝까지 가격이 떨어지지 않았습니다.
- 2초 시점의 ₩2은 끝까지 가격이 떨어지지 않았습니다.
- 3초 시점의 ₩3은 1초뒤에 가격이 떨어집니다. 따라서 1초간 가격이 떨어지지 않은 것으로 봅니다.
- 4초 시점의 ₩2은 1초간 가격이 떨어지지 않았습니다.
- 5초 시점의 ₩3은 0초간 가격이 떨어지지 않았습니다.

### 접근방법
- prices의 기준index와 다음index를 비교해 가격하락 시간을 기록  
   
### solution.py
<pre>
def solution(prices):
    answer = []
    
    for i in range(len(prices)):
        c = 0
        for j in range(i + 1, len(prices)):
            c += 1
            if prices[i] > prices[j]:
                break

        answer.append(c)
    return answer
</pre>
