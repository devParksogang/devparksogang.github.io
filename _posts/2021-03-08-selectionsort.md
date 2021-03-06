---
layout: singlee
title: "TIL210308: 자료구조 1강_선택정렬, 이항계수, 순열 
---

오늘은 자료구조 첫 수업을 한 날로, 선택정렬, 이항계수, 순열을 다루었다.

선택정렬(Selection Sort)은 정렬되지 않은 데이터(여기서는 대수 비교가 가능한 숫자)를 정렬하는 기법이다. 정렬되지 않은 부분(unsorted part)에서 최솟값을 '선택'해서 정렬된 부분의 가장 
오른쪽으로 이동시키기 때문에 선택정렬이라는 이름이 쓰인다. 

여기선 숫자 5개가 정렬되지 않은 채 담겨 있는 배열을 선택정렬해보자. 

---c
#include <stdio.h>
int main(void)
{
	int arr[5];
	arr[0] = 5;
	arr[1] = 3;
	arr[2] = 1;
	arr[3] = 2;
	arr[4] = 4;
	//정렬되지 않은 배열 __ 5, 3, 1, 2, 4  
}
---

우선 선택정렬 함수를 만든다. 선택정렬 함수는 데이터가 담겨 있는 배열의 포인터와 배열의 크기를 parameter로 받는다. 


---c
void select_sort(int* arr, int n)
---

함수 안에서는 n개의 데이터를 총 n번의 반복을 통해서 최솟값을 arr[0], arr[1]...에 순차적으로 저장한다.
즉 arr[0]부터 arr[n-1]을 순차적으로 탐색하면서 최솟값이 담긴 인덱스를 min변수에 저장하고, arr[0]의 값과 arr[min]의 값을 교환해주는 것이다. 

위의 배열은 최초의 시행을 거친 후 다음의 결과를 가지게 된다. 

5, 3, 1, 2, 4 -->  1, 3, 5, 2, 4 (5와 1의 위치가 바뀜)

이제 0번째 인덱스는 정렬이 끝났으므로 1번째 인덱스의 데이터부터 선택정렬을 다시 시작한다. 이러한 과정을 마지막에서 두 번째 인덱스까지 정렬을 마칠 때까지 반복한다. 
마지막에서 두 번째 인덱스까지 정렬을 마칠 경우 마지막 인덱스는 최댓값으로서 저절로 정렬이 된 것이다. 
이를 c언어로 구현된 코드로 표현하면 다음과 같다. 다음은 iterative 기법을 이용한 코드이다.

---c
---

iterative 기법 안에서는 for문이 nested 되는데, 바깥쪽 반복문은 처음부터 끝까지 순차적으로 정렬하기 위함이고, 안쪽 반복문은 정렬된 부분을 제외한 정렬되지 않은 부분에서
최솟값을 찾아서 정렬된 부분 바로 다음 인덱스의 값과 switch해주기 위함이다. 따라서 바깥쪽 for문이  i = 0 에서부터 i = n-2까지 반복한다면, 안쪽 for문은 j = 1부터 j = n -1까지 
반복하게 된다. 

---c
void select_sort(int* arr, int n)
{
	int i, j, min, temp; 
	
	for(i = 0; i < n-1; i++)
	{
		arr[min] = arr[i];
		for(j = 1; j < n; j++)
		{
			if (arr[j] < arr[min])
				arr[min] = arr[j];	
		}
		temp = arr[i];
		arr[i] = arr[min];			
		arr[min] = temp;
	 } 
}
---

이 코드는 틀린 코드이다. 컴파일해보면 잘못된 결과를 출력한다. 최솟값을 찾을 때 정렬되지 않은 부분(해결해야 할 부분) 안에서'만' 찾아야 한다. 정렬된 부분은 우리가 더 이상 건드려선 
안되는 부분이다. 위 코드에서는 안쪽 for 부분에서 j = 1으로 매번 초기화됨으로써 i = 1 초과의 상황, 즉 i = 1의 인덱스의 정렬을 이미 마친 상황에서도 최솟값을 j = 1에서부터 찾고 있다. 
따라서 해당 부분의 코드는 다음과 같이 바뀌어야 한다. 

---c
		for(j = i+1; j < n; j++)
---

i는 이번 회기에 정렬되어지는 인덱스이다. 따라서 최솟값을 찾는 안쪽 반복문은 i + 1에서부터 시작되어야 한다. 최초의 코드는 이를 정확히 인지하지 못한 나의 실수이다. 

선택정렬은 최초에 n개의 데이터에 대해서 최솟값을 선택한 뒤, 그 다음 n-1개, n-2개의 데이터에 대해서 최솟값을 선택하는 문제의 크기가 점점 작아지는 구조이다. 따라서 Recursion을
통해서도 선택정렬을 구현할 수 있다. 다음 문서에는 recursion을 이용한 선택정렬에 대해서 알아볼 것이다. 

