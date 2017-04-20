# Search 

Search에는 여러 종류의 Search 접근 법이 있겠지만 크게 Uninformed Search, Informed Search로 2가지로 나눌 수 있다.

# Uniformed search

- BFS (Breath - first)
- DFS (Depth - first)
- Brute Force
- Hill climbing
- Dijkstra

# Informed Search
- Greedy Best First Search ( huristics )
- A*

# Complete, Optimal

어떤 문제에 대한 해를 구할 수 있는 알고리즘은 Comlete하다라고 하며,

구한 해가 최적의 해이면 Optimal 하다고 한다.

# BFS 

엣지간의 가중치가 없다면 Complete, Optimal 하다

모든 레벨의 경우의 수를 체크하기 때문

Queue를 이용해서 구현한다.


# DFS

엣지간의 가중치가 없다면 Complete, Optimal 하지 않다.


### Copleteness 하지 않은 이유는,

![dfs-tree](https://mhesham.files.wordpress.com/2010/04/300pxdepthfirsttree-svg_thumb.png?w=449&h=288)

트리를 탐색할 때 문제의 해가 root 근처의 우측 sub tree에 존재한다고 가정해보자.

탐색을 좌측부터 파고 들어 가는데, 좌측 sub tree의 bound는 정해져있지 않으며, 무한하게 확장된다고 한다면

dfs는 절대 해를 찾을 수 없을 것이다.

해의 Depth가 높게 위치한다면, 빠른 시간 내에 찾을 수 있다.

# Brute Force

모든 경우의 수를 체크해서 값을 비교한다.

n개의 문자열에서 m개의 문자열 찾기

O(nm)

# Search를 사용 할 수 있는 Case 

1. Single source shortest pass (SSSP)
한점에서 시작해서 가장 빠른 경로를 찾는 알고리즘

2. No negative wieghts
Priorty Q : A B C D E (A의 가중치를 0으로 둔다)

while(모든 방문)
{
    Priorty Q의 값을 갱신한다
    최소 가중치를 선택한다
    노드 가중치를 갱신한다
}

# 휴리 스틱

AI 란? 
Algorithm + Heuristics(경험/지식)
                        Heuristic function h(n)

f(n) = evaluation function

노드간의 cost가 있고, 노드와 목적지간의 직선거리 (휴리 스틱)
여러 도시에서 목적지에서 직선거리가 가까워지는 도시만을 선택해서 감

 # Greedy best first sounds
 휴리스틱만을 이용해서 탐색

Complete 하지 않다. Backtracking 하는 다른 알고리즘이 필요함
해를 찾아도 Optimal 하지 않다.

# A *

A* f(n) = g(n) + h(n) 
실제값 누적 + 휴리스틱 값

Complete? O
Optimal? O