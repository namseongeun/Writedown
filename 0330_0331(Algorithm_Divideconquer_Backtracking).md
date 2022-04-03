- - - 분할 정복 & 백트래킹
    
      ## 분할정복
    
      **분할정복 기법**
    
      - 설계 전략
        - 분할(Divide): 해결할 문제를 여러 개의 작은 부분을 나눈다.
        - 정복(Conquer): 나눈 작은 문제를 각각 해결한다.
        - 통합(Combine): 해결된 해답을 모은다.
    
      
    
      **거듭제곱**
    
      - 분할 정복 기반의 알고리즘 (O(logn))
    
        ```pseudocode
        Recursive_Power(x, n)
        	IF n == 1: RETURN x
        	IF n is even
        		y << Recursive_Power(x, n/2)
        		RETURN y * y
        	ELSE
        		y << Recursive_Power(x, (n-1)/2)
        		RETURN y * y * x
        ```
    
      
    
      **병합정렬(Merge Sort)**
    
      - 여러 개의 정렬된 자료의 집합을 병합하여 한 개의 정렬된 집합으로 만드는 방식
    
      - 분할 정복 알고리즘 활용
    
        - 자료를 최소 단위의 문제까지 나눈 후에 차례대로 정렬하여 최종 결과를 얻어냄
        - top-down 방식
    
      - 시간복잡도
    
        - O(nlogn)
    
      - 병합정렬 과정
    
        - 분할 단계: 전체 자료 집합에 대하여 최소 크기의 부분집합이 될 때까지 분할 작업을 계속한다.
        - 병합 단계: 2개의 부분집합을 정렬하면서 하나의 집합으로 병합
        - 8개의 부분집합이 1개로 병합될 때까지 반복함
    
      - 분할과정 알고리즘
    
        ```pseudocode
        merge_sort(LIST m)
        	IF length(m) == 1: RETURN m
        	
        	LIST left, right
        	middle << length(m) / 2
        	FOR x in m before middle
            	add x to left
        	FOR x in m after or equal middle
        		add x to right
        		
        	left << merge_sort(left)
            right << merge_sort(right)
            
        	RETURN merge(left, right)
        ```
    
      - 병합과정 알고리즘
    
        ```pseudocode
        merge(LIST left, LIST right)
        	LIST result
        	WHILE length(left) > 0 OR length(right) > 0
        		IF length(left) > 0 AND length(right) > 0 
        			IF first(left) <= first(right)
        				append popfirst(left) to result
        			ELSE
        				append popfirst(right) to result
        		ELIF length(left) > 0
        			append popfirst(left) to result
        		ELIF length(right) > 0
        			append popfirst(right) to result
        	REUTRN result
        
        ```
    
      
    
      **퀵 정렬**
    
      - 주어진 배열을 두 개로 분할하고 각각을 정렬한다.
      - 병합 정렬과 다른 점
        - 병합정렬은 그냥 두부분으로 나누는 반면에 퀵정렬은 분할할 때 기준 아이템 중시믕로 이보다 작은 것은 왼편 큰 것은 오른편에 위치시킨다.
        - 각 부분 정렬이 끝난 후 병합정렬은 병합이란 후처리 작업이 필요하나 퀵정렬을 필요로 하지 않는다.
    
      
    
      ## 백트래킹
    
      **백트래킹의 개념**
    
      - 여러 가지 선택지들이 존재하는 상황에서 한가지를 선택한다.
      - 선택이 이루어지면 새로운 선택지들의 집합이 생성된다.
      - 이런 선택을 반복하면서 최종 상테에 도달한다.
        - 올바른 선택을 계속하면 목표 상태(goal state)에 도달한다.
    
      
    
      - **당첨리프 노드찾기**
        - 루트에서 갈 수 있는 노드를 선택한다.
        - 꽝 노드까지 도달하면 최근의 선택으로 되돌아와서 다시 시작한다.
        - 더 이상의 선택지가 없다면 이전의 선택지로 돌아가서 다른 선택을 한다.
        - 루트까지 돌아갔을 경우 더 이상 선택지가 없다면 찾는 답이없다.
    
      
    
      - **백트래킹과 깊이 우선 탐색과의 차이**
        - 어떤 노드에서 출발하는 경로가 해결책으로 이어질 것 같지 않으면 더 이상 그 경로를 따라가지 않음으로써 시도의 횟수를 줄임 (Prunning 가지치기)
        - 깊이 우선 탐색이 모든 경로를 추적하는데 비해 백트래킹은 불필요한 경로를 조기에 차단
        - 깊이 우선 탐색을 가하기에는 경우의 수가 너무나 많음. 즉 N! 가지의 경우의 수를 가진 문제에 대해 깊이 우선 탐색을 가하면 당연히 처리 불가능한 문제
        - 백트래킹 알고리즘을 적용하면 일반적으로 경우의 수가 줄어들지만 이 역시 최악의 경우에는 여전히 지수함수 시간을 요하므로 처리 불가능
    
      
    
      - **8-Queens 문제**
        - 퀸 8개를 8x8 체스판 안에 서로 공격할 수 없도록 배치하는 모든 경우를 구하는 문제
        - 후보 해의 수: 4,426,165,368개
        - 실제 해의 수: 92개
        - 즉 44억 개가 넘는 후보의 수 속에서 92개를 최대한 효율적으로 찾아내는 조건
        - 루트 노드에서 리프 노드까지의 경로는 해답 후보가(candidate solution) 되는데, 깊이 우선검색을 하여 그 해답후보 중에서 해답을 찾을 수 있다.
        - 그러나 이 방법을 사용하면 해답이 될 가능성이 전혀 없는 노드의 후손 노드들도 모두 검색해야 하므로 비효율적이다.
    
      
    
      **백트래킹 기법**
    
      - 어떤 노드의 유망성을 점검한 후에 유망(promising)하지 않다고 결정되면 그 노드의 부무로 되돌아가 다음 자식 노드로 감
      - 어떤 노드를 방문하였을 때 그 노드를 포함한 경로가 해답이 될 수 없음녀 그 노드는 유망하지 않다고 하며, 반대로 해답의 가능성이 있으면 유망하다고 한다.
      - 가지치기(pruning): 유망하지 않은 노드가 포함되는 경로는 더 이상 고려하지 않는다.
    
      
    
      - **백트래킹의 과정**
        1. 상태 공간 트리의 깊이 우선검색을 실시한다.
        2. 각 노드가 유망한지를 정검한다.
        3. 만일 그 노드가 유망하지 않으면, 그 노드의 부모 노드로 돌아가서 검색을 계속한다.
    
      
    
      - **일반 백트래킹 알고리즘**
    
        ```pseudocode
        checknode (node V)
        	IF promising(v)
        		IF there is a solution at v
        			write the solution
        		ELSE
        			FOR each child u of v
        				checknode(u)
        ```
    
      
    
      **상태공간트리를 구축하여 문제를 해결**
    
      - {1,2,3}의 powerset을 구하는 백트래킹 알고리즘
    
        ```pseudocode
        backtrack(a[], k, input)
        	c[MAXCANDIDATES]
        	ncands
        	
        	IF k == input: process_solution(a[], k)
        	ELSE
        		k++
        		make_candidates(a[], k, input, c[], ncands)
        		FOR i in 0 ~ (ncands - 1)
        			a[k] << c[i]
        			backtrack(a, k, input)
        			
        
        main()
        	a[MAX] 					// powerset을 저장할 배열
        	backtrack(a[], 0 ,3)	// 3개의 원소를 가지는 powerset
        	
        
        make_candidates(a[], k, n ,c[], ncands)
        	c[0] << TRUE
        	c[1] << FALSE
        	ncands << 2
        	
        process_solution(a[], k)
        	FOR i in 1 ~ k
        		IF a[i] == TRUE : print(i)
        ```
    
      - 순열을 구하는 백트래킹 알고리즘
    
        ```pseudocode
        maek_candidates(a[], k, n, c[], ncands)
        	i_perm[NMAX] << FALSE
        	
        	FOR i in 1~(k-1)
        		in_perm[a[i]] << TRUE
        		
        	FOR i in 1~n
        		IF in_perm[i] == FALSE
        			c[ncands] << i
        			ncands++
        			
        			
        process_solution(a[], k)
         	FOR i in 1~k : print(a[i])
        ```
    
        
    
      ## 트리
    
      - 트리는 싸이클이 없는 **무향 연결 그래프**이다.
        - 두 노드 사이에는 유일한 경로가 존재한다.
        - 각 노드는 최대 하나의 부모 노드가 존재할 수 있다.
        - 각 노드는 자식 노드가 없거나 하나 이상이 존재할 수 있다.
    
      
    
      - **비선형 구조**
        - 원소들 간에 1:n 관계를 가지는 자료구조
        - 원소들 간에 계층 관계를 가지는 계층형 자료구조
    
      
    
      (나머지는 Algorithm_Tree와 동일)
