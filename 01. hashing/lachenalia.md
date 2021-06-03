# Hash
## 완주하지 못한 선수: 42576
### 문제 설명
* input: participant(마라톤 참가자 리스트), completion(마라톤 완주자 리스트)
* output: 완주하지 못한 선수의 이름
* 단 1명의 선수만 완주하지 못했다.
* 선수의 이름은 중복될 수 있다.

### 접근방법
선수의 이름이 중복될 수 있으므로, 각 선수가 완주했는지 여부를 in 연산자 등을 이용하여 확인하는 것은 불가능하다.
참가자와 완주자 리스트를 모두 정렬하게 되면, 완주하지 못한 선수의 순서에서 완주자와 참가자의 이름이 달라지게 되므로 완주하지 못한 선수의 이름을 알 수 있다.

### Code
```
def solution(participant, completion):
    # 참가자와 완주자 모두 정렬
    participant.sort()
    completion.sort()
    for i in range(len(completion)):
        # 참가자의 이름이 완주자와 다른 경우 완주하지 못한 선수
        if participant[i] != completion[i] : return participant[i]
    # for문이 마지막까지 돌았다면 마지막 선수가 완주하지 못함
    return participant[len(completion)]
```
## 전화번호 목록: 42577
### 문제 설명
* input: phone_book(전화번호 리스트)
* output: boolean
* 한 번호가 다른 번호의 시작부분과 일치하는 경우가 있으면 False를 반환한다.

### 접근방법
숫자가 아닌 문자로 입력값이 주어진다.
문자를 정렬하게 되면, 앞에서부터 문자의 오름차순으로 정렬되므로, 만약 한 번호가 다른 번호의 시작부분과 일치한다면 해당 번호는 앞뒤로 위치하게 된다.
따라서 정렬 후, 각 번호의 뒤 번호의 시작부분이 앞 번호와 일치하는지 여부를 확인한다.

### Code
```
def solution(phone_book):
    # 정렬한다.
    phone_book.sort()
    for i in range(len(phone_book) - 1):
        a = phone_book[i]
        b = phone_book[i+1]
        # 만약 뒷 번호의 시작부분이 앞 번호와 같다면 False를 반환한다.
        if a in b and a == b[:len(a)]:
            return False
    return True
```

## 위장: 42578
### 문제 설명
* input: clothes([옷, 옷의 종류] 형태의 리스트를 원소로 갖는 리스트)
* output: 옷의 조합 갯수
* 옷의 종류별로 최대 1가지만 입을 수 있다.
* 최소 1개의 옷을 입어야 한다.

### 접근방법

옷의 종류마다 입을 수 있는 경우의 수는 갯수 + 1 이다.
(빨강, 파랑, 노랑의 3벌의 바지가 있다면 [안입음, 빨강, 파랑, 노랑] 의 4가지 경우의 수 존재)

전체 옷의 종류별 갯수를 구한 후, 각 갯수에 1을 더해 곱하면 전체 가능한 조합의 갯수를 구할 수 있고, 아무 옷도 입지 않는 경우인 1을 빼면 정답을 구할 수 있다.

### Code
```
def solution(clothes):
    # 옷의 종류를 구하기 위한 Dictionary 생성
    clothes_dic = {}
    # 옷의 종류별 갯수를 세어 dictionary에 입력
    for c in clothes:
        if clothes_dic.get(c[1]):
            clothes_dic[c[1]] += 1
        else:
            clothes_dic[c[1]] = 1
            
    # 곱셈 계산을 위한 초기값 1 지정
    answer = 1
    # 옷의 종류별 갯수에 1을 더해 곱하여 전체 경우의 수 계산
    for k in clothes_dic.keys():
        answer *= (clothes_dic[k] + 1)
    
    # 아무 옷도 입지 않는 경우를 제외하기 위해 1을 빼서 출력
    return answer - 1
```

## 베스트 앨범: 42579
### 문제 설명
* input: genres(각 곡의 장르), plays(각 곡의 재생수)
* output: 장르별 재생수에 따른 1, 2위곡 리스트
* 가장 많이 재생된 장르가 우선된다.
* 각 장르에서 가장 많이 재생된 2곡만 수록한다. (1곡밖에 없는 경우 1곡만 수록)

### 접근방법
장르의 이름을 key로 갖는 dictionary를 생성한다.
이때 value의 값은 [전체 재생수, 1위곡 번호, 2위곡 번호] 로 한다.
dictionary의 value 값을 전체 재생수로 정렬한 후, 1, 2위곡 번호를 순차적으로 리스트에 담아 리턴한다.

### Code
```
def solution(genres, plays):
    dic = {}
    # genres: [total_plays, first, second]
    for i in range(len(genres)):
        g = genres[i]
        if dic.get(g):
            dic[g][0] += plays[i]
            
            # 현재 곡이 1번 곡보다 많이 재생된 경우
            if plays[i] > plays[dic[g][1]]:
                dic[g] = [dic[g][0], i, dic[g][1]]
            # 2번 곡이 없거나 현재 곡이 2번 곡보다 많이 재생된 경우
            elif dic[g][2] == -1 or plays[i] > plays[dic[g][2]]: 
                dic[g] = [dic[g][0], dic[g][1], i]
        else:
            dic[g] = [plays[i], i, -1]
    best_song = sorted(dic.items(), key=lambda x: x[1][0], reverse = True)
    answer = []
    for s in best_song:
        answer.append(s[1][1])
        # 2번 곡이 있는 경우 수록 (장르별 1곡만 있는 경우 존재)
        if s[1][2] != -1:
            answer.append(s[1][2])
    return answer
```
