# Algorithm List_2

---

### Array 2 (배열 2)

---

- **2차원 배열**

  - 2차원 배열의 선언

    - 1차원 List를 묶어놓은 List
    - 2차원 이상의 다차원 List는 차원에 따라 Index를 선언
    - 2차원 List의 선언: 세로길이(행의 개수), 가로길이(열의 개수)를 필요로 함 
    - python에서는 데이터 초기화를 통해 변수선언과 초기화가 가능함
    - 예시 (2행 4열의 2차원 List)

    ```python
    arr = [[0,1,2,3],[4,5,6,7]]
    ```

  - **2차원 배열 입력받기**

    ```python
    # 공백이 있는 경우
    arr = [list(map(int, input().split())) for _ in range(N)]
    
    # 공백이 없는 경우
    arr = [list(map(int, input())) for _ in range(N)]
    ```

  - **배열 순회**

    - n x m 배열의 n*m개의 모든 원소를 빠짐없이 조사하는 방법

  - **행 수선 순회**

    ```python
    # i 행의 좌표
    # j 열의 좌표
    for i in range(n):
    	for j in range(m):
            Array[i][j]
            # 필요한 연산 수행
    ```

  - **열 우선 순회**

    ```python
    # i 행의 좌표
    # j 행의 좌표
    for j in range(m):
        for i in range(n):
            Array[i][j]
            # 필요한 연산 수행
    ```

  - **지그재그 순회**

    ```python
    # i 행의 좌표
    # j 행의 좌표
    for i in range(n):
        for j in range(m):
            Array[i][j + (m-1-2*j) * (i%2)]
            # 필요한 연산 수행
          
    # 짝수 행일 때는 j만 남으므로 정상 순회, 홀수 행일 때는 m-1부터 0까지 순회     
    ```

  - **델타를 이용한 2차 배열 탐색**

    - 2차 배열의 한 좌표에서 4방향의 인접 배열 요소를 탐색하는 방법

      ```
      # 수도 코드
      # N x N 배열
      arr[0...N-1][0...N-1]
      # 상하좌우
      di[] <- [0,0,-1,1]
      dj[] <- [-1,1,0,0]
      for i : 1 -> N-1 :
      	for j : 1 -> N-1:
      		for k in range(4):
      			ni <- i + di[k]
      			nj <- j + dj[k]
      			if 0 <= ni N and O <= nj < N:
      			# 유효한 인덱스면 
      			test(arr[ni][nj])
      			
      ```

    - 상하좌우를 인덱스 0 1 2 3 으로 생각해서 인덱스 `[x][y]`에 각각 어떤 수를 더할지를 생각해서 리스트 설정하고 더해주기

      ```python
      # 우 0 하 1 좌 2 상 3 인 경우
      # 현재 위치: arr[i][j]
      di = [0,1,0,-1]
      dj = [1,0,-1,0]
      for k in range(4):
          ni = i + di[k]
          nj = j + dj[k]
          # 만약 인덱스를 벗어나는 경우
          if 0<ni<N and 0<nj<M:
              print(arr[ni][nj])
              
      # 또는
      for di, dj in [(0,1),(1,0),(0,-1),(-1,0)]:
          ni = i + di
          nj = j + dj
          if 0 <= ni <N and 0 <= nj < M:
              arr[ni][nj]
      ```

    - 살사용예

      ```python
      arr = [[1,2,3],[4,5,6],[7,8,9]]
      N = 3
      
      for i in range(N):
          for j in range(M):
              for di,dj in [(-1,0),(1,0),(0,-1),(0,1)]:
                  ni = i + di
                  nj = j + dj
                  if 0<=ni<N and 0<=nj<N:
                      print(i,j,arr[ni][nj])
      ```




- **전치행렬**

  ```python
  # i : 행의 좌표, len(arr)
  # j : 열의 좌표, len(arr[0])
  arr = [[1,2,3],[4,5,6],[7,8,9]]		# 3x3 행렬
  
  for i in range(3):
      for j in range(3):
          if i < j:
              arr[i][j], arr[j][i] = arr[j][i], arr[i][j]
             
  ```



- Tip! 2차원 배열 상,좌에 0으로 감싸고 싶은 경우

  ```python
  '''
  0 0 0 0
  0 1 2 3
  0 4 5 6
  0 7 8 9
  '''
  
  arr2 = [[0]*(N+2)] + [[0] + list(map(int, input().split())) for _ in rnge(N)]
  ```




