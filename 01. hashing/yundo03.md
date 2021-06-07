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

<pre>
def solution(participant, completion):
    participant.sort()
    completion.sort()
    for par, com in zip(participant, completion):
        if par != com:
            return par
    return participant[-1]
</pre>
