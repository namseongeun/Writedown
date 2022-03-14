# Queue

## **큐(Queue)**

**큐의 특성**

- **스택과 마찬가지로 삽입과 삭제의 위치가 제한적인 자료구조**
- **선입선출구조 (FIFO: First In First Out)**

- **선입선출**

  ```python
  append(elem), pop(0)
  ```

- 데이터의 삽입이 일어나는 곳: **rear**

- 데이터의 삭제가 일어나는 곳: **front**

- 데이터 삽입 연산: **enqueue(ADDQ)**

- 데이터 삭제 연산: **dequeue(DELETEQ)**



**큐의 연산** 

- **enQueue(item)** :큐의 뒤쪽(rear 다음)에 원소를 삽입하는 연산
- **deQueue()**: 큐의 앞쪽(front)에서 원소를 삭제하고 반환하는 연산
- **CreateQueue**: 공백 상태의 큐를 생성하는 연산
- **IsEmpty()** : 큐가 고백상태인지를 확인하는 연산
- **IsFull()** : 큐가 공백상태인지를 확인하는 연산
- **AddQ** (=enQueue)
- **DeleteQ** (=deQueue)
- **Qpeek()** : 큐의 앞쪽(front)에서 원소를 삭제 없이 반환하는 연산



- **AddQ 구현**

  ```python
  n = 5
  rear = -1
  front = -1
  queue = [None] * n
  
  
  rear += 1
  queue[rear] = "A"
  
  rear += 1
  queue[rear] = "B"
  
  rear += 1
  queue[rear] = "C"
  
  print(queue)
  
  ```

- **DeleteQ 구현**

  ```python
  front += 1
  queue[front] = None
  
  front += 1
  queue[front] = None
  
  front += 1
  queue[front] = None
  
  print(queue)
  ```

- **IsEmpty와 IsFull 구현**

  ```python
  """
  is_empty
  True or False를 리턴
  front == rear 일때 True
  """
  
  """
  is_full
  True or False를 리턴
  rear == size-1 dlfEo True
  """
  ```




## 선형큐

- **1차원 배열을 이용한 큐**
  - **큐의 크기 = 배열의 크기**
  - **front: 저장된 첫 번째 원소의 인덱스 (꺼내진 위치)**
  - **rear: 저장된 마지막 원소의 인덱스**



- **상태 표현**
  - 초기상태: front = rear = -1
  - 공백상태: front == rear
  - 포화상태: rear == n-1 (n: 배열의 크기, n-1: 배열의 마지막 인덱스)



- **초기 공백 큐 생성**
  - 크기 n인 1차원 배열 생성
  - front와 rear를 -1로 초기화



- **삽입**

  - 마지막 원소 뒤에 새로운 원소를 삽입하기 위해

    1. rear 값을 하난 증가시켜 새로운 원소를 삽입할 자리를 마련
    2. 그 인덱스에 해당하는 배열 원소 Q[rear]에 tiem을 저장

    ```python
    def enQueue(item):
        global rear
        if isFull():
            print("Queue_Full")
        else:
            rear += 1
            Q[rear] = item
    ```

- **삭제**

  - 가장 앞에 있는 원소를 삭제하기 위해
    1. front 값을 하나 증가시켜 큐에 남아있게 될 첫 번째 원소 이동
    2. 새로운 첫 번째 원소를 리턴함으로써 삭제와 동일한 기능

- **공백상태 및 포화상태 검사**

  - 공백상태: front == rear
  - 포화상태: rear == n-1 (n: 배열의 크기, n-1: 배열의 마지막 인덱스)

- **검색 (Qpeek())**

  - 가장 앞에 있는 원소를 검색하여 반환하는 연산

  - 현재 front의 한자리 뒤에 있는 원소, 즉 큐의 첫번째에 있는 원소를 반환

    ```python
    def Qpeek():
        if isEmpty():
            print("Queue Empty")
        else:
            return Q[front + 1]
    ```

    

- **선형 큐 이용시 문제점**

  - **잘못된 포화상태 인식**

    - 선형 큐를 이용하여 원소의 삽입과 삭제를 계속할 경우 배열의 앞부분에 활용할 수 있는 공간이 있음에도 불구하고 rear=n-1인 상태, 즉 포화상태로 인식하여 더 이상의 삽입을 수행하지 않게 된다.

    - **해결 방법 1**

      - 매 연산이 이루어질 때마다 저장된 원소들을 배열의 앞부분으로 모두 이동시킴
      - 원소 이동에 많은 시간이 소요되어 큐의 효율서이 급격히 떨어진다.

    - **해결 방법 2**

      - 1차원 배열을 사용하되, 논리적으로는 배열의 처음과 끝이 연결되어 원형 형태의 큐를 이룬다고 가정하고 사용

        

## 원형큐

- **원형큐의 구조**

  - **초기 공백 상태**

    - front = rear = 0

  - **Index의 순환**

    - front와 rear의 위치가 배열의 마지막 인덱스인 n-1를 가리킨 후, 그 다음에는 논리적 순환을 이루어 배열의 처음 인덱스인 0으로 이동해야 함

  - **front 변수**

    - 공백 상태와 포화상태 구분을 쉽게 하기 위해 front가 있는 자리는 사용하지 않고 항상 빈자리로 둔다.

  - **삽입 위치 및 삭제 위치**

    - **선형큐**

      - 삽입위치

        ```python
        rear += 1
        ```

      - 삭제위치

        ```python
        front += 1
        ```

    - **원형큐**

      - 삽입위치

        ```python
        rear = (rear + 1) % n
        ```

      - 삭제위치

        ```python
        front = (front + 1) % n
        ```



- **초기 공백 큐 생성**
  - 크기 n인 1차원 배열 생성
  - front와 rear를 0으로 초기화