- **부분집합 생성**

  - 유한 개의 정수로 이루어진 집합이 있을 때, 이 집합의 부분집합 중에서 그집합의 원소를 모두 더한 값이 0이 되는 경우가 있는지를 알아내는 문제

  - 완전검색 기법으로 부분집합 합 문제를 풀기 위해서는 우선 집합의 모든 부분집합을 생성한 후에 각 부분집합의 합을 게산해야 한다.

  - 부분집합의 수

    - 집합의 원소가 n개일 때, 공집합을 포함한 부분집합의 수는 2**n 개
    - 이는 각 원소를 부분집합에 포함시키거나 포함시키지 않는 2자기 경우를 모든 원소에 적용한 경우의 수와 같다.

  - 각 원소가 부분집합에 포함되었는지를 loop 이용하여 확인하고 부분집합을 생성하는 방법

    (원소가 적을 때)

    ```python
    bit = [0,0,0,0]
    for i in range(2):
        bit[0] = i
        for j in ragne(2):
            bit[1] = j
            for k in range(2):
                bit[2] = k
                for l in range(2):
                    bit[3] = l
                    print_subset(bit)                
    ```

    ```python
    bit = [0]*3
    for i in range(2):
        bit[0] = i
        for j in ragne(2):
            bit[1] = j
            for k in range(2):
                bit[2] = k
                for p in range(3):
                    if bit[p]:
                        print(A[p], end=" ")
                print()
    ```

  - **비트 연산자(원소가 많을 때)**

    - 비트연산자 

      - `&`  : 비트 단위로 AND 연산을 한다.
      - `|`  : 비트 단위로 OR 연산을 한다.
      - `<<` : 피연산자의 비트 열을 왼쪽으로 이동시킨다.
      - `>>` : 피연산자의 비트 열을 오른쪽으로 이동시킨다.
      - n<<m
        - n의 2진수를 <<쪽으로 m칸 민다.
        - 밀어진 빈칸에는 0이 추가됨
        - 2진수 오른쪽에 0이 m칸 추가된다
        - 자릿수가 m만큼 상승한다.
        - 10진수에는 n * (10 **m)느낌 각요소에 접근하기 

    - `<<` 연산자

      - 1 << n : 2**n 즉, 원소가 n개일 경우의 모든 부분 집합의 수를 의미한다.

    - `&` 연산자

      - i & (1 << j): i 의 j 번째 비트가 1인지 아닌지를 검사한다.

    - 보다 간결하게 부분집합을 생성하는 방법

      ```python
      arr = [3,6,7,1,5,4]
      n = len(arr)		# n: 원소의 개수
      
      for i in range(1<<n):					# 1<<n: 부분집합의 개수
          for j in range(n):					# 원소의 수만큼 비트를 비교함
              if i & (1<<j): 					# i의 j번 비트가 1인경우
                  print(arr[j], end =", ")	# j번 원소 출력
          print()
      print()    
      ```



- **순차 검색(Sequential Search)**

  - 일렬로 되어있는 자료를 순서대로 검색하는 방법

    - 가장 간단하고 직관적인 검색방법
    - 배열이나 연결리스트 등 순차 구조로 구현된 자료구조에서 원하는 항목을 찾을 때 유용함
    - 알고리즘이 단순하여 구현이 쉽지만 검색대상의  수가 많은 경우에는 수행시간이 급격히 증가하여 비효율적임

  - 2가지 경우

    - 정렬되어 있지 않는 경우

      - 검색과정

        - 첫번째 원소부터 순서대로 검색 대상과 키 값이 같은 원소가 있는지 비교하며 찾는다.
        - 키값이 동일한 원소를 찾으면 그 원소의 인덱스를 반환한다.
        - 자료구조의 마짐가에 이를 때까지 검색 대상을 찾지 못하면 검색 실패

      - 찾고자 하는 원소의 순서에 따라 비교회수가 결정됨

        - 첫번째 원소를 찾을 때는 1번비교, 두 번째 원소를 찾을 때는 2번 비교

        - 정렬되지 않은 자료에서의 순차검색의 평균 비교 횟수

          `(1/n) * (1+2+3+ ... +n) = (n+1)/2`

        - 시간 복잡도: O(n)

          ```python
          def sequentilaSearch(a,n,k):
              i = 0
              while i < n and a[i] != k:
                  i = i + 1
          	if i < n:
                  return i
              else:
                  return -1
          ```

    - 정렬되어 있는 경우

      - 검색과정

        - 자료가 오름차순으로 정렬된 상태에서 검색을 실시한다고 가정하자.
        - 자료를 순차적으로 검색하면서 키 값을 비교하여 원소의 키 값이 검색 대상의 키 값보다 크면 찾는 원소가 없다는 것이므로 더 이상 검색하지 않고 검색을 종료한다.

      - 찾고자 하는 원소의 순서에 따라 비교회수가 결정됨

        - 정렬 되어있으므로 검색 실패를 반환하는 경우 평균 비교회수가 반으로 줄어든다.

        - 시간 복잡도 O(n)

          ```python
          def sequentilaSearch(a,n,k):
              i = 0
              while i < n and a[i] < k:
                  i = i + 1
          	if i < n and a[i] == k:
                  return i
              else:
                  return -1
          ```

          

