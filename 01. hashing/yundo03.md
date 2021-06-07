# 코딩테스트 연습_코딩테스트 고득점 Kit_해시(Hash)
## 완주하지 못한 선수
### 입출력 예
|participant	|completion	|return   |
|-------------|-----------|---------|
|["leo", "kiki", "eden"]|	["eden", "kiki"]|	"leo"|
|["marina", "josipa", "nikola", "vinko", "filipa"]|	["josipa", "filipa", "marina", "nikola"]|	"vinko"|
|["mislav", "stanko", "mislav", "ana"]|	["stanko", "ana", "mislav"]|	"mislav" | 

### 입출력 예 설명
예제 #1   
"leo"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.   

예제 #2   
"vinko"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.   

예제 #3   
"mislav"는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.   

### 접근방법
- 동명이인을 구별해야한다.
- (제한사항)completion의 길이는 participant의 길이보다 1 작다.
- 동명이인이 없다면 정렬을 했을때 참가자명단 리스트에서 맨뒤에 위치해있다.   

### solution.py
<pre>
def solution(participant, completion):
    participant.sort()
    completion.sort()
    for par, com in zip(participant, completion):
        if par != com:
            return par
    return participant[-1]
</pre>

## 전화번호 목록
### 입출력 예제
|phone_book	|return|
|-----------|------|
|["119", "97674223", "1195524421"]|	false|
|["123","456","789"]|	true|
|["12","123","1235","567","88"]|	false|   

### 입출력 예 설명
입출력 예 #1
앞에서 설명한 예와 같습니다.

입출력 예 #2
한 번호가 다른 번호의 접두사인 경우가 없으므로, 답은 true입니다.

입출력 예 #3
첫 번째 전화번호, “12”가 두 번째 전화번호 “123”의 접두사입니다. 따라서 답은 false입니다.

### 접근방법
- 접두어를 구하지만 접두어로 찾기 않고 각각의 번호의 앞부터(index 0부터) phone_book각각의 값에 대입해 찾는 방식

### solution.py
<pre>
def solution(phone_book):
    answer = True
    hash_map = {}

    for phone_number in phone_book:
        hash_map[phone_number] = 1

    for phone_number in phone_book:
        temp = ""
        for number in phone_number:
            temp += number
            if temp in hash_map and temp != phone_number:
                return False #answer = False

    return answer
</pre>

## 위장
### 입출력 예
|clothes|	return|
|-------|---------|
|[["yellowhat", "headgear"], ["bluesunglasses", "eyewear"], ["green_turban", "headgear"]]|	5|
|[["crowmask", "face"], ["bluesunglasses", "face"], ["smoky_makeup", "face"]]|	3|   
   
### 입출력 예 설명
예제 #1   
headgear에 해당하는 의상이 yellow_hat, green_turban이고 eyewear에 해당하는 의상이 blue_sunglasses이므로 아래와 같이 5개의 조합이 가능합니다.
1. yellow_hat
2. blue_sunglasses
3. green_turban
4. yellow_hat + blue_sunglasses
5. green_turban + blue_sunglasses   
   
예제 #2   
face에 해당하는 의상이 crow_mask, blue_sunglasses, smoky_makeup이므로 아래와 같이 3개의 조합이 가능합니다.
1. crow_mask
2. blue_sunglasses
3. smoky_makeup   

### 접근방법
- 경우의 수를 구하는 문제
- 옷의 종류에 경우의 수를 구해 아무것도 안입은 경우를 빼서 구했다.


### solution.py
<pre>
def solution(clothes):
    dress = {}
    for cloth in clothes:
        if cloth[1] in dress:
            dress[cloth[1]]+=1
        else:
            dress[cloth[1]]=1

    c = 1
    for i in dress.values():
        c *= (i+1)

    return c-1
</pre>   

## 베스트앨범
### 입출력 예
|genres	|plays|	return|
|-------|-----|-------|
|["classic", "pop", "classic", "classic", "pop"]|	[500, 600, 150, 800, 2500]|	[4, 1, 3, 0]|

### 입출력 예 설명
classic 장르는 1,450회 재생되었으며, classic 노래는 다음과 같습니다.

고유 번호 3: 800회 재생
고유 번호 0: 500회 재생
고유 번호 2: 150회 재생
pop 장르는 3,100회 재생되었으며, pop 노래는 다음과 같습니다.

고유 번호 4: 2,500회 재생
고유 번호 1: 600회 재생
따라서 pop 장르의 [4, 1]번 노래를 먼저, classic 장르의 [3, 0]번 노래를 그다음에 수록합니다.
