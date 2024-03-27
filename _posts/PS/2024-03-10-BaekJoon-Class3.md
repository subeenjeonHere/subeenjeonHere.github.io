---
title: BaekJoon Class-3
date: 2024-03-10
categories: PS
tags:
  [
    PS
  ]
---

# [문제 풀이 Github](https://github.com/subeenjeonHere/int-_Algorithms/issues/7)

---

# 동전
<summary> 
<details>

# ☻ 동전 0

2024년 3월 10일

## 문제

준규가 가지고 있는 동전은 총 N종류이고, 각각의 동전을 매우 많이 가지고 있다.

동전을 적절히 사용해서 그 가치의 합을 K로 만들려고 한다. 이때 필요한 동전 개수의 최솟값을 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 N과 K가 주어진다. (1 ≤ N ≤ 10, 1 ≤ K ≤ 100,000,000)

둘째 줄부터 N개의 줄에 동전의 가치 Ai가 오름차순으로 주어진다. (1 ≤ Ai ≤ 1,000,000, A1 = 1, i ≥ 2인 경우에 Ai는 Ai-1의 배수)

## 출력

첫째 줄에 K원을 만드는데 필요한 동전 개수의 최솟값을 출력한다.

## 예제 입력 1

```
10 4200
1
5
10
50
100
500
1000
5000
10000
50000

```

## 예제 출력 1

```
6

```

## 예제 입력 2

```
10 4790
1
5
10
50
100
500
1000
5000
10000
50000

```

## 예제 출력 2

```
12
```

---

## ☺︎ Snippets

```java
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer stk = new StringTokenizer(br.readLine(), " ");
        int[] info = new int[2];
        int i = 0;
        while (stk.hasMoreTokens()) {
            info[i] = Integer.parseInt(stk.nextToken());
            i++;
        }
        System.out.println(Arrays.toString(info));

        //동전 가치 저장
        int[] coin = new int[info[0]];
        for (int j = 0; j < coin.length; j++) {
            coin[j] = Integer.parseInt(br.readLine());
        }
        System.out.println(Arrays.toString(coin));

        //목표 가치
        int k = info[1];
        int sum = 0;
        int count = 0;
        int j = coin.length - 1;

        while (k >= 0) {
            System.out.println("Coin: " + coin[j]);

            //K보다 코인 가치가 크면 나눌 수 없으므로 다음 코인
            if (coin[j] > k) {
                j--;
                continue;
            }

            // K <=인 동전이 있으면 나누기
            // sum 은 나머지:  나머지가 음수거나, 나머지를 다시 나눌 수 있는 동전이 없다면
            // 이전에 나눴던 동전 1개를 더하고 카운트를 감소하며 최적값을 찾아가야 함
            // 코드 구현 생각하기
            count = k / coin[j];
            sum = k % coin[j];
            //k 가치를 다 채웠으면 종료
            if (sum == 0) {
                System.out.println(count);
                return;
            }
            //k가치가 0이 될 때까지 반복
            sum = sum + coin[j];
            count--;
            System.out.println("Coin, Sum, Count: " + coin[j] + " " + sum + " " + count);
        }
    }
}
```

저번에 그리디 설탕배달 풀 때 처럼, 조건문 내에서 제어하는 걸 구현하는 것에서 시간을 많이 소요하고 있다. 문제를 어떻게 풀어야 할지는 알겠는데, 의도대로 정확한 코드를 못 짜고 있다.

> 2차 시도 : 성공
>

```java
public class Main {
//중략
        while(j >=0)

    {
        //K보다 코인 가치가 크면 나눌 수 없으므로 다음 코인 사용 위해 j 하나 감소, 반복문 종료
        //k <= 코인 이라면 나눌 수 있다. 반복문에서 나눠질 수 있는 코인에 도달하게 된다. That means k를 나눌 수 있는 최대의 동전
        /**
         * @Point
         * -  최대의 동전으로 나누고 남은 가치를 나눌 수 있는 동전이 없을 때
         *  가치에 직전에 나눴던 동전 1개만큼을 다시 더해주고, Count를 하나 빼주고 (즉 1개 무효) 나눌 수 있는 동전이 있는지 체크
         * - 음수가 될 때
         */
        if (coin[j] <= k) {
            count += k / coin[j];
            remain = k % coin[j];
            k = remain;
            if (k == 0) {
                System.out.println(count);
                return;
            }
        }
        j--;

    }
}
}
```

~~굳이 복잡하게 생각하니 코드 작성이 안 됐고, 필요한 것만 생각해서 흐름대로 코드를 썼고 성공. 후 ㅜ~~