- **바이너리 서치(Binary Search)**

  - 자료의 가운데에 있는 항목의 키 값과 비교하여 다음 검색의 위치를 결정하고 검색을 계속 진행하는 방법

    - 목적키를 찾을 때까지 이진 검색을 순환적으로 반복 수행함으로써 검색 범위를 반으로 줄여가면서 보다 빠르게 검색을 수행함

  - 이진 검색을 하기 위해서는 자료가 정렬된 상태여야 한다.

  - 검색 과정

    - 자료의 중앙에 있는 원소를 고른다.
    - 중앙 원소의 값과 찾고자 하는 목표 값을 비교한다.
    - 목표 값이 중앙 원소의 값보다 작으면 자료의 왼쪽 반에 대해서 새로 검색을 수행하고 크다면 자료의 오른쪽 반에 대해서 새로 검색을 수행한다.
    - 찾고자 하는 값을 찾을 때까지 과정을 반복한다.

  - 구현

    - 검색 범위의 시작점과 종료점을 이용하여 검색을 반복 수행한다.

      ```python
      def binarySearch(a, N, Key):
          start = 0
          end = N-1
          while start <= end:
              middle = (start + end) // 2
              if a[middle] == key:	# 검색 성공
                  return true
              elif a[middle] > key:
                  end = middle - 1
      		else:
                  start = middle + 1
      	return false				# 검색 실패            
      ```

  - 재귀함수를 이용한 구현

    - 아래와 같이 재귀 함수를 이용하여 이진 검색을 구현할 수도 있다.

    - 재귀 함수에 대해서는 나중에 더 자세히!

      ```python
      def binarySearch(a, low, high, K):
          if low > high: 	# 검색 실패
              return False
          else:
              middle = (low + high) // 2
              if key == a[middle]: 	# 검색 성공
                  return true
              elif key < a[middle]:
                  return binarySearch2(a, low, middle-1, key)
              elif a[middle] < key:
                  return binarySearch2(a, middle+1, high, key)
      ```

      

- **선택정렬 (Selection Sort)**

  - 주어진 자료들 중 가장 작은 값의 원소부터 차레대로 선태갛여 위치를 교환하는 방식

    - 앞서 살펴본 셀렉션 알고리즘을 전체 자료에 적용한 것

  - 정렬과정

    - 주어진 리스트 중에서 최소값을 찾는다.
    - 그 값을 리스트의 맨 앞에 위치한 값과 교환한다.
    - 맨 처음 위치를 제외한 나머지 리스트를 대상으로 위의 과정을 반복한다.

  - 시간 복잡도

    - O(n2)

  - 정렬과정

    - 주어진 리스트에서 최솟값을 찾는다.

    - 리스트의 맨 앞에 위치한 값과 교환한다.

    - 미정렬 리스트에서 최솟값을 찾는다.

    - 리스트의 맨앞에 위치한 값과 교환한다.

    - 반복

      ```python
      def selectionSort(a, N):
          for i in ragne(N-1):
              minIdx = i
              for j in range(i+1,N):
                  if a[minIdx] > a[j]:
                      minIdx = j
          	a[i], a[minIdx] = a[minIdx], a[i]        
      ```



- **셀렉션 알고리즘(Selection Algorithm)**

  - 저장되어 있는 자료로부터 k번째로 큰 혹은 작은 원소를 찾는 방법을 셀렉션 알고리즘이라 한다.

  - 최소값, 최대값, 혹은 중간값을 찾는 알고리즘을 의미하기도 한다.

  - 선택과정

    - 셀렉션은 아래와 같은 과정을 통해 이루어진다.
      - 정렬 알고리즘을 이용하여 자료 정렬하기
      - 원하는 순서에 있는 원소 가져오기

  - 예: k번째로 작은 원소를 찾는 알고리즘

    - 1번부터 k번째까지 작은 원소들을 찾아 배열의 앞쪽으로 이동시키고, 배열의 k번째를 반환한다.

    - k가 비교적 작을때 유용하며 O(kn)의 수행시간을 필요로 한다.

      ```python
      def select(arr, k):
          fori in range(k):
              minIndex = i
              for j in range(i+1, len(arr)):
                  if arr[minIndex] > arr[j]:
                      minIndex = j
      		arr[i], arr[minIndex] = arr[minIndex], arr[i]
      	return arr[k-1]        
      ```




- 버블 정렬과 선택 정렬 차이점 서술하기
  - 선택정렬: 교환의 횟수가 버블, 삽입정렬보다 작다.