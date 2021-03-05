---
layout: post
title: "Union-Find Algorithm"
---

오늘은 며칠 동안 막히는 부분이었던 union-find algorithm을 공부했다.
교재에 있는 코드는 이해가 어려워서 유튜버 '동빛나'님의 강의와 코드를 참조하였다.

union-find algorithm은 두 원소가 속한 각 집합을 서로 합치고(union), 두 원소가 같은 집합에 포함되어 있는지를 찾는(find) 알고리즘이다. 
서로소 집합(disjoint set)을 찾는 데 쓰이고, 그래프가 싸이클을 이루는지를 확인하는데에 쓰인다.

그래프의 각 노드는 다른 노드와 연결 관계에 있는데, 연결되어 있는 부분 집합의 경우 노드의 숫자가 가장 작은 노드를 루트 노드로 해서 루트 노드 집합에 저장한다. 루트 노드가 같은 원소(노드)
들은 서로 연결되어 있는 노드라고 할 수 있다.

이렇게 각 노드의 루트노드를 찾기 위해서는 재귀적인 함수 호출이 필요하다. 즉 노드 a의 루트노드를 찾기 위해선 노드a와 연결된 노드b 함수의 루트노드를 찾아나가면서 노드의 숫자가 가장 작은
루트 노드를 찾아 나가는 것이다. 

```c
int get_parent(int parent[], int a)
{
  if (parent[a] == a) return a;
  parent[a] = getparent(parent, parent[a]);
}
```

또한 두 노드가 포함된 각 집합을 합치는 알고리즘을 생각해볼 수 있다. 가령 {1}과 {2} 두 집합이 있을 때 union_parent(parent, 1, 2) 함수를 호출하면 {1, 2}로 합쳐지는 것이다.

```c
int union_parent(int parent[], int a, int b)
{
    a = get_parent(parent, a);  // a와 b의 부모노드를 찾아서 a, b 각각에 저장한다. 새로운 루트노드 변수를 만들지 않는 것은 변수를 재활용함으로써 메모리를 아끼는 것인가 싶다. 
    b = get_parent(parent, b);
    if (a > b) parent[a] = b; // 루트노드는 연결된 노드 중 가장 작은 노드가 되므로, 더 작은 루트노드를 가지고 있는 쪽으로 다른쪽이 편입된다. 이 경우 b가 더 작은 루트노드이므로 a의 루트노드가 b가 된다. 
    else parent[b] = a;
}
```
3번째 줄에서 a는 parameter a의 루트노드이다. 즉 parameter a가 속한 집합의 우두머리인 셈이다. 그 우두머리가 3번째 줄에서 다른 집합의 우두머리 b를 루트노드로 삼겠다고 선언했으니 
루트노드 a의 '똘마니'들도 b를 루트노드로 '섬기게' 될 것이다. 단 우두머리 a의 똘마니들이 바로 동기화되지 않는다는 문제가 이 함수에 있다. 


이렇게 하면 두 노드가 같은 루트노드를 공유하는지, 즉 서로 연결되어 있는지를 확인하는 함수도 만들 수 있다. 
```c
int find_parent(int parent[], int a, int b)
{
  a = get_parent(parent, a);
  b = get_parent(parent, b); // a와 b의 루트노드를 찾아서 a, b에 할당한다. 
  if (a == b) //루트노드가 같으면(연결되어 있으면)
    return 1;
   else
    return 0;
```

따라서 메인함수에서는 다음과 같인 검증할 수 있다.
```c
int main(void)
{
  int parent[11]; // 크기가 10인(노드를 1부터 시작시킴) parent배열 - 각 노드의 루트노드를 저장함- 선언
  for(int i = 1; i < 11; i ++)
  {
    parent[i] = i; // 최초에 서로 연결되지 않은 각 노드들은 자기 자신이 루트노드인 셈이다. 
  }
  
  union_parent(parent, 1, 2);
  union_parent(parent, 3, 2);
  union_parent(parent, 4, 3);
  union_parent(parent, 5, 6);
  union_parent(parent, 6, 7); // 1- 2- 3- 4 / 5-6-7 과 같이 연결됨
  
  printf("%d", get_parent(parent, 4)); //4
  printf("%d", find_parent(parent, 3, 7)); // 0
  
  union_parent(parent, 2, 6);
  
  printf("%d", find_parent(parent, 3, 7); 
  ```


 이제 union-find algorithm을 학습하였으므로 '최소 신장 트리' 라는 개념을 학습할 수 있겠다. 다음 TIL은 일요일에 게재된다. 

