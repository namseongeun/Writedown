# sw 문제해결 응용

## BFS 알고리즘

1. **방문할 곳을 기록할 리스트를 생성하고 큐를 생성!**
2. **시작점을 큐에 삽입하고**
3. **큐가 비어있지 않은 경우에는 큐의 첫번째 원소를 반환해서**
4. **그 원소와 연결된 노드들에 대해 만약 조건에 맞는 경우에는 큐에 넣어서 다시 거기서부터 탐색을 할 수 있도록 한다.**

---

```python
bfs():
    [0] queue, visited 등 필요한 요소를 초기화
    [1] queue에 초기데에터(들) 삽입
    	visited 배열 표시 및 필요작업
        
while queue:		# 큐에 데이터 있는 동안 반복
    [2] queue에서 데이터 1개 꺼냄		# 종료조건의 경우 이곳에서 처리(return)
    [3] 4/8방향, 연결된 경우 등 '조건'에 맞으면 큐에 삽입
    	next 좌표/ 노드 계산해서 범위내 '조건'에 맞으면
        queue에 next 삽입, visited 체크 및 필요작업
        
# 더 이상 찾을 수 없는 경우 또는 모든 처리 완료 후
return 특정 값
```

---

- 미로, 특정위치/ 조건까지의 거리/ 단계 수 등 한단계 씩 탐색하면서 답을 찾는 경우
- 기본적으로 각 단게 진행에 필요한 비용이 동일한 경우에 중복방문을 방지하면서 진행
  - 경우에 따라 중복방문을 허용하면서 진행하는 경우도 있음



## 1238. Contact

```python
import sys


sys.stdin = open('Contact.txt')

def bfs(start_node):
    queue = [start_node]
    visited = [0] * 101     # 방문여부와 노드의 깊이를 나타내는 리스트

    visited[start_node] = 1
    result = start_node

    while queue:
        v = queue.pop(0)

        if visited[result] < visited[v] or (visited[result] == visited[v] and result < v):
            result = v

        for j in range(1, 101):
            if contact_mat[v][j] == 1 and visited[j] == 0:
                queue.append(j)
                visited[j] = visited[v] + 1

    return result

for tc in range(1, 11):
    N, Start = map(int, input().split())
    contact_list = list(map(int, input().split()))

    contact_mat = [[0] * 101 for _ in range(101)]

    for i in range(0, len(contact_list), 2):
        contact_mat[contact_list[i]][contact_list[i+1]] = 1

    answer = bfs(Start)

    print(f"#{tc} {answer}")
```



## 1861. 정사각형 방

```python
import sys


sys.stdin = open('Square_room.txt')

def bfs(K):
    visited = []
    queue = [K]

    while queue:
        v = queue.pop(0)
        if v not in visited:
            visited.append(v)
            if v in adj_graph.keys():
                queue.append(adj_graph[v])

    return visited

T = int(input())
for tc in range(1, T+1):
    N = int(input())
    grid = [list(map(int, input().split())) for _ in range(N)]

    # 델타: 상하좌우
    di = [-1, 1, 0, 0]
    dj = [0, 0, -1, 1]

    adj_graph = {}

    for i in range(N):
        for j in range(N):
            for d in range(4):
                ni = i + di[d]
                nj = j + dj[d]
                if 0 <= ni < N and 0 <= nj < N and grid[i][j] + 1 == grid[ni][nj]:
                    adj_graph[grid[i][j]] = grid[ni][nj]

    real_result = []
    for key in adj_graph.keys():
        result = bfs(key)
        if len(result) > len(real_result) or (len(result) == len(real_result) and result[0] < real_result[0]):
            real_result = result

    print(f"#{tc} {real_result[0]} {len(real_result)}")
```



## 1953. [모의 SW 역량테스트] 탈주범 검거

```python
import sys


sys.stdin = open('input.txt')

pipe = [[0,0,0,0], [1,1,1,1], [1,1,0,0], [0,0,1,1], [1,0,0,1], [0,1,0,1],[0,1,1,0],[1,0,1,0]]
di, dj = (-1,1,0,0), (0,0,-1,1)

def BFS(N, M, si, sj, L):
    q = []
    v = [[0]*M for _ in range(N)]
    
    q.append((si, sj))
    v[si][sj] = 1
    cnt = 1
    
    while q:
        ci, cj = q.pop(0)
        
        if v[ci][cj] == L:
            return ctn
        
        for k in range(4):
            ni, nj = ci + di[k], cj + dj[k]
            if 0 <= ni < N and 0 <= nj < N and v[ni][nj] == 0 and pipe[arr[ci][cj]][k] and pipe[arr[ni][nj]][opp[k]]:
                q.append((ni,nj))
                v[ni][nj] = v[ci][cj] + 1
                cnt += 1
        
        

T = int(input())
for tc in range(1, T+1):
    N, M, R, C, L = map(int, input().split())
    arr = [list(map(int,input().split())) for _ in range(N)]
    ans = BFS(N, M, R, C, L)
    print(f'#{tc} {ans}')
```



