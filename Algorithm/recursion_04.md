# Recursion의 응용 - 미로찾기 1

> **영리한 프로그래밍을 위한 알고리즘 강좌** ([💻인프런](https://www.inflearn.com/course/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EA%B0%95%EC%A2%8C)) - 부경대학교 IT융합응용공학과 권오흠 교수님
> 

## Maze - 미로찾기

- 크기를 N × N으로 가정
- 출구의 위치는 (N-1, N-1)로 가정
- 흰색이 지나갈 수 있는 통로, 파란색이 벽
- 입구에서 출구까지 가는 경로를 찾는 문제

![Maze](./Algorithm/img/maze_01.png)

### Recursive Thinking

- 현재 위치에서 출구까지 가는 경로가 있으려면
    1. 현재 위치가 출구이거나(이미 내가 출구에 도착해있거나) 혹은
    2. 이웃한 셀들 중 하나에서 **현재 위치를 지나지 않고** 출구까지 가는 경로가 있거나
    - 둘 중 하나가 성립되어야 출구까지 가는 경로가 있다고 할 수 있다.

### 미로찾기(Decision Problem) - 답이 yes or no인 문제

- 출발점에서 출구까지 가는 경로가 있느냐 없느냐의 문제라고 생각한다.
- (x, y)와 이웃한 (x’, y’) 셀은 최대 4개
    - (x’, y’) 각각에 대해서, pathway가 아닌 경우는 생각할 필요가 없다.
    - (x’, y’)가 pathway라면 (x’, y’)에서 다시 출구까지 가는 경로가 있는지 recursion으로 검사
- recursion을 작성할 때 가장 기본적으로 고려해야 할 것은 무한 루프에 빠지지 않는가 이다.
    - Base case가 반드시 있어야 하고, Base case로 수렴하는가를 확인해야 한다.
    - 아래 코드의 경우 pathway인 두 셀이 무한루프에 빠질 수 있다.

```
boolean findPath(x, y)
	if (x, y) is the exit
		return true;
	else
		for each neighbouring cell (x', y') of (x, y) do
			if (x', y') is on the pathway
				if findPath(x', y')
					return true;
		return false;
```

- 따라서 x, y의 인접한 셀 x’, y’에서 다시 x, y로 돌아오는 것은 막아줄 필요가 있다.
    - 이미 방문한 위치와 방문하지 않은 위치를 구분하여 처리한다.
    - 현재 위치가 이미 방문한 위치임을 표시
    - 조건문에서 각각의 인접한 셀을 pathway 여부와 함께 방문한 셀인지의 여부를 체크하기 때문에 무한 루프에 빠지지 않는다.

```
boolean findPath(x, y)
	if (x, y) is the exit
		return true;
	else
		mark (x, y) as a visited cell;
		for each neighbouring cell (x', y') of (x, y) do
			if (x', y') is on the pathway and not visited
				if findPath(x', y')
					return true;
		return false;
```

- 다른 방법으로 x, y에 인접한 셀에 대해 recursion을 호출한 다음 , recursion의 시작 지점에서 pathway 여부와 방문여부를 체크하는 방법이 있다.
    - 위의 코드보다 recursion이 호출되는 횟수는 더 많아지지만 코드는 더 간결해지는 면이 있다.

```
boolean findPath(x, y)
	if (x, y) is either on the wall or a visited cell
		return false;
	else if (x, y) is the exit
		return true;
	else
		mark (x, y) as a visited cell;
		for each neighbouring cell (x', y') of (x, y) do
			if findPath(x', y')
				return true;
		return false;
```

### Maze - Java로 구현

- 사람이 다닐 수 있는 통로는 0, 벽은 1
- `PATH_COLOR` : `visited`이며 아직 출구로 가는 경로가 될 가능성이 있는 cell
- `BLOCKED_COLOR` : `visited`이며 출구까지의 경로상에 있지 않음이 밝혀진 cell
- 어떤 cell을 방문하면 일단 `PATH_COLOR`로 칠하고, 출구까지의 경로상에 있지 않음이 밝혀진 경우 `BLOCKED_COLOR`로 칠한다.

```java
public class Maze {
	private static int N = 8;
	private static int [][] maze = {
		{0, 0, 0, 0, 0, 0, 0, 1},
		{0, 1, 1, 0, 1, 1, 0, 1},
		{0, 0, 0, 1, 0, 0, 0, 1},
		{0, 1, 0, 0, 1, 1, 0, 0},
		{0, 1, 1, 1, 0, 0, 1, 1},
		{0, 1, 0, 0, 0, 1, 0, 1},
		{0, 0, 0, 1, 0, 0, 0, 1},
		{0, 1, 1, 1, 0, 1, 0, 0}
	};
	
	private static final int PATHWAY_COLOR = 0;  // white
	private static final int WALL_COLOR = 1;  // blue
	private static final int BLOCKED_COLOR = 2;  // red
	private static final int PATH_COLOR = 3;  // green

	public static boolean findMazePath(int x, int y) {
		if (x < 0 || y < 0 || x >= N || y >= N) // x, y 좌표가 유효한 범위가 아닌 경우
			return false;
		else if (maze[x][y] != PATHWAY_COLOR) // 이미 방문했거나 벽인 경우
			return false;
		else if (x == N-1 && y == N-1) { // 출구인 경우
			maze[x][y] == PATH_COLOR;
			return true;
		} else {
			maze[x][y] = PATH_COLOR; // 일단 출구로 가는 경로가 될 가능성이 있음
			if (findMazePath(x-1, y) || findMazePath(x, y+1)
				|| findMazePath(x+1, y) || findMazePath(x, y-1)) { // x, y에 인접한 4개의 위치 각각에 대해 출구까지 갈 수 있는지 체크
				return true;
			}
			maze[x][y] = BLOCKED_COLOR; // 여기에 도달했다는 것 = 이미 방문한 셀을 지나지 않고서는 출구까지 가는 경로가 없음
			return false;
		}
	}

	public static void main(Strin[] args) {
		printMaze();
		findMazePath(0, 0);
		printMaze();
	}
}
```
