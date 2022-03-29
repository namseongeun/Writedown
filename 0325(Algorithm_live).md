# sw 문제해결 응용

## DFS를 이용한 완전 탐색!

1. **문제를 보고 가능한 모든 경우의 수로 루트 노드부터 트리 형식으로 도식화를 그려보기**
2. **부모 노드와 자식노드까지의 간선들만 생각해서 함수를 define 해보기**
   - **이때 중요한 것은 종료조건을 설정하고 반환값을 잘 설정하기**
   - **종료조건에 도달하기 전까지 어떤 방식으로 나아갈 것인가를 잘 로직을 구현하는 것**
3. **자식노드들에 대해서 필요한 파라미터를 생각해서 가능한 경우의 수에 대해 재귀 함수를 호출하기**

4. **메인 코드에서 최상위 노드의 값으로 함수 호출하기**



## 104. Stack_2 연습2 부분집합

```python
def DFS(n, sum):
    global sol
    
	if >= N:	# 종료조건
        if sum == K:	# 종료시점에 정답처리
            sol += 1
        return	
    
    DFS(n+1, sum + lst[n])	# 숫자를 포함하는 경우
    DFS(n+1, sum)	# 숫자를 포함하지 않는 경우
    
    
T = int(input())
for tc in range(1, T+1):
    N, K = map(int, input().split())
    lst = list(map(int, input().split()))
    sol = 0
    DFS(0, 0)
    print(f'#{tc} {sol}')
```



## 4012. 요리사

```python
def DFS(n, alst, blst):
    global ans
    if n == N:
        if len(alst) == len(blst):		# 두 요리 재료의 개수가 같은 경우
            asum = bsum = 0
            for i in range(len(alst)):
                for j in range(len(alst)):		# 각재료의 모든 시너지 조합을 누적
                    asum += arr[alst[i]][alst[j]] 
                    bsum += arr[blst[i]][blst[j]]
            if ans > abs(asum - bsum):		# 정답갱신
                ans = abs(asum - bsum)
        return
    
    DFS(n + 1, alst + [n], blst)
    DFS(n + 1, alst, blst + [n])
    
    
T = int(input())
for tc in range(1, T+1):
    N = int(input())
    arr = [list(map(int, input().split())) for _ in range(N)]
    ans = 12345678
    DFS(0, [], [])
    print(f"#{tc} {ans}")
```



## 2819. 격자판의 숫자 이어 붙이기

```python
import sys


sys.stdin = open('input.txt')

def DFS(n, ci, cj, num):
    if n == 7:
        sset.add(num)
        return
    
    for di, dj in ((-1,0 ), (1,0), (0,-1), (0,1)):
        ni, nj = ci+di, cj+dj
        if 0 <= ni < 4 and 0 <= nj < 4:
            DFS(n+1, ni, nj, num * 10 + arr[ni][nj])
            
            
T = int(input())        
for tc in range(1, T+1):
    arr = [list(map(int, input().splti())) for _ in  range(4)]
    sset = set()
    for i in range(4):
        for j in range(4):
            DFS(0, i , j, 0)
    print(f"#{tc} {len(sset)}")
```



## 1952. [모의 SW 역량테스트] 수영장

```python
import sys


sys.stdin = open('input.txt')
def DFS(n, ssum):
    global ans  # ans를 계속 갱신해야함
    if n > 12:
        if ans > ssum:
            ans = ssum
        return
    
    # 가능한 모든 경우에 대해 재귀 호출 (여기서는 일권/월권/3개월권/연간권)
    DFS(n+1, ssum + lst[n] * day)
    DFS(n+1, ssum + mon)
    DFS(n+3, ssum + mon3)
    DFS(n+12, ssum + year)
       

T = int(input())
for tc in range(1, T+1):
    day, mon, mon3, year = map(int,input().split())
    lst = [0] + list(map(int, input().split()))
    ans = 12345678
    DFS(1, 0)		# 항상 가장 루트의 상태를 넣는다.
    
    print(f"#{tc} {ans}")
```

---

