
## Bubble Sort (버블 정렬)
- 서로 인접한 두 원소 비교, 조건에 따라 자리 교환 및 정렬
- 시간 복잡도 O(N^2)
- 구현이 간단하고 직관적, 지정된 배열 내에서 교환하므로 추가적인 메모리 필요 없음


## Selection Sort (선택 정렬)
- 해당 순서에 원소 넣을 위치는 정해져 있고 어떤 원소 넣을지 선택하는 알고리즘
- 시간 복잡도 O(N^2)
- 단순 비교 횟수는 많지만 교환 횟수가 비교적 적음, 추가적 메모리 공간 필요 없음


## Insertion Sort (삽입 정렬)
- 선택 정렬과 유사하지만 좀 더 효율적임
- 두 번째 원소부터 시작하여 그 앞의 원소들과 비교하면서 삽입 위치를 지정함
- 시간 복잡도 O(N^2) , 모두 정렬되어 있는 경우 O(N)


## Quick Sort (퀵 정렬)
- 배열 중 하나의 원소를 선택함 (Pivot)
- 피벗을 기준으로 왼쪽은 작은 원소, 오른쪽은 큰 원소들이 오도록 함
- 피벗을 제외하고 다음 차례에 같은 작업을 재귀적으로 반복함
- 시간 복잡도 O(N^2), 최선의 경우 O(NlogN)으로 개선 (제일 많이 쓰이는 정렬 방식)
- 최악인 경우는 피벗이 항상 제일 크거나 작은 경우 → 한쪽이 텅 비어버림
- 장점 : 데이터 접근 시 배열 내에서 발생하므로 캐시에 이미 로드된 데이터에 접근할 가능성이 높아져 캐시 적중률이 높아 전체 실행시간이 단축됨 (머지 소트보다 퀵 소트 쓰는 이유)


## Merge Sort (병합 정렬)
- 요소 쪼갠 후 다시 합병하면서 정렬해나가는 방식 (순차 접근)
- 시간 복잡도 O(NlogN)
- 퀵 정렬은 피벗을 통해 쪼개고 정렬함
- 병합 정렬은 쪼갤 수 있을 때 까지 쪼개고 정렬하면서 합침
- 순차 접근이므로 LinkedList의 정렬이 필요할 때 효율적 



## Heap Sort (힙 정렬)
- 완전 이진트리를 기본으로 하는 힙 자료구조 기반 정렬 방식
- 시간 복잡도 모두 O(NlogN)
- 최대, 최소값 구할 때 유용함


## Binary Search (이분 탐색)
- 탐색 범위를 두 부분으로 분할하면서 찾는 방식
- O(logN)


## BFS & DFS
- DFS
	- 루트 노드 혹은 임의 노드에서 다음 브랜치로 넘어가기 전, 해당 브랜치를 모두 탐색하는 방법
	- 모든 경로 방문 시 적합
- BFS
	- 루트 노드 혹은 임의 노드에서 인접한 노드부터 먼저 탐색하는 방법
	- 최소 비용 (모든 곳 탐색이 아닌, 최소 비용 구할 때)에 적합



## 다익스트라
- 최단 경로 탐색 알고리즘
- 특정 정점 → 다른 모든 정점으로 가는 최단 경로를 기록
- 해당 정점 까지 최단 거리 저장 및 방문 여부 저장을 통해 최단 거리 도출