---

## 풀이 정리

### 목표

주어진 동전으로, K라는 목표 값을 달성해야 한다.(즉, 주어진 동전을 최소한으로 사용해서 0원으로 만들어야 한다.)

5kg의 배낭을 최대한 많이 사용해서 봉지의 수를 최소화 해야 했던 설탕 배달이랑 유사한 문제로, 그리디 알고리즘이다.

### 그리디

각 단계에서 항상 **'가장 큰 가치의 동전으로 나누는' 최적의 선택**을 해야한다. 각 단계에서 최선의 선택을 함으로써, 전체적으로 최적의 결과를 도출해내는 것이 목표이기 때문임.

### 어떻게 풀었나

`j`가 0 이상인 동안 while 반복문이 계속된다. 반복문 안에서 j 인덱스에 있는 동전의 가치와, 목표 가치가 계속 비교된다. → 주어진 동전들로 0원을 만들어야 하기 때문에, k가 0이 될 때까지 이 반복문은
종료될 수 없다.

### 시간 많이 소요했던 부분

제어문 컨트롤 하는 것, But Solved

### if, j 인덱스의 동전 가치가 k ≤ 라면

k 라는 목표 금액을 해당 동전으로 나눌 수 있다.

- 동전들이 오름차순으로 주어졌으니, **역순으로 탐색했다. 때문에 탐색 순서는 가장 가치가 큰 동전이 먼저 올 것이니, 나눌 수 있는 만큼 나눠준다.** 그리고 그 몫을 `count`에 더한다.
- `remain:` 동전으로 나누고 남은 가치, 즉 다른 동전으로 나눠야 할 금액
    - k라는 목표값을 remain 값으로 갱신해준다.
- 만약, k가 0이 되면, 즉 모든 동전을 나눈 결과가 목표 가치와 정확히 일치하면, 현재까지 count를 출력하고 메소드를 종료한다.

### 동전 가치 > k 라면

현재 동전 가치가 k보다 크므로, 해당 동전으로 k를 만들 수 없다. 그럼 위의 If문에 해당하지 않으므로 **j—;** 다음 동전으로 넘어간다.

</details>
</summary>

---

# 바이러스

<summary> 
<details>

# ☻ 바이러스

2024년 3월 11일

## 문제

신종 바이러스인 웜 바이러스는 네트워크를 통해 전파된다. 한 컴퓨터가 웜 바이러스에 걸리면 그 컴퓨터와 네트워크 상에서 연결되어 있는 모든 컴퓨터는 웜 바이러스에 걸리게 된다.

예를 들어 7대의 컴퓨터가 <그림 1>과 같이 네트워크 상에서 연결되어 있다고 하자. 1번 컴퓨터가 웜 바이러스에 걸리면 웜 바이러스는 2번과 5번 컴퓨터를 거쳐 3번과 6번 컴퓨터까지 전파되어 2, 3, 5, 6 네 대의 컴퓨터는 웜 바이러스에 걸리게 된다. 하지만 4번과 7번 컴퓨터는 1번 컴퓨터와 네트워크상에서 연결되어 있지 않기 때문에 영향을 받지 않는다.

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/4f26267c-5338-41c6-a5e8-190895f7c590)

어느 날 1번 컴퓨터가 웜 바이러스에 걸렸다. 컴퓨터의 수와 네트워크 상에서 서로 연결되어 있는 정보가 주어질 때, 1번 컴퓨터를 통해 웜 바이러스에 걸리게 되는 컴퓨터의 수를 출력하는 프로그램을 작성하시오.

## 입력

첫째 줄에는 컴퓨터의 수가 주어진다. 컴퓨터의 수는 100 이하인 양의 정수이고 각 컴퓨터에는 1번 부터 차례대로 번호가 매겨진다. 둘째 줄에는 네트워크 상에서 직접 연결되어 있는 컴퓨터 쌍의 수가 주어진다. 이어서 그 수만큼 한 줄에 한 쌍씩 네트워크 상에서 직접 연결되어 있는 컴퓨터의 번호 쌍이 주어진다.

## 출력

1번 컴퓨터가 웜 바이러스에 걸렸을 때, 1번 컴퓨터를 통해 웜 바이러스에 걸리게 되는 컴퓨터의 수를 첫째 줄에 출력한다.

## 예제 입력 1

```
7
6
1 2
2 3
1 5
5 2
5 6
4 7
```

## 예제 출력 1

```
4
```

---