- **공백상태 및 포화상태 검사**

  - 공백상태: front = rear

  - 포화상태: 삽입할 rear의 다음 위치 == 현재 front - (rear + 1) % n == front

    ```python
    def isEmpty():
        return front == rear
    
    def isFull():
        return (rear+1) % len(cQ) == front
    ```

- **삽입**

  - 마지막 원소 뒤에 새로운 원소를 삽입하기 위해 rear값을 조정하여 새로운 원소를 삽입할 자리를 마련함

  - 그 인덱스에 해당하는 배열원소 cQ[rear]에 item을 저장

    ```python
    def enQueue(item):
    	global rear
        if isFull():
            print("Queue Full")
        else:
            rear = (rear + 1) % len(cQ)
            cQ[rear] = item
    ```

- **삭제**

  - 가장 앞에 있는 원소를 삭제하기 위해 front 값을 조정하여 삭제할 자리를 준비하고 새로운 front 원소를 리턴함으로써 삭제와 동일한 기능을 한다.

    ```python
    def deQueue():
        global front
        if isEmpty():
            print("Queue Empty")
        else:
            front = (front + 1) % len(cQ)
            return cQ[front]
    ```




## 우선순위 큐 (Priority Queue)

- **우선순위 큐의 특성**
  - 우선순위를 가진 항목들을 저장하는 큐
  - FIFO 순서가 아니라 우선순위가 높은 순서대로 먼저 나가게 된다.
- **우선순위 큐의 적용분야**
  - 시뮬레이션 시스템
  - 네트워크 트래픽 제어
  - 운영체제의 테스크 스케줄링



- **우선순위 큐의 구현**
  - **배열을 이용한 우선순위 큐**
    - 배열을 이용하여 자료 저장
    - 원소를 삽입하는 과정에서 우선순위를 비교하여 적절한 위치에 삽입하는 구조
    - 가장 앞에 최고 우선순위의 원소가 위치하게 됨
    - 문제점
      - 배열을 사용하므로, 삽입이나 삭제 연산이 일어날 때 원소의 재배치가 발생함
      - 이에 소요되는 시간이나 메모리 낭비가 큼

  - **리스트를 이용한 우선순위 큐**



- **우선순위 큐의 기본 연산**
  - **삽입**
  - **삭제**



## 버퍼 (Buffer)

- **큐의 활용: 버퍼**
  - **데이터를 한 곳에서 다른 한 곳으로 전송하는 동안 일시적으로 그 데이터를 보관하는 메모리의 영역**
  - 버퍼링: 버퍼를 활용하는 방식 또는 버퍼를 채우는 동작을 의미
- **버퍼의 자료구조**
  - 버퍼는 일반적으로 **입출력 및 네트워크와 관련된 기능에서 이용**된다.
  - 순서대로 입력/출력/전달되어야 하므로 FIFO 방식의 자료구조인 큐가 활용된다.



## 데큐 (DEQ: Doubly-Ended Queue)

- **큐는 큐인에 addq랑 deleteq를 양 끝에서 모두 일어날 수 있는 큐**

- **모듈 가져오기**

  ```python
  from colloections import deque
  q = deque()
  ```

- **AddQ_FRONT**

  ```python
  q.appendleft()
  ```

- **AddQ_REAR**

  ```python
  q.append()
  ```

- **DeleteQ_FRONT**

  ```python
  q.popleft()
  ```

- **DeleteQ_REAR**

  ```python
  q.pop()
  
  '''
  데큐에서는 pop()에다가 인덱스를 쓸 수가 없다.
  '''
  ```

  

## BFS (Breadth First Search)

**그래프를 탐색하는 방법**

- **깊이 우선 탐색 (DFS: Depth First Search)**
- **너비 우선 탐색 (BFS: Breadth First Search)**



**BFS 알고리즘**

- 탐색 시작점의 인접한 정점들을 먼저 모두 차례로 방문한 후에 방문했던 정점을 시작점으로 하여 다신 인접한 정점들을 차례로 방문하는 방식

- 인접한 정점들에 대해 탐색을 한 후 차례로 다시 너비 우선탐색을 진행해야 하므로 선입 선출 형태의 자료구조인 큐를 활용함

- **입력 파라미터: 그래프 G와 탐색 시작점 v**

  ```python
  def BFS(G, v, n):
      visited = [0] * (n+1)
      queue = []
      queue.append(v)
      visited[v] = 1
      while queue:
          t = queue.pop(0)
          visit(f)
          for i in G[t]:
              if not visited[i]:
                  queue.append(i)
                  visited[i] = visited[n] + 1
      
  '''
  G: 그래프, v: 탐색 시작점, n: 정점의 개수
  큐를 생성하고 시작점을 큐에 삽입한다.
  반복문에서는 큐가 비어있지 않은 경우 큐의 첫번째 원소를 반환하고 그 원소가 연결된 모든 정점에 대해
  방문되지 않은 곳이라면 큐에 넣는다.
  그 후 n으로부터 1만큼 이동
  '''
  ```

- **연습문제 미로**

  ```python
  def fstart(N):
      for i in range(N):
          for j in range(N):
              if maze[i][j] == '2':
                  return i, j
  	return -1, -1   
  
  
  T =int(input())
  for tc in range(1, T+1):
      N = int(input())
      maze = [list(map(int, input())) for _ in range(N)]
      sti, sij = fstart(N)
      ans = bfs(sti)
      print(f"#{tc} {ans})
  ```

- **BFS 파이썬 코드**

  ```python
  while q:
      tmp = q.popleft()
      
      for node in graph[tmp]:
          if node not in visited:
              visited.append(node)
              q.append(node)
  
  print(visited)    
  ```