```python
def DFS(n, total):
    global ans
	# 종료조건(n이 12보다 커지면)
    if n > 12:
        if total < ans:		# 1년을 다 돌았을 때 결과가 현재의 ans보다 낮은 수라면
            ans = total		# 결과를 갱신한다.
        return
    
    # 재귀 호출(가능한 모든 경우의 수)
    # 그 다음 상태 호출할 때 파라미터 생각 잘 해볼 것!!
    DFS(n+1, total + plan[n] * price[0])
    DFS(n+1, total + price[1])
    DFS(n+3, total + price[2])
    DFS(n+12, total + price[3])
    
    
T = int(input())
for tc in range(1, T+1):
    price = list(map(int, input().split()))
    plan = [0] + list(map(int, input().split()))
    ans = 10000000
    DFS(1, 0)		# 가장 초기 상태(1월부터 0원으로 시작)
    
    print(f"#{tc} {ans}")
```





## 2105. [모의 SW 역량테스트] 디저트 카페

```python
import sys


sys.stdin = open('input.txt')

def DFS(n, ci, cj, v, cnt):
    global si, sj, ans
    if n>3:		# 종료조건
        return
    
    if n==3 and ci==si and cj==sj and ans<cnt:		# 정답갱신
        ans = cnt
        return
    
    for k in range(n, n+2):
        ni, nj = ci = ci + di[k], cj + dj[k]
        if 0 <= ni < N and 0 <= nj < N and arr[ni][nj] not in v:
            DFS(k, ni, nj, v + [arr[ni][nj]], cnt + 1)
            

di, dj = (1, 1,-1,-1), (-1,1,1,-1)        
T = int(input())
for tc in range(1, T+1):
    N = int(input())
    arr = [list(map(int, input().split())) for _ in range(N)]
    ans = -1
    for i in range(N):
        for j in range(N):
            DFS(0, i, j, [], 0)
   	print(f'#{tc} {ans}')
```

---

**백트래킹**

```python
import sys


sys.stdin = open('input.txt')

def DFS(n, ci, cj, v, cnt):
    global si, sj, ans
    if n>3:		# 종료조건
        return
    
    if n==3 and ci==si and cj==sj and ans<cnt:		# 정답갱신
        ans = cnt
        return
    
    for k in range(n, n+2):
        ni, nj = ci = ci + di[k], cj + dj[k]
        if 0 <= ni < N and 0 <= nj < N and arr[ni][nj] not in v:
            v.append(arr[ni][nj])
            DFS(k, ni, nj, v, cnt + 1)
            v.pop()
            

di, dj = (1, 1,-1,-1,1), (-1,1,1,-1,-1)
T = int(input())
for tc in range(1, T+1):
    N = int(input())
    arr = [list(map(int, input().split())) for _ in range(N)]
    ans = -1
    for i in range(N):
        for j in range(N):
            DFS(0, i, j, [], 0)
   	print(f'#{tc} {ans}')
```

---

**백트래킹 + 가지치기**

```python
import sys


sys.stdin = open('input.txt')

def DFS(n, ci, cj, v, cnt):
    global si, sj, ans
    # 가지치기
    if n == 2 and ans >= cnt*2:
        return
    
    # 종료조건
    if n>3:
        return
    
    # 정답갱신
    if n==3 and ci==si and cj==sj and ans<cnt:
        ans = cnt
        return
    
    # DFS 재귀
    for k in range(n, n+2):
        ni, nj = ci = ci + di[k], cj + dj[k]
        if 0 <= ni < N and 0 <= nj < N and arr[ni][nj] not in v:
            v.append(arr[ni][nj])
            DFS(k, ni, nj, v, cnt + 1)
            v.pop()
            

di, dj = (1, 1,-1,-1,1), (-1,1,1,-1,-1)
T = int(input())
for tc in range(1, T+1):
    N = int(input())
    arr = [list(map(int, input().split())) for _ in range(N)]
    ans = -1
    for i in range(N):
        for j in range(N):
            DFS(0, i, j, [], 0)
            
   	print(f'#{tc} {ans}')
```



