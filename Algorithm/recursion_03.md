# 순환(Recursion)의 개념과 기본 예제 3

> **영리한 프로그래밍을 위한 알고리즘 강좌** ([💻인프런](https://www.inflearn.com/course/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EA%B0%95%EC%A2%8C)) - 부경대학교 IT융합응용공학과 권오흠 교수님
> 

## Designing Recursion - 순환 알고리즘의 설계

### 순환적 알고리즘 설계

- 적어도 하나의 base case, 즉 순환되지 않고 종료되는 case가 있어야 함
- 모든 case는 결국 base case로 수렴해야 함
- **암시적(implicit) 매개변수를 명시적(explicit) 매개변수로 바꾸어라.**

### 순차 탐색

- 이 함수의 미션은 data[0]에서 data[n-1] 사이에서 target을 검색하는 것이다. 하지만 검색 구간의 시작 인덱스 0은 보통 생략한다. 즉 **암시적 매개변수**이다.

```java
int search(int [] data, int n, int target) {
	for (int i=0; i < n; i++)
		if(data[i] == target)
			return i;
	return -1;
}
```

### 매개변수의 명시화 : 순차 탐색

- 이 함수의 미션은 data[begin]에서 data[end] 사이에서 target을 검색한다. 즉, **검색 구간의 시작점을 명시적(explicit)으로 지정**한다.
- 이 함수를 search(data, 0, n-1, target)으로 호출한다면 앞 페이지의 함수와 완전히 동일한 일을 한다.

```java
int search(int [] data, int begin, int end, int target) {
	if(begin > end)
		return -1;
	else if (target == items[begin])
		return begin;
	else
		return search(data, begin+1, end, target);
}
```

### 순차 탐색 : 다른 버전

- 이 함수의 미션은 data[begin]에서 data[end] 사이에서 target을 검색한다. 즉, 검색 구간의 시작점을 명시적(explicit)으로 지정한다.

```java
int search(int [] data, int begin, int end, int target) {
	if(begin > end)
		return -1;
	else if (target == items[end])
		return end;
	else
		return search(data, begin, end-1, target);
}
```

### 순차 탐색 : 다른 버전

- binary search와는 다름

```java
int search(int [] data, int begin, int end, int target) {
	if(begin > end)
		return -1;
	else {
		int middle = (begin + end) / 2;
		if (data[middle] == target)
			return middle;
		int index = search(data, begin, middle-1, target);
		if(index != -1)
			return index;
		else
			return search(data, middle+1, end, target);
	}
}
```

### 매개변수의 명시화 : 최대값 찾기

- 이 함수의 미션은 data[begin]에서 data[end] 사이에서 최대값을 찾아 반환한다.
begin ≤ end라고 가정한다.
- 최대값은 전체에서 **첫번째 값**과 **첫번째 값을 제외한 나머지 중 최대값**을 비교
- base case는 데이터 개수가 1개인 경우

```java
int findMax(int [] data, int begin, int end) {
	if (begin == end)
		return data[begin];
	else
		return Math.max(data[begin], findMax(data, begin+1, end));
}
```

### 최대값 찾기 : 다른 버전

```java
int findMax(int [] data, int begin, int end) {
	if (begin == end)
		return data[begin];
	else {
		int middle = (begin / end) / 2;
		int max1 = findMax(data, begin, middle);
		int max2 = findMax(data, middle+1, end);
		return Math.max(max1, max2);
	}
}
```

### Binary Search

- Binary Search은 데이터들이 크기순으로 정렬되어 배열에 저장되어 있을 때 적용할 수 있는 검색 방법
- items[begin]에서 items[end] 사이에서 target을 검색한다.
- Binary Search에서 매개변수를 암시적으로 표시할 경우 Recursion 내부에서 순환함수를 호출할 수 있는 방법이 없어진다.

```java
public static int binarySearch(String[] items, String target, int begin, int end) {
	if(begin > end)
		return -1;
	else {
		int middle = (begin + end) / 2;
		int compResult = target.compareTo(items[middle]);
		if (compResult == 0)
			return middle;
		else if (compResult < 0)
			return binarySearch(items, target, begin, middle-1);
		else
			return binarySearch(items, target, middle+1, end);
	}
}
```

### 순환적 알고리즘 설계하는데 중요한 것

- 적어도 하나의 base case가 있어야 하고, 모든 case는 base case로 수렴해야 함
- **암시적(implicit) 매개변수를 명시적(explicit) 매개변수로 바꾸어라.**
