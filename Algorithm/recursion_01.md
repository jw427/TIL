# 순환(Recursion)의 개념과 기본 예제 1

> **영리한 프로그래밍을 위한 알고리즘 강좌** ([💻인프런](https://www.inflearn.com/course/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EA%B0%95%EC%A2%8C)) - 부경대학교 IT융합응용공학과 권오흠 교수님
> 

## Recursion

자기 자신을 호출하는 함수 혹은 자기 자신을 다시 호출하는 메서드

우리말로 순환 또는 재귀함수라고 한다.

```java
void func(...)
{
	...
	func(...);
	...
}
```

### 예1 - 무한루프

```java
public class Code01 {
	public static void main(String [] args) {
		func();
	}

	public static void func() {
		System.out.println("Hello...");
		func();
	}
}
```

```
Hello...
Hello...
Hello...
Hello...
...
```

### 예2 - recursion은 항상 무한루프에 빠질까?

- recursion이 항상 무한루프에 빠지는 것은 아님
    - **Base case** : 적어도 하나의 recursion에 빠지지 않는 경우가 존재해야 한다.
    - **Recursive case** : recursion을 반복하다 보면 결국 base case로 수렴해야 한다.

```java
public class Code02 {
	public static void main(String [] args) {
		int n = 4;
		func(n);
	}

	public static void func(int k) {
		if (k <= 0)
			return;
		else {
			System.out.println("Hello...");
			func(k-1);
		}
	}
}
```

```
Hello...
Hello...
Hello...
Hello...
```

### 예3 - 1~n까지의 합

```java
public class Code03 {
	public static void main(String [] args) {
		int result = func(4);
	}

	public static int func(int n) {
		if (n == 0)
			return 0;
		else
			return n + func(n-1);
	}
}
```

```
10
```

### recursion의 해석

- 이 함수의 mission은 0~n까지의 합을 구하는 것이다.
- n=0이라면 합은 0이다.
- n이 0보다 크다면 0에서 n까지의 합은 0에서 n-1까지의 합에 n을 더한 것이다.

```java
public static int func(int n) {
	if (n == 0)
		return 0;
	else
		return n + func(n-1);
}
```

### 순환함수와 수학적귀납법

- 정리 : `func(int n)`은 음이 아닌 정수 n에 대해서 0에서 n까지의 합을 올바로 계산한다.
- 증명
    1. `n=0`인 경우 : `n=0`인 경우 0을 반환한다. 올바르다.
    2. 임의의 양의 정수 k에 대해서 `n<k`인 경우 0에서 n까지의 합을 올바르게 계산하여 반환한다고 가정하자.
    3. `n=k`인 경우를 고려해보자. `func`은 먼저 `func(k-1)` 호출하는데 2번의 가정에 의해서 0에서 k-1까지의 합이 올바로 계산되어 반환된다. 메서드 `func`은 그 값에 n을 더해서 반환한다. 따라서 메서드 `func`은 0에서 k까지의 합을 올바로 계산하여 반환한다.

## Factorial : n!

- $0! = 1$
- $n! = n \times (n-1)! \ \ \ \ n>0$
- Factorial이 가지고 있는 순환적인 구조를 그대로 이용해 아래와 같은 코드를 작성할 수 있다.

```java
public static int factorial(int n)
{
	if(n == 0)
		return 1;
	else
		return n * factorial(n-1);
}
```

### 순환함수와 수학적귀납법

- 정리 : `factorial(int n)`은 음이 아닌 정수 n에 대해서 `n!`을 올바로 계산한다.
- 증명
    1. `n=0`인 경우 : `n=0`인 경우 1을 반환한다. 올바르다.
    2. 임의의 양의 정수 k에 대해서 `n<k`인 경우 `n!`을 올바르게 계산한다고 가정하자.
    3. `n=k`인 경우를 고려해보자. `factorial`은 먼저 `factorial(k-1)` 호출하는데 2번의 가정에 의해서 `(k-1)!`이 올바로 계산되어 반환된다. 따라서 메서드 `factorial`은 `k*(k-1)!=k!`을 반환한다.

## $X^n$

- $X^0 = 1$
- $X^n = X*X^{n-1} \ \ \ \ if \ n>0$

```java
public static double power(double x, int n) {
	if (n == 0)
		return 1;
	else
		return x * power(x, n-1);
}
```

## Fibonacci Number

- $f_0 = 0$
- $f_1 = 1$
- $f_n = f_{n-1} + f_{n-2} \ \ \ \ n>1$

```java
public int fibonacci(int n) {
	if (n < 2)
		return n;
	else
		return fibonacci(n-1) + fibonacci(n-2);
}
```

## 최대공약수 : Euclid Method

- `m≥n`인 두 양의 정수 m과 n에 대해서 m이 n의 배수이면 `gcd(m, n) = n`
- 그렇지 않으면 `gcd(m, n) = gcd(n, m % n)`이다.
⇒ m과 n의 최대공약수는 n과 m을 n으로 나눈 나머지 사이의 최대공약수와 같다.

```java
public static double gcd(int m, int n) {
	if (m < n) {
		int tmp = m; m = n; n = tmp; // swap m and n
	}
	if (m % n == 0)
		return n;
	else
		return gcd(n, m % n);
}
```

### Euclid Method : 좀 더 단순한 버전

- $gcd(p, q)$
  - $p \ \ \ \ \ \ if \ \ q = 0$
  - $gcd(q, p$ `%` $q) \ \ \ \ \ \ otherwise.$

```java
public static int gcd(int p, int q) {
	if (q == 0)
		return p;
	else
		return gcd(q, p%q);
}
```