## ☺︎ at

인접행렬에 저장한 후, 1번 노드를 시작으로 연결된 정점들을 탐색한다. 인접행렬의 값이 1 (연결) 되어있고, 방문하지 않은 경우 count를 증가시키며 dfs를 호출한다.

4번과 7번 정점은 연결되어 있지 않으므로 탐색되지 않는다.

---

### 더 생각할 것

- 1번과 연결되어 있지 않은 정점들은 영향을 받지 않는데, 이걸 어떻게 처리할 것인지
  - *`if* (matrix[Node][i] == 1 && !visited[i])` 해당 조건문을 만족 시킬 때만 dfs를 수행할 수 있으므로, 탐색되지 않음.
- 인접행렬/인접 리스트, 스택 어떻게 사용할 것인지
  - 인접 행렬을 사용했다. 인접 행렬은 인접 리스트보다 두 노드간의 연결 여부를 빠르게 확인해야 하는 경우에 더 효율적이다. 컴퓨터 수는 100대이고 연결 여부만 확인하면 되므로 인접 행렬 사용했다.
  - 인접 리스트는 공간 복잡도가 더 낮아 노드 수가 더 많은 그래프에 효율 적이고, 이웃 노드들을 빠르게 탐색해야 하는 경우 사용한다.

---

## ☺︎ Snippets

## 🌍 DFS

```java
public class Main {

    static boolean[] visited;
    static int[][] matrix;
    static int count;

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        //노드 수 (컴퓨터)
        int n = sc.nextInt();
        //엣지 수
        int edge = sc.nextInt();

        // 방향성 그래프 인접 행렬 할당
        matrix = new int[n + 1][n + 1];
        for (int i = 1; i <= edge; i++) {
            int v1, v2;
            v1 = sc.nextInt();
            v2 = sc.nextInt();
            matrix[v1][v2] = 1;
            matrix[v2][v1] = 1;
        }

        /**
         * @Point: 1과 연결되어 있는가 , 1과 연결된 노드들과 연결된 또 다른 노드들 개수 카운트
         * 1을 시작노드로 dfs >> 연결된 모든 노드(컴퓨터)들을 끝까지 탐색해야 하므로
         * DFS 인접 행렬 재귀로 구현
         **/
        visited = new boolean[n + 1];

        // 시작 노드
        int start = 1;
        if (dfs(start) == 0) {
            System.out.println("0");
        } else {
            System.out.print(dfs(start));
        }

    }

    // DFS 인접행렬 재귀 구현
    private static int dfs(int Node) {
        // 방문 체크
        visited[Node] = true;
        for (int i = 1; i < matrix.length; i++) {
            if (matrix[Node][i] == 1 && !visited[i]) {
                count++;
                dfs(i);
            }
        }
        if (count == 0) {
            return 0;
        }
        return count;
    }
}
```

---

## 실수했던 것

```java
 * @Point
 * count가 Null일 수도 있음
 * 인접행렬 할당할 때 Edge 기준으로 할당하지 않았다.
 * Count 출력을 print(dfs)로 해야했다.
```

### count가 Null 일 수도 있다.

시작 노드와 연결된 다른 노드가 없는 경우를 고려해야 한다. 이 경우, count를 출력하려고 하면 에러 발생한다.

### 인접행렬 할당할 때 Edge 기준으로 할당하지 않았다.

for 문의 **반복 종료 조건을 Edge(간선)**이 아니라, 노드를 기준으로 설정했어서, 자꾸 5%에서 틀렸다.

그래프의 각 노드는 행렬의 행, 열에 해당하고 각 셀은 두 노드간의 연결 상태를 나타낸다. 따라서 **각 노드 간의 연결 상태(Edge)를 기준으로 인접 행렬을 할당**해야 한다. 각 Edge가 두 노드의 연결을 나타내므로, 모든 Edge를 확인해야 그래프 연결 상태를 정확하게 파악할 수 있다.

### Count 출력을 print(dfs)로 해야했다.

### 수정 전

```java
        if (count == 0) {
            System.out.print("0");
        } else {
            System.out.print(count);
        }
```

### 수정 후

```java
        if (dfs(start) == 0) {
            System.out.println("0");
        } else {
            System.out.print(dfs(start));
        }
```

DFS 메소드 내에서 모든 노드에 대해 재귀호출을 반복하는 걸 간과했다.

시작 노드를 기준으로 if문 내에서 호출 → DFS 메소드 내에서 재귀 호출 반복 → DFS 종료 이후 if 문으로 돌아와 count 반환CancelUpdate comment

---

## ☺︎Snippets

### 🌍 BFS

```java
public class Main_BFS {
    static int n;
    static int[][] graph;
    static boolean[] visit;
    static int count;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        // 노드 수
        n = Integer.parseInt(br.readLine());
        // 엣지 수
        int edge = Integer.parseInt(br.readLine());

        // 인접 행렬 할당 그래프
        graph = new int[n + 1][n + 1];

        for (int i = 1; i <= edge; i++) {
            // 공백으로 나눠 입력
            String[] info = br.readLine().split(" ");
            int from = Integer.parseInt(info[0]);
            int to = Integer.parseInt(info[1]);
            graph[from][to] = 1;
            graph[to][from] = 1;
        }

        //방문 배열
        visit = new boolean[n + 1];

        //시작노드
        int start = 1;
        if (bfs(start) == 0) {
            System.out.print("0");
        } else {
            System.out.print(bfs(start));
        }
    }

    //BFS 큐 구현
    private static int bfs(int Node) {
        Queue<Integer> q = new LinkedList<>();
        //큐에 시작노드 삽입
        q.add(Node);
        //노드 방문 체크
        visit[Node] = true;

        while (!q.isEmpty()) {
            int nowNode = q.poll();

            // 어디까지 반복
            for (int i = 0; i < graph.length; i++) {
                //인접 행렬에서 노드와 연결되어있고
                if (graph[nowNode][i] == 1) {
                    //방문하지 않았다면
                    if (!visit[i]) {
                        //방문 체크하고
                        visit[i] = true;
                        //큐에 삽입
                        q.add(i);
                        count++;
                    }
                }
            }
        }
        if (count == 0) {
            return 0;
        }
        return count;
    }
}
```

큐가 비어있지 않은 동안, 즉 방문해야 할 노드가 남아 있는 동안 탐색을 수행한다.

1. 시작 노드를 큐에 삽입하고, 방문 표시
2. 현재 노드와, i번째 노드가 인접하고, 그리고 i 번째 노드를 방문하지 않았다면 방문 표시하고, i번째 노드를 큐에 삽입한다. 또한 방문한 노드의 수를 증가시킨다.

---

## 실수했던 것

```java
7
6
2 3
4 5
6 7
6 5
4 3
2 1
```

```java

output: 2
answer: 6
```

문제에 있는 테케는 통과하나, 반례 테케에서 내 코드는 0이 출력됨.

### 원인

```java
        graph = new int[n + 1][n + 1];

        for (int i = 1; i <= edge; i++) {
            // 공백으로 나눠 입력
            String[] info = br.readLine().split(" ");
            int from = Integer.parseInt(info[0]);
            int to = Integer.parseInt(info[1]);
            graph[from][to] = 1;
            graph[to][from] = 1;
        }
```

인접행렬 할당할 때 for loop 범위 설정을 잘못했다.

위의 반례에서는 1이 마지막 라인에 주어졌는데, For 종료 조건을 i < edge 로 설정해서 1 직전에 for loop가 종료됨 → 1 노드의 연결 노드가 할당되지 않았음.

---

## 바이러스 BFS, DFS 풀이 성능 두 배 차이나는 이유

```java
BFS 124ms
DFS 220ms
```

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/4cef23d8-d352-404d-b52d-fe232ef34df9)

## BFS가 두 배 더 빠른 이유

### 그래프

```java
  1
 / \
2   5
 \ / \
  3   6
 /
4
 \
  7
```

- BFS의 탐색 경로: 1 -> 2 -> 5 -> 3 -> 6 -> 4 -> 7
- DFS의 탐색 경로: 1 -> 2 -> 3 -> 4 -> 7 -> 5 -> 6

문제 예제의 그래프는 위와 같다.

DFS는 한 경로를 끝까지 탐색하고 다시 다른 경로로 돌아와야 한다. 이 그래프의 경우 7까지 간 이후 5로 다시 돌아와야 하는데, 이러한 특성 때문에 그래프 깊이가 깊고 경로가 긴 경우에는 DFS가 더 많은 재귀호출을 하게되어 성능이 BFS보다 떨어졌다.

반면 BFS는 한 레벨의 너비를 우선적으로 탐색한다. 따라서 위 그래프 처럼 깊고 얕은 경우에는 한 레벨을 탐색하고 넘어가므로, 다른 노드 탐색을 위해 재귀적으로 돌아올 필요가 없다. 따라서 BFS가 두 배나 더 성능이 빨랐던 것임.

---

