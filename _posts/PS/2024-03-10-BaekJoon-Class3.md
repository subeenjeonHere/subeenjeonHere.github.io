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

# ☻ 동전 0

<summary> 
<details>

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

# ☻ 바이러스

<summary> 
<details>

2024년 3월 11일

## 문제

신종 바이러스인 웜 바이러스는 네트워크를 통해 전파된다. 한 컴퓨터가 웜 바이러스에 걸리면 그 컴퓨터와 네트워크 상에서 연결되어 있는 모든 컴퓨터는 웜 바이러스에 걸리게 된다.

예를 들어 7대의 컴퓨터가 <그림 1>과 같이 네트워크 상에서 연결되어 있다고 하자. 1번 컴퓨터가 웜 바이러스에 걸리면 웜 바이러스는 2번과 5번 컴퓨터를 거쳐 3번과 6번 컴퓨터까지 전파되어 2, 3, 5, 6
네 대의 컴퓨터는 웜 바이러스에 걸리게 된다. 하지만 4번과 7번 컴퓨터는 1번 컴퓨터와 네트워크상에서 연결되어 있지 않기 때문에 영향을 받지 않는다.

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/4f26267c-5338-41c6-a5e8-190895f7c590)

어느 날 1번 컴퓨터가 웜 바이러스에 걸렸다. 컴퓨터의 수와 네트워크 상에서 서로 연결되어 있는 정보가 주어질 때, 1번 컴퓨터를 통해 웜 바이러스에 걸리게 되는 컴퓨터의 수를 출력하는 프로그램을 작성하시오.

## 입력

첫째 줄에는 컴퓨터의 수가 주어진다. 컴퓨터의 수는 100 이하인 양의 정수이고 각 컴퓨터에는 1번 부터 차례대로 번호가 매겨진다. 둘째 줄에는 네트워크 상에서 직접 연결되어 있는 컴퓨터 쌍의 수가 주어진다. 이어서
그 수만큼 한 줄에 한 쌍씩 네트워크 상에서 직접 연결되어 있는 컴퓨터의 번호 쌍이 주어진다.

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
 *@Point
 *
count가 Null일
수도 있음
 *
인접행렬 할당할
때 Edge
기준으로 할당하지
않았다 .
 *
Count 출력을

print(dfs)로 해야했다.
```

### count가 Null 일 수도 있다.

시작 노드와 연결된 다른 노드가 없는 경우를 고려해야 한다. 이 경우, count를 출력하려고 하면 에러 발생한다.

### 인접행렬 할당할 때 Edge 기준으로 할당하지 않았다.

for 문의 **반복 종료 조건을 Edge(간선)**이 아니라, 노드를 기준으로 설정했어서, 자꾸 5%에서 틀렸다.

그래프의 각 노드는 행렬의 행, 열에 해당하고 각 셀은 두 노드간의 연결 상태를 나타낸다. 따라서 **각 노드 간의 연결 상태(Edge)를 기준으로 인접 행렬을 할당**해야 한다. 각 Edge가 두 노드의 연결을
나타내므로, 모든 Edge를 확인해야 그래프 연결 상태를 정확하게 파악할 수 있다.

### Count 출력을 print(dfs)로 해야했다.

### 수정 전

```java
        if(count ==0){
        System.out.

print("0");
        }else{
                System.out.

print(count);
        }
```

### 수정 후

```java
        if(dfs(start) ==0){
        System.out.

println("0");
        }else{
                System.out.

print(dfs(start));
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

output:2
answer:6
```

문제에 있는 테케는 통과하나, 반례 테케에서 내 코드는 0이 출력됨.

### 원인

```java
        graph =new int[n +1][n +1];

        for(
int i = 1;
i <=edge;i++){
// 공백으로 나눠 입력
String[] info = br.readLine().split(" ");
int from = Integer.parseInt(info[0]);
int to = Integer.parseInt(info[1]);
graph[from][to]=1;
graph[to][from]=1;
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
        2 5
        \ / \
        3 6
        /
        4
        \
        7
```

- BFS의 탐색 경로: 1 -> 2 -> 5 -> 3 -> 6 -> 4 -> 7
- DFS의 탐색 경로: 1 -> 2 -> 3 -> 4 -> 7 -> 5 -> 6

문제 예제의 그래프는 위와 같다.

DFS는 한 경로를 끝까지 탐색하고 다시 다른 경로로 돌아와야 한다. 이 그래프의 경우 7까지 간 이후 5로 다시 돌아와야 하는데, 이러한 특성 때문에 그래프 깊이가 깊고 경로가 긴 경우에는 DFS가 더 많은
재귀호출을 하게되어 성능이 BFS보다 떨어졌다.

반면 BFS는 한 레벨의 너비를 우선적으로 탐색한다. 따라서 위 그래프 처럼 깊고 얕은 경우에는 한 레벨을 탐색하고 넘어가므로, 다른 노드 탐색을 위해 재귀적으로 돌아올 필요가 없다. 따라서 BFS가 두 배나 더
성능이 빨랐던 것임.

</details>
</summary>

---


# ☻ DFS와 BFS

<summary>

<details>

2024년 3월 11일

## 문제

그래프를 DFS로 탐색한 결과와 BFS로 탐색한 결과를 출력하는 프로그램을 작성하시오. 단, 방문할 수 있는 정점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문하고, 더 이상 방문할 수 있는 점이 없는 경우 종료한다. 정점 번호는 1번부터 N번까지이다.

## 입력

첫째 줄에 정점의 개수 N(1 ≤ N ≤ 1,000), 간선의 개수 M(1 ≤ M ≤ 10,000), 탐색을 시작할 정점의 번호 V가 주어진다. 다음 M개의 줄에는 간선이 연결하는 두 정점의 번호가 주어진다. 어떤 두 정점 사이에 여러 개의 간선이 있을 수 있다. 입력으로 주어지는 간선은 양방향이다.

## 출력

첫째 줄에 DFS를 수행한 결과를, 그 다음 줄에는 BFS를 수행한 결과를 출력한다. V부터 방문된 점을 순서대로 출력하면 된다.

## 예제 입력 1

```
4 5 1
1 2
1 3
1 4
2 4
3 4
```

## 예제 출력 1

```
1 2 4 3
1 2 3 4

```

## 예제 입력 2

```
5 5 3
5 4
5 2
1 2
3 4
3 1
```

## 예제 출력 2

```
3 1 2 5 4
3 1 4 2 5
```

## 예제 입력 3

```
1000 1 1000
999 1000

```

## 예제 출력 3

```
1000 999
1000 999
```

---

## Snippets

```java

public class Main {
    static int[][] arr;
    static boolean[] visit;

    static void dfs(int Node) {
        //방문 기록
        visit[Node] = true;
        // 방문한 노드 출력
        System.out.print(Node + " ");
        //스택 자료구조 할당
        Stack<Integer> stk = new Stack<>();
        stk.push(Node);
        //스택 빌 때까지 반복
        while (!stk.isEmpty()) {
            int nowNode = stk.pop();
            for (int i = 0; i < arr.length; i++) {
                if (visit[i]) {
                    continue;
                }
                if (arr[nowNode][i] == 1 && !visit[i]) {
                    dfs(i);
                }
            }
        }
        //dfs 재귀호출
    }

    static void bfs(int Node) {
        visit[Node] = true;
        //큐로 구현
        Queue<Integer> que = new LinkedList<>();

        //시작 노드 큐 삽입
        que.add(Node);

        //큐가 빌 때까지 현재 노드 poll 하고 방문
        while (!que.isEmpty()) {
            //일단 큐에서 시작노드 poll 하고, 시작노드와 인접한 노드 삽입

            int nowNode = que.poll();
            System.out.print(nowNode + " ");
            for (int i = 0; i < arr.length; i++) {
                //인접 행렬 1이고, 방문 안 했다면 큐 삽입
                if (arr[nowNode][i] == 1 && !visit[i]) {
                    que.add(i);
                    // 인접 노드 방문 체크
                    visit[i] = true;

                }
            }
        }

    }

    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();

        //정점개수 n, 간선 개수 m, 시작 정점 번호 v 입력받기
        String[] info = br.readLine().split(" ");
        int n = Integer.parseInt(info[0]);
        int m = Integer.parseInt(info[1]);
        int vtx = Integer.parseInt(info[2]);

        //인접행렬 할당
        arr = new int[n + 1][n + 1];
        visit = new boolean[n + 1];

        //간선 연결 하기 간선개수만큼 반복
        for (int i = 0; i < m; i++) {
            String[] ab = br.readLine().split(" ");
            int from = Integer.parseInt(ab[0]);
            int to = Integer.parseInt(ab[1]);
            arr[from][to] = 1;
            arr[to][from] = 1;
        }
        //시작노드
        //dfs 수행
        dfs(vtx);
        visit = new boolean[n + 1];
        System.out.println();
        //bfs 수행
        bfs(vtx);

        //결과 출력하기
    }
}
```

---

## 실수한 것

```java
 * @Subject DFS, BFS 기본 문제
 * @Point 
* 탐색 결과 어떻게 출력
 * dfs 스택 무한루프, 다 탐색했을 때 어떻게 종료 -> 스택 Pop 해야했음
 * 방문배열 초기화
```

### DFS 스택 무한루프

스택에서 pop이 아니라 peek을 해서 무한루프에 빠짐.

### 왜

스택에서 원소를 제거하지 않으면 기**존에 방문한 노드를 계속 참조**하기 때문에 무한루프에 빠졌었다.

| 메소드 | 내용 |
| --- | --- |
| Pop | 스택의 최상단에 위치한 요소를 빼내는 작업 |
| peek | 스택의 최상단에 위치한 요소를 조회 |

peek만 수행하고 pop을 안 하면, 계속 같은 노드를 조회하게 된다. 스택을 왜 내가 사용했냐 백트래킹 위해선데, 현재 정점에서 모든 경로 탐색하다가 **이전 정점으로 돌아가서 다른 경로 탐색해야 한다.**

pop을 안 해주니까, 스택에서 최상단 노드가 삭제가 안 되고 계속 그 노드를 방문했다.

### 방문 배열 초기화

DFS 수행 이후, BFS 호출할 때 **방문 배열 초기화를 안 했다.** 각각 별도의 방문 배열을 사용하거나, 한 탐색이 끝나고 난 후에는 **방문 배열을 초기화** 해주어야 한다.

</details>
</summary>

---

# ☻ 유기농 배추

<summary>
<details>

2024년 3월 11일 2024년 3월 12일 2024년 3월 13일 2024년 3월 15일

## 문제

차세대 영농인 한나는 강원도 고랭지에서 유기농 배추를 재배하기로 하였다. 농약을 쓰지 않고 배추를 재배하려면 배추를 해충으로부터 보호하는 것이 중요하기 때문에, 한나는 해충 방지에 효과적인 배추흰지렁이를 구입하기로 결심한다. 이 지렁이는 배추근처에 서식하며 해충을 잡아 먹음으로써 배추를 보호한다. 특히, 어떤 배추에 배추흰지렁이가 한 마리라도 살고 있으면 이 지렁이는 인접한 다른 배추로 이동할 수 있어, 그 배추들 역시 해충으로부터 보호받을 수 있다. 한 배추의 상하좌우 네 방향에 다른 배추가 위치한 경우에 서로 인접해있는 것이다.

한나가 배추를 재배하는 땅은 고르지 못해서 배추를 군데군데 심어 놓았다. 배추들이 모여있는 곳에는 배추흰지렁이가 한 마리만 있으면 되므로 서로 인접해있는 배추들이 몇 군데에 퍼져있는지 조사하면 총 몇 마리의 지렁이가 필요한지 알 수 있다. 예를 들어 배추밭이 아래와 같이 구성되어 있으면 최소 5마리의 배추흰지렁이가 필요하다. 0은 배추가 심어져 있지 않은 땅이고, 1은 배추가 심어져 있는 땅을 나타낸다.

| 1 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 | 0 | 0 | 0 | 1 | 1 | 1 |
| 0 | 0 | 0 | 0 | 1 | 0 | 0 | 1 | 1 | 1 |

## 입력

입력의 첫 줄에는 테스트 케이스의 개수 T가 주어진다. 그 다음 줄부터 각각의 테스트 케이스에 대해 첫째 줄에는 배추를 심은 배추밭의 가로길이 M(1 ≤ M ≤ 50)과 세로길이 N(1 ≤ N ≤ 50), 그리고 배추가 심어져 있는 위치의 개수 K(1 ≤ K ≤ 2500)이 주어진다. 그 다음 K줄에는 배추의 위치 X(0 ≤ X ≤ M-1), Y(0 ≤ Y ≤ N-1)가 주어진다. 두 배추의 위치가 같은 경우는 없다.

## 출력

각 테스트 케이스에 대해 필요한 최소의 배추흰지렁이 마리 수를 출력한다.

## 예제 입력 1

```
2
10 8 17
0 0
1 0
1 1
4 2
4 3
4 5
2 4
3 4
7 4
8 4
9 4
7 5
8 5
9 5
7 6
8 6
9 6
10 10 1
5 5
```

## 예제 출력 1

```
5
1
```

## 예제 입력 2

```
1
5 3 6
0 2
1 2
2 2
3 2
4 2
4 0
```

## 예제 출력 2

```
2
```

---

## ☺︎ a/t

### Input

- 가로 길이 M, 세로 길이 N
  - 인접 행렬 그래프의 크기
- K
  - 배추가 심어져있는 위치의 개수 = 배추의 개수 = 정점
  - 두 배추의 위치가 같은 경우는 없다.

---

## 목표

- 필요한 지렁이 개수 구하기

---

## 아이디어

- 인접행렬을 사용해, 4방 탐색으로 인접한 노드들을 탐색한다. 각 노드에 대해 4방 탐색을 통해 1이라면 (배추가 심어져 있다면) → 탐색 진행 → 4방 탐색으로 연결된 노드가 없을 때 까지 탐색한다. 더 이상 노드가 없다면 종료한다.
- 지렁이는 상하좌우에 인접한 배추가 있다면 이동할 수 있다. 즉, 인접한 배추들이 있다면 인접된 배추들을 따라가면서, 더 이상 인접한 배추들이 없을 때 까지 똑같은 1개의 지렁이를 사용할 수 있다.
- DFS, BFS를 사용한다. 더 이상 인접한 노드(배추)들이 없을 때 까지 탐색하는 것이다.

---

## Snippets

## DFS

```java
public class Main {
    static int n, m;
    static int[][] g;
    static boolean[][] visited;
    static int[] node;
    
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int tc = Integer.parseInt(br.readLine());

        for (int t = 1; t <= tc; t++) {
            String[] info = br.readLine().split(" ");
            m = Integer.parseInt(info[0]);
            n = Integer.parseInt(info[1]);
            int k = Integer.parseInt(info[2]);

            g = new int[51][51];

            //노드 저장
            node = new int[51];
            // 인접 행렬 할당
            for (int i = 1; i <= k; i++) {
                String[] ab = br.readLine().split(" ");
                int x = Integer.parseInt(ab[0]);
                int y = Integer.parseInt(ab[1]);
                g[x][y] = 1;
            }

            // 방문 배열 할당
            visited = new boolean[51][51];
            int result = 0;
            //시작할 노드 찾기 (0,0)
            // 1이 있으면 dfs Call
            for (int i = 0; i < 51; i++) {
                for (int j = 0; j < 51; j++) {
                    if (g[i][j] == 1 && !visited[i][j]) {
                        dfs(i, j, g, visited);
                        result++;
                    }
                }
            }
            System.out.println(result);
        }
    }
    
    //DFS 동작
    private static void dfs(int x, int y, int[][] g, boolean[][] visited) {
        //방문 체크
        visited[x][y] = true;
        // 상하 좌우
        int[] dx = {-1, 1, 0, 0};
        int[] dy = {0, 0, -1, 1};

        // 4방 탐색하며 인접 노드 탐색
        for (int d = 0; d < 4; d++) {
            int nx = x + dx[d];
            int ny = y + dy[d];

            if (nx < 0 || ny < 0 || x > g.length || y > g[0].length) {
                continue;
            }

            if (g[nx][ny] == 0) {
                continue;
            }
            // 방문하지 않았고, 노드 있으면 방문체크, dfs recursive call
            if (!visited[nx][ny] && g[nx][ny] == 1) {
                visited[nx][ny] = true;
                dfs(nx, ny, g, visited);
            }
        }
    }
}
```

---

## 시간 많이 소요했던 것

### 델타 4방 탐색

[델타 4방 탐색 (Delta 4-way Search)](https://subeenjeonhere.github.io/algorithms/델타-4방-탐색(Delta4-way-Search)/)

### 인접행렬 길이 할당

### DFS 호출 시 `!visited[i][j]` 조건문에 설정해주지 않은 것

```java
for (int i = 0; i < 51; i++) {
	for (int j = 0; j < 51; j++) {
		if (g[i][j] == 1 && !visited[i][j]) {
           dfs(i, j, g, visited);
	          result++;
                    }
                }
            }
			System.out.println(result);
```

DFS 메소드 내에서 visit 배열 체크해주면 된다고 생각해서 단지를 탐색하는 Main 메소드 내에서는 visit 배열 체크를 고려하지 않았음.

메인 메소드 내에서도 방문 배열 체크하고 방문했다면, 해당 노드를 탐색하지 않아야 함. 저 코드는 단지 개수를 찾기위해 (= 시작 지점을 찾기 위한) 코드임.

만약 DFS 메소드 내에서 인접 배열로서 인접 노드로 방문 체크가 되었고, 단지 탐색 이후 다시 시작 노드를 찾을 때 그 노드가 호출될 것임. → **노드는 1이기에 배추가 심어져있긴 하나, 인접 노드로서 이미 방문을 했기에 방문할 필요가 없음.**

즉 **한번 탐색에 사용된 노드가 다른 시작점의 탐색에 영향을 주지 않도록 하기 위해, 메인 메소드 내에서도 방문 체크**를 해주어야 했음.

---

## BFS

```java
/**
 * @Point 왜 그래프 n+2, m+2로 해줘야 하는지
 * 큐 BFS 4방탐색
 * 예제 2에서 4,0 탐색 안 됨
 * 4방탐색 continue 조건 설정
 */
public class Main_BFS {
    // 가로 m, 세로 n, 노드 k
    static int m, n, k;
    static int[][] graph;
    static boolean[][] visit;
    static int count;

    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        //테케
        int tc = Integer.parseInt(br.readLine());
        //가로길이, 세로길이, 노드
        for (int i = 1; i <= tc; i++) {

            String[] info = br.readLine().split(" ");
            m = Integer.parseInt(info[0]);
            n = Integer.parseInt(info[1]);
            k = Integer.parseInt(info[2]);

            // 인접 행렬 할당
            graph = new int[51][51];
            visit = new boolean[51][51];

            //인접 행렬 할당 2
            for (int j = 1; j <= k; j++) {
                String[] ab = br.readLine().split(" ");
                int x = Integer.parseInt(ab[0]);
                int y = Integer.parseInt(ab[1]);
                graph[x][y] = 1;
            }
            for (int x = 0; x < graph.length; x++) {
                for (int y = 0; y < graph[x].length; y++) {
                    if (graph[x][y] == 1 && !visit[x][y]) {
                        count++;
                        bfs(x, y, visit, graph);
                    }
                }
            }
            System.out.println(count);
            count = 0;
        }
    }

    //BFS 시작
    private static void bfs(int x, int y, boolean[][] visit, int[][] graph) {

        //델타 4방 탐색
        int[] dx = {-1, 1, 0, 0};
        int[] dy = {0, 0, -1, 1};
        //큐 배열 선언
        Queue<int[]> queue = new LinkedList<>();
        //큐에 시작노드 좌표 삽입
        queue.add(new int[]{x, y});

        //방문 체크
        visit[x][y] = true;
        //BFS 반복
        while (!queue.isEmpty()) {

            //좌표배열 있어야 함
            int[] current = queue.poll();
            int nowX = current[0];
            int nowY = current[1];

            //4방 탐색 인접 행렬
            for (int d = 0; d < 4; d++) {
                int nx = nowX + dx[d];
                int ny = nowY + dy[d];

                if (nx < 0 || ny < 0 || nx >= graph.length || ny >= graph[0].length) {
                    continue;
                }
                // 큐에 방문하지 않았고, 4방 탐색 좌표가 1이라면
                if (graph[nx][ny] == 1 && !visit[nx][ny]) {
                    //방문 처리
                    visit[nx][ny] = true;
                    queue.add(new int[]{nx, ny});
                }
            }
        }
    }
}

```

---

## 시간 많이 소요한 것

### 4방탐색 continue 조건 설정

수정 전에도 정답엔 문제가 없었는데, 뇌에 외워진대로 썼던 것 같아서 다시 정리하면

```java
if (nx < 0 || ny < 0 || nx >= graph.length || ny >= graph[0].length) {
    continue;
}
```

여기서 nx, ny는 탐색할 좌표의 위치이다.

4방탐색을 할 땐 배열의 경계를 넘어서지 않도록 해주어야 한다.

- `nx < 0 || ny < 0`
  - 탐색할 위치가 배열의 왼쪽, 위쪽 경계를 넘어섰는지 확인
- `nx >= graph.length || ny >= graph[0].length`
  - 탐색할 위치가 배열 오른쪽, 아래쪽 경계를 넘어갔는지 확인

---

### BFS 큐 자료구조 할당

좌표를 사용해야 하고, BFS 큐를 돌릴 때 큐 자료구조 할당 어떻게 했는지

```java
//큐 배열 선언
Queue<int[]> queue = new LinkedList<>();
//큐에 시작노드 좌표 삽입
queue.add(new int[]{x, y});
```

- 좌표를 사용해야 했기에 두 개 이상의 값을 한 번에 저장하고 싶었다. 예를 들어, 2차원 배열에서의 위치를 나타내는 (x, y)좌표를 저장하고 싶을 때, `Queue<int[]>`를 사용하면 int형 배열을 하나의 원소로 저장할 수 있다.
- `new int[]{x, y}`
  - int 타입의 배열을 새로 생성하고, 그 배열에 x와 y라는 두 개의 값을 저장하겠다는 의미. 왜냐면 큐는 한 번에 하나의 원소만 추가할 수 있는데, x와 y 두 개의 값을 한 번에 큐에 추가하려면 이 두 값을 하나의 단위로 묶어야 했다.

```java
// 원래
Queue<int[]> queue = new LinkedList<>();
```

반면, 위는 하나의 정수값 만을 저장한다.

따라서, 저장해야 하는 정보가 하나의 정수일 경우에는 `Queue<Integer>`를 사용하고, 두 개 이상의 연관된 값을 한 번에 저장하고 싶을 때는 `Queue<int[]>`를 사용한다.

---

## BFS/DFS 성능 차이

```java
BFS 180ms
DFS 180ms
```

### 바이러스

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

### 유기농 배추

```java
    0
    |
    1
    |
    2 -- 3 -- 4
    |
    5
```

"바이러스" 문제의 경우, 트리가 깊고 얕은 경우에 한 레벨을 탐색하고 넘어가므로, DFS처럼 2 →..→ 7 까지 갔다가 재귀호출해서 돌아올 필요가 없었다. 따라서 BFS가 훨씬 빨랐다.

유기농 배추에선 트리가 연결 리스트와 같이 가지가 적다. 이 경우 DFS, BFS 모두 거의 모든 노드 순회해야 하므로 성능 비슷했다.

</details>
</summary>

---

# ☻ 미로탐색

<summary>

<details>

2024년 3월 16일 2024년 3월 17일

## 문제

N×M크기의 배열로 표현되는 미로가 있다.

| 1 | 0 | 1 | 1 | 1 | 1 |
| --- | --- | --- | --- | --- | --- |
| 1 | 0 | 1 | 0 | 1 | 0 |
| 1 | 0 | 1 | 0 | 1 | 1 |
| 1 | 1 | 1 | 0 | 1 | 1 |

미로에서 1은 이동할 수 있는 칸을 나타내고, 0은 이동할 수 없는 칸을 나타낸다. 이러한 미로가 주어졌을 때, (1, 1)에서 출발하여 (N, M)의 위치로 이동할 때 지나야 하는 최소의 칸 수를 구하는 프로그램을 작성하시오. 한 칸에서 다른 칸으로 이동할 때, 서로 인접한 칸으로만 이동할 수 있다.

위의 예에서는 15칸을 지나야 (N, M)의 위치로 이동할 수 있다. 칸을 셀 때에는 시작 위치와 도착 위치도 포함한다.

## 입력

첫째 줄에 두 정수 N, M(2 ≤ N, M ≤ 100)이 주어진다. 다음 N개의 줄에는 M개의 정수로 미로가 주어진다. 각각의 수들은 **붙어서** 입력으로 주어진다.

## 출력

첫째 줄에 지나야 하는 최소의 칸 수를 출력한다. 항상 도착위치로 이동할 수 있는 경우만 입력으로 주어진다.

## 예제 입력 1

```
4 6
101111
101010
101011
111011
```

## 예제 출력 1

```
15
```

## 예제 입력 2

```
4 6
110110
110110
111111
111101
```

## 예제 출력 2

```
9
```

## 예제 입력 3

```
2 25
1011101110111011101110111
1110111011101110111011101

```

## 예제 출력 3

```
38
```

## 예제 입력 4

```
7 7
1011111
1110001
1000001
1000001
1000001
1000001
1111111
```

## 예제 출력 4

```
13
```

---

## ☺︎ a/t

- DFS 사용
  - 깊게 탐색해야 하므로, DFS 사용
    - BFS 사용해야 했다.
- 자료구조 뭐 사용할 것인지? 입력과 동시에 인접 행렬 저장 vs 인접 리스트 사용
  - 인접리스트 사용
    - 노드 정보를 알아야 한다. → 좌표를 노드로 사용 가능
  - 인접 행렬 사용
    - 입력과 동시에 인접 행렬 할당한다.
- BFS로 최단거리 어떻게 찾을 지
  - **Distance**만 계산해주면 된다.

---

## ☺︎ Snippets

```java
/***
 * @Point
 * 인접노드 여러개일 때 한 개의 최단경로
 * bfs가 최단거리를 보장하는 원리 생각해보기
 */
public class Main2 {
    //방문 배열
    static boolean[][] visit;
    //인접 행렬 배열
    static int[][] g;
    //노드 탐색 카운트 int
    static int count;
    //세로,가로 길이
    static int n, m;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        //N,M 입력 받기
        String[] nm = br.readLine().split(" ");
        n = Integer.parseInt(nm[0]);
        m = Integer.parseInt(nm[1]);

        //인접행렬 메모리 할당
        g = new int[n][m];
        //방문 배열 할당
        visit = new boolean[n][m];

        //인접행렬 입력받기
        for (int i = 0; i < n; i++) {
            String[] node = br.readLine().split("");
            for (int j = 0; j < m; j++) {
                g[i][j] = Integer.parseInt(node[j]);
            }
        }
        System.out.print(bfs(0, 0) + 1);
    }

    private static int bfs(int x, int y) {
        //델타 배열
        int[] dx = {1, -1, 0, 0};
        int[] dy = {0, 0, -1, 1};

        //큐 할당
        Queue<Point> q = new LinkedList<>();

        //큐 시작 위치, 거리 삽입
        q.add(new Point(0, 0, 0));

        //방문 체크
        visit[0][0] = true;

        while (!q.isEmpty()) {
            Point now = q.poll();

            int nowX = now.getX();
            int nowY = now.getY();
            int dis = now.getDis();
            if (nowX == n - 1 && nowY == m - 1) {
                return dis;
            }

            for (int d = 0; d < 4; d++) {
                int newX = nowX + dx[d];
                int newY = nowY + dy[d];

                if (newX < 0 || newY < 0 || newX >= n || newY >= m) {
                    continue;
                }
                // 인접 노드 1이고, 방문하지 않았다면 방문체크 큐 삽입 거리 +1 갱신
                if (g[newX][newY] == 1 && !visit[newX][newY]) {
                    visit[newX][newY] = true;
                    q.add(new Point(newX, newY, dis + 1));
                }
            }
        }
        
        return -1;
    }

    //데이터 쌍 저장할 Pair 클래스
    public static class Point {
        private final int x, y, dis;

        public Point(int x, int y, int dis) {
            this.x = x;
            this.y = y;
            this.dis = dis;
        }

        public int getX() {
            return x;
        }

        public int getY() {
            return y;
        }

        public int getDis() {
            return dis;
        }
    }
}
// 인접 노드 여러 개 일 때 한 꺼번에 큐에 삽입?
// 인접행렬이 아니라 인접된 노드를 for 돌려야 하는건지 그럼 4방 탐색은 어떻게 할건지
// (0, 3) -> (0,4) 탐색못하고 continue됨 = nx,ny 길이
// bfS 종료 시점 언제
// 거리 출력 void -> int, 리턴값
```

---

## 시간 소요한 것

## BFS랑 최단 경로

### 1. 왜 BFS로 최단 경로를 찾을 수 있는건지

BFS는 시작 노드에서 거리가 1인 모든 노드를 탐색한다. 즉 부모 노드에서 인접한 (=거리가 1인, 인접 노드끼리는 같은 레벨) 노드를 모두 탐색한다. 이게 DFS랑 달리 **BFS는 레벨 단위**로 탐색하기 때문에 가능하다.

### 2. 모든 탐색 경로를 탐색하여 목표 지점까지 도달하고 나서야 어떤 루트가 최단거리인지 알 수 있는 것 아닌가?

최단 경로를 찾는 과정에서 BFS는 시작 정점에서 가까운 정점들부터 탐색한다. → 이 특성 때문에 목표 지점에 처음 도달했을 때 경로가 최단 경로가 된다.

### 3. 그럼 부모 노드를 저장해두고, 경로를 다 탐색한 이후 다시 또 돌아와서 탐색해도 되는 것 아닌가

BFS가 왜 최단 경로를 보장하는지 이해가 되지않아 생각했던 방법 중 하나이다. 당연히 시간 복잡도 면에서 효율성 떨어질 것이며 최악의 경우 지수 시간복잡도를 갖을 것이다.

### 4. BFS가 가장 거리가 짧은 노드를 선택하여 이동한다는 근거

이웃 정점들을 큐에 추가한다. 항상 현재 정점에서 거리가 1인 인접 정점들을 탐색하고, 이후에 거리가 2인 정점들을 방문한다.

### 5. 예를 들어 0과 연결된 노드가 1, 2일 때 1, 2 어떤 노드로 움직여야 최단 경로인지 어떻게 알 수 있는건가

0과 연결된 노드 중에서도 먼저 들어온 노드를 방문한다. 그럼 하나 더 궁금한게 생긴다. 탐색은 내가 델타 배열로 하는데, 큐에 삽입되는 순서는?

### 6. 델타 배열로 탐색할 때, 거리가 긴 노드가 큐에 먼저 삽입된다면

이해된다. 다음 레벨로 이동할 때 마다 거리가 1씩 증가한다. 각 레벨에서 모든 노드를 방문하면서 거리를 1씩 증가시키므로 최종적으로 도달한 노드까지의 거리는 최단 거리가 된다.

---

## 거리 +1

또한, 큐에 노드를 삽입할 때 마다 거리를 1씩 증가 시켜주었다. 그럼 도달할 때 까지 모든 노드를 탐색하는데. 이때 마다 거리가 1씩 증가하는 것 아닌가?

```java
// 인접 노드 1이고, 방문하지 않았다면 방문체크 큐 삽입 거리 +1 갱신
if (g[newX][newY] == 1 && !visit[newX][newY]) {
    visit[newX][newY] = true;
    q.add(new Point(newX, newY, dis + 1));
}
```

---

## ☺︎ Point 클래스

각 노드 간의 **거리를 저장**하기 위해 Point 클래스 생성했다.

### 메모리 사용

Point 객체는 메모리에 저장되고, 객체마다 변수 x, y, dis가 메모리에 올라감. BFS 탐색할 때마다 새로운 Point 객체 생성되어 큐에 저장되고, 큐 제거 되면서 메모리 사용 발생한다.

### 작동 원리

시작 노드부터, 인접 노드까지 Point 객체 생성하여 큐에 저장했다. 그리고, dis + 1 현재 노드의 거리에 +1을 해줬다. 인접노드라서 각 레벨간의 거리는 1이기 때문에.

각 노드간의 거리를 구하기 위해서, Point 클래스 생성했는데 이게 노드를 탐색할 때마다 객체 생성하면서 메모리 사용하기 때문에 비효율적, 수행시간 저하 요인인 듯 하다.

```java
            Point now = q.poll();

            int nowX = now.getX();
            int nowY = now.getY();
            int dis = now.getDis();
//생략
	
					q.add(new Point(newX, newY, dis + 1));
```

```java
    public static class Point {
        private final int x, y, dis;
        public Point(int x, int y, int dis) {
            this.x = x;
            this.y = y;
            this.dis = dis;
        }

        public int getX() {
            return x;
        }

        public int getY() {
            return y;
        }

        public int getDis() {
            return dis;
        }
    }
```

---

## ☺︎ 다른 풀이

### 성능 차이

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/bc1360b2-afdd-4b70-821b-8ab41885010b)

Point 클래스 생성하지 않고 거리 저장하는 풀이로 수행시간 단축

```java
    private static int bfs(int x, int y) {
        Queue<int[]> q = new LinkedList<>();
        //시작 노드 삽입
        q.offer(new int[]{x, y});
        //방문 체크
        visit[x][y] = true;

        while (!q.isEmpty()) {
            int[] nowNode = q.poll();

            //4방탐색
            for (int d = 0; d < 4; d++) {
                int newX = nowNode[0] + dx[d];
                int newY = nowNode[1] + dy[d];

                //4방탐색 탐색조건
                if (newX < 0 || newY < 0 || newX >= n || newY > m) {
                    continue;
                }
                if (graph[newX][newY] == 1 && !visit[newX][newY]) {
                    visit[newX][newY] = true;
                    // 중요 최단경로
                    graph[newX][newY] = graph[nowNode[0]][nowNode[1]] + 1;
                    q.add(new int[]{newX, newY});
                }
            }
        }
        return -1;
    }
```

### 이유

1. Point 객체 생성하지 않고, 인접 노드에 부모 노드 거리 +1 해주어 갱신함
2. 목표 노드까지 최단 경로 누적되어 저장됨

</details>
</summary>

---

# ☻ 토마토

<summary>

<details>

2024년 3월 17일

## 문제

철수의 토마토 농장에서는 토마토를 보관하는 큰 창고를 가지고 있다. 토마토는 아래의 그림과 같이 격자 모양 상자의 칸에 하나씩 넣어서 창고에 보관한다.

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/4d6167d5-75d0-4095-be57-a2240717d169)

창고에 보관되는 토마토들 중에는 잘 익은 것도 있지만, 아직 익지 않은 토마토들도 있을 수 있다. 보관 후 하루가 지나면, 익은 토마토들의 인접한 곳에 있는 익지 않은 토마토들은 익은 토마토의 영향을 받아 익게 된다. 하나의 토마토의 인접한 곳은 왼쪽, 오른쪽, 앞, 뒤 네 방향에 있는 토마토를 의미한다. 대각선 방향에 있는 토마토들에게는 영향을 주지 못하며, 토마토가 혼자 저절로 익는 경우는 없다고 가정한다. 철수는 창고에 보관된 토마토들이 며칠이 지나면 다 익게 되는지, 그 최소 일수를 알고 싶어 한다.

토마토를 창고에 보관하는 격자모양의 상자들의 크기와 익은 토마토들과 익지 않은 토마토들의 정보가 주어졌을 때, 며칠이 지나면 토마토들이 모두 익는지, 그 최소 일수를 구하는 프로그램을 작성하라. 단, 상자의 일부 칸에는 토마토가 들어있지 않을 수도 있다.

## 입력

첫 줄에는 상자의 크기를 나타내는 두 정수 M,N이 주어진다. M은 상자의 가로 칸의 수, N은 상자의 세로 칸의 수를 나타낸다. 단, 2 ≤ M,N ≤ 1,000 이다. 둘째 줄부터는 하나의 상자에 저장된 토마토들의 정보가 주어진다. 즉, 둘째 줄부터 N개의 줄에는 상자에 담긴 토마토의 정보가 주어진다. 하나의 줄에는 상자 가로줄에 들어있는 토마토의 상태가 M개의 정수로 주어진다. 정수 1은 익은 토마토, 정수 0은 익지 않은 토마토, 정수 -1은 토마토가 들어있지 않은 칸을 나타낸다.

토마토가 하나 이상 있는 경우만 입력으로 주어진다.

## 출력

여러분은 토마토가 모두 익을 때까지의 최소 날짜를 출력해야 한다. 만약, 저장될 때부터 모든 토마토가 익어있는 상태이면 0을 출력해야 하고, 토마토가 모두 익지는 못하는 상황이면 -1을 출력해야 한다.

## 예제 입력 1

```
6 4
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 1
```

## 예제 출력 1

```
8
```

## 예제 입력 2

```
6 4
0 -1 0 0 0 0
-1 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 1
```

## 예제 출력 2

```
-1
```

## 예제 입력 3

```
6 4
1 -1 0 0 0 0
0 -1 0 0 0 0
0 0 0 0 -1 0
0 0 0 0 -1 1
```

## 예제 출력 3

```
6
```

## 예제 입력 4

```
5 5
-1 1 0 0 0
0 -1 -1 -1 0
0 -1 -1 -1 0
0 -1 -1 -1 0
0 0 0 0 0
```

## 예제 출력 4

```
14
```

## 예제 입력 5

```
2 2
1 -1
-1 1
```

## 예제 출력 5

```
0
```

---

## ☺︎ a/t

### BFS 너비우선 탐색

1. 인접 노드를 전체 탐색하면 상하좌우 토마토가 익는데에 1일이 걸린다.
2. 그럼 1레벨을 탐색할 때마다 1일을 더해준다.

### 익지 않은 토마토 개수

1. 닿을 수 없는 칸이다. 이는 상하좌우가 -1로 둘러쌓여있는 경우이다.
2. 인접 행렬에 0인 토마토의 개수를 누적한다. 그리고 BFS 내에서 노드를 방문할 때마다 개수를 감소시킨다.
3. BFS 종료 후 0의 개수가 0이 되었다면 전체 탐색 완료했으므로, 날짜를 리턴한다.
4. 0의 개수가 > 0 이라면 남은 토마토가 있으므로, -1을 리턴한다.
5. 인접 행렬에 입력받을 때 0이 없다면, 바로 0을 출력한다.

---

## 시간 소요한 것

### 1. 탐색 알고리즘 어떤 걸 사용할 것인지

감으로 탐색 알고리즘을 풀어야 겠다고 생각했는데, BFS, DFS 탐색 알고리즘을 사용할 것인가? → 최소 일수를 구하라고 했으므로 BFS다. 시작 정점에서 종료 노드 까지의 최단 거리를 구하면 된다.

### 2. Depth를 어떻게 구할 것인지, 또 한 레벨 탐색 완료 시 +1일 이라는 접근은 적절했는지

우선 시작 정점은 이미 익은 토마토고, 상하좌우 탐색을 통해 인접 노드를 탐색한다.

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/ea8511a1-9002-458c-8c52-883847cb9506)

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/3c3d6290-4099-4ffc-9df1-909aadc9546b)


### 3. 1이 여러개 일 때 시작 정점 설정

내가 간과했던 부분이다.

For Loop에서 1을 찾으면 이걸 정점으로 바로 BFS를 호출했다. 1인 토마토는 여러개일 수도 있다. 따라서 다른 1인 토마토 쪽에서도 익힐 수 있는데, 최단 거리가 더 많이 출력됐다.

### 3.1 시작 정점 여러개일 때 해결법

우선 각각의 정점에서 **동시에 BFS를 시작해야 한다.** 이를 위해, BFS를 시작하기 전에 모든 시작 정점을 큐에 미리 넣는다.

동시에 시작 정점을 큐에 미리 넣고, 또 동시에 BFS를 수행해야 한다.

→ 이미 방문한 노드를 다시 방문하지 않아야 하기에. 또 각 탐색이 독립적으로 수행되면 안 된다.

</details>
</summary>

---



# ☻ 연결 요소의 개수

<summary>

<details>

2024년 3월 23일 2024년 3월 24일

## 문제

방향 없는 그래프가 주어졌을 때, 연결 요소 (Connected Component)의 개수를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 정점의 개수 N과 간선의 개수 M이 주어진다. (1 ≤ N ≤ 1,000, 0 ≤ M ≤ N×(N-1)/2) 둘째 줄부터 M개의 줄에 간선의 양 끝점 u와 v가 주어진다. (1 ≤ u, v ≤ N, u ≠ v) 같은 간선은 한 번만 주어진다.

## 출력

첫째 줄에 연결 요소의 개수를 출력한다.

## 예제 입력 1

```
6 5
1 2
2 5
5 1
3 4
4 6
```

## 예제 출력 1

```
2
```

## 예제 입력 2

```
6 8
1 2
2 5
5 1
3 4
4 6
5 4
2 4
2 3
```

## 예제 출력 2

```
1
```

---

## ☺︎ a/t

1. 각 정점의 인접 정점을 저장할 연결 리스트를 생성
  1. 이 연결 리스트는 각 정점마다 하나씩 있어야 하므로, 연결 리스트의 배열 또는 ArrayList<ArrayList<Integer>> 형태로 저장
2. 탐색 알고리즘은
  1. DFS를 사용. 깊이 우선 탐색으로 각 연결 요소들의 끝까지 완전히 탐색해야 한다.

---

## ☺︎ Snippets

```java
public class Main {

    static ArrayList<ArrayList<Integer>> g;
    static boolean visit[];
    static int n, m;
    static int count;
    static int subNode;

    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        //정점 N, 간선 M 입력
        String[] nm = br.readLine().split(" ");
        n = Integer.parseInt(nm[0]);
        m = Integer.parseInt(nm[1]);

        //인접 리스트 할당
        g = new ArrayList<>();

        //노드 개수 만큼 각 arraylist 초기화

        for (int i = 0; i <= n; i++) {
            g.add(i, new ArrayList<>());
        }

        //무방향 그래프 간선 연결, 간선 개수 m만큼 반복
        for (int i = 0; i < m; i++) {
            String[] ab = br.readLine().split(" ");
            int a = Integer.parseInt(ab[0]);
            int b = Integer.parseInt(ab[1]);
            g.get(a).add(b);
            g.get(b).add(a);
        }
        //출력
        print();
        //방문 배열 초기화
        visit = new boolean[n + 1];
        //카운트 초기화
        count = 0;

        for (int i = 0; i <= n; i++) {
            for (int j = 0; j < g.get(i).size(); j++) {
                int node = g.get(i).get(j);
                if (!visit[node]) {
                    dfs(g, node, visit);
                    count++;
                }
            }
        }
        for (int i = 1; i <= n; i++) {
            if (!visit[i]) {
                subNode++;
            }
        }

        if (m == 0) {
            System.out.print(n);
        } else {
            System.out.print(count + subNode);
        }

    }

    //dfs
    private static void dfs(ArrayList<ArrayList<Integer>> g, int node, boolean[] visit) {
        visit[node] = true;
        for (int v : g.get(node)) {
            if (!visit[v]) {
                dfs(g, v, visit);
            }
        }
    }

    private static void print() {
        for (ArrayList<Integer> subList : g) {
            System.out.println(subList);
        }
    }
}
```

---

## 시간 소요한 것

### 1. ArrayList<ArrayList<Integer>>

자바에서 `ArrayList<ArrayList<Integer>>`는 임의의 수의 ArrayList를 보유할 수 있는 동적인 데이터 구조이다.

`ArrayList<ArrayList<Integer>>`는 사실상 2차원 배열처럼 작동한다. 또한 ArrayList의 특성에 따라 크기가 동적으로 변경될 수 있다.

```
1 -> 2 -> 5
2 -> 1 -> 5
3 -> 4
4 -> 3 -> 6
5 -> 2 -> 1
6 -> 4
```

위의 그래프를 `ArrayList<ArrayList<Integer>>` 형식으로 표현하면

```java
[
  [2, 5], // 1번 노드의 인접 노드 리스트
  [1, 5], // 2번 노드의 인접 노드 리스트
  [4],    // 3번 노드의 인접 노드 리스트
  [3, 6], // 4번 노드의 인접 노드 리스트
  [2, 1], // 5번 노드의 인접 노드 리스트
  [4]     // 6번 노드의 인접 노드 리스트
]
```

이 구조는 **정점의 번호를 인덱스로 사용**하여 해당 정점의 **인접 노드 리스트에 접근한다.**

---

### 2. 연결 리스트 생성

탐색 문제 풀 때, 인접 행렬을 주로 사용했어서 처음에 구현을 못 하였다. 문제 예제에 있는 노드들을 연결 리스트로 표현하면 이렇게 구현할 수 있고

```java
1 -> 2 -> 5
2 -> 1 -> 5
3 -> 4
4 -> 3 -> 6
5 -> 2 -> 1
6 -> 4
```

방문 순서는

| 순서 | 방문 노드 | 방문 리스트 |
| --- | --- | --- |
| 1 | 1 | [1] |
| 2 | 2 | [1, 2] |
| 3 | 5 | [1, 2, 5] |
| 4 | 1 (이미 방문) | [1, 2, 5] |
| 5 | 2 (이미 방문) | [1, 2, 5] |
| 6 | 3 | [1, 2, 5, 3] |
| 7 | 4 | [1, 2, 5, 3, 4] |
| 8 | 6 | [1, 2, 5, 3, 4, 6] |

---

### 3. 스택이 아닌 DFS

```java
 private static void dfs(ArrayList<ArrayList<Integer>> g, int node, boolean[] visit) {
        visit[node] = true;
        for (int v : g.get(node)) {
            if (!visit[v]) {
                dfs(g, v, visit);
            }
        }
    }
```

- g.get(node)
  - 다차원 ArrayList에서 **node 번째 인덱스에 위치**한 ArrayList를 반환
  - node가 3이라면, 3번 정점과 직접 연결된 모든 정점 리스트를 반환

만약 그래프가 아래와 같이 연결되어 있다고 가정하면,

```java
1 - 2
|   |
5 - 3 - 4
    |
    6
```

`g.get(3)`은 [2, 4, 6]을 반환한다.

---

</details>
</summary>


---

# ☻ 잃어버린 괄호

<summary>
<details>

2024년 3월 25일

## 문제

세준이는 양수와 +, -, 그리고 괄호를 가지고 식을 만들었다. 그리고 나서 세준이는 괄호를 모두 지웠다.

그리고 나서 세준이는 괄호를 적절히 쳐서 이 식의 값을 최소로 만들려고 한다.

괄호를 적절히 쳐서 이 식의 값을 최소로 만드는 프로그램을 작성하시오.

## 입력

첫째 줄에 식이 주어진다. 식은 ‘0’~‘9’, ‘+’, 그리고 ‘-’만으로 이루어져 있고, 가장 처음과 마지막 문자는 숫자이다. 그리고 연속해서 두 개 이상의 연산자가 나타나지 않고, 5자리보다 많이 연속되는 숫자는 없다. 수는 0으로 시작할 수 있다. 입력으로 주어지는 식의 길이는 50보다 작거나 같다.

## 출력

첫째 줄에 정답을 출력한다.

## 예제 입력 1

```
55-50+40
```

## 예제 출력 1

```
-35
```

## 예제 입력 2

```
10+20+30+40
```

## 예제 출력 2

```
100
```

## 예제 입력 3

```
00009-00009
```

## 예제 출력 3

```
0
```

---

## ☺︎ a/t

### 1. 목표

최솟값 만들기

### 2. - 연산자

1. ‘-’ 연산자 뒤에 오는 수가 클 수록 최솟값으로 만들 수 있다.
2. ‘-’ 연산자 사이에 ‘+’ 연산자가 올 수 있는데, 함께 연산하지 않는다. ‘-’로 분리한다.
  1. '+' 연산자의 우선순위가 높기 때문에 먼저 계산이 이뤄진다.

---

## ☺︎ Snippets

### 1차 풀이

```java
public class Main2 {
    static String[] nums;
    static int[] res;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        //식 입력 받기
        String s = br.readLine();

        // - 를 기준으로 구분해서 배열 저장
        nums = s.split("[-]");
        res = new int[nums.length];

        //각 원소들은 +로 연결되어있거나 or Not
        // +로 연결되어 있는 경우 두 숫자를 더하고 int 배열에 합계 형태로 저장
        int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            String a = nums[i];
            if (a.contains("+")) {
                String[] ab = a.split("[+]");
                for (String ele : ab) {
                    sum += Integer.parseInt(ele);
                }
                res[i] = sum;
                sum = 0;
            } else {
                res[i] = Integer.parseInt(a);
            }
        }

        // 첫 번째 원소 부터 빼기 연산
        int result = res[0];
        for (int i = 1; i < res.length; i++) {
            result -= res[i];
        }
        System.out.print(result);
    }
} 
```

### 2차: 스택으로 풀기

```java
public class Main3_Stack {
    static Stack<Integer> stk;
    static String[] nums;
    static String[] strings;
    static int[] strToInt;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        //식 입력 받기
        String s = br.readLine();

        //String 참조 변수 배열
        nums = s.split("[-]");

        //+ 포함하는 원소 합계 구하기
        strToInt = new int[nums.length];
        int a = 0;
        String str = "";
        for (int i = 0; i < nums.length; i++) {

            str = nums[i];

            if (str.contains("+")) {
                strings = str.split("[+]");
                for (int j = 0; j < strings.length; j++) {
                    a += Integer.parseInt(strings[j]);
                }
                strToInt[i] = a;
                a = 0;
            } else {
                strToInt[i] = Integer.parseInt(nums[i]);
            }
        }

        System.out.print(stack());
    }

    private static int stack() {
        stk = new Stack<Integer>();

        int result = strToInt[0];
        //첫 번째 원소 저장
        stk.push(strToInt[0]);
        int start = 1;
        while (start < strToInt.length) {
            stk.push(strToInt[start]);
            start++;
        }
        while (stk.size() > 1) {
            result -= stk.pop();
        }
        return result;

    }
}
```

---

## 시간 소요한 것

이게 뭐야..

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/963e2661-4b57-46a7-8aa2-4cbda052382a)


### 1. split 메소드: 두 개 이상 문자열 분리해서 저장

```java
String[] res = s.split("[+\\-]");

// 주어진 문자열 s를 '+' 또는 '-'를 기준으로 분리
```

### 2. 이걸 그리디 알고리즘이라고 볼 수 있는 것인지?

각 단계에서 최적의 선택을 하여 최종적 해답에 도달 → ‘+’로 묶인 숫자들을 먼저 계산하고, 그 결과로 ‘-’로 연결된 숫자들에 차례로 빼주면서 최솟값을 찾아낸다.

### 3. 활용할 수 있는 자료구조

스택, 덱, 큐 등의 자료구조를 활용할 수 있지 않을까 → 스택을 활용할 수 있을 것 같다.

### 4. NFE

엣지 케이스 고려 못 하고 NFE 발생 시켰다. 2번째 테케로 디버깅 하면서 첫 번째 연산자로 +가 오는 것을 간과했다.

- 첫 번째 원소는 따로 변수에 저장 → `1+2` **Integer.parseInt → “+” → NFE**

```java
//1
1+2-3+4-5
//2
55-50+40
```

## 5. 스택으로도 sol

> 두 풀이 다 성능 124ms로 똑같다.


![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/1d656b64-e80a-48d4-88a9-77b82bfc2961)

|  | 1차 | 2차 (스택 사용) |
| --- | --- | --- |
| 입력 문자열을 "-"로 분리 | O(n) | O(n) |
| 분리된 각 문자열에서 "+" 찾기 | O(n) | O(n) |
| 각 문자열을 정수로 변환 | O(1) | O(1) |
| 숫자를 스택에 넣기 | N/A | O(1) |
| 모든 숫자를 더하거나 빼기 | O(n) | O(n) |
| 총 시간 복잡도 | O(n) | O(n) |

### 스택

문제 로직 자체를 우선 - 연산자를 기준으로 전체 String을 분리하고, 그 이후의 연산은 다 -로 처리하도록 로직을 생각했다.

그래서 스택에 전체 숫자들을 저장한 후에, 스택에서 꺼내면서 - 연산을 수행할 수 있겠다고 생각해서 스택을 사용했는데 성능은 비슷하다.

```java
//생략
    private static int stack() {
        stk = new Stack<Integer>();

        int result = strToInt[0];
        //첫 번째  저장
        stk.push(strToInt[0]);
        int start = 1;
        while (start < strToInt.length) {
            stk.push(strToInt[start]);
            start++;
        }
        while (stk.size() > 1) {
            result -= stk.pop();
        }
        return result;

    }
```

- '-' 연산자를 기준으로 분리한 후, '+' 연산자로 연결된 숫자들을 묶어서 계산하고, 이를 스택에 저장
- 이후 스택에 저장된 값을 순차적으로 빼면서 결과를 계산

---

</details>
</summary>

---

# ☻ 숨바꼭질

<summary>

<details>

2024년 3월 26일

## 문제

수빈이는 동생과 숨바꼭질을 하고 있다. 수빈이는 현재 점 N(0 ≤ N ≤ 100,000)에 있고, 동생은 점 K(0 ≤ K ≤ 100,000)에 있다. 수빈이는 걷거나 순간이동을 할 수 있다. 만약, 수빈이의 위치가 X일 때 걷는다면 1초 후에 X-1 또는 X+1로 이동하게 된다. 순간이동을 하는 경우에는 1초 후에 2*X의 위치로 이동하게 된다.

수빈이와 동생의 위치가 주어졌을 때, 수빈이가 동생을 찾을 수 있는 가장 빠른 시간이 몇 초 후인지 구하는 프로그램을 작성하시오.

## 입력

첫 번째 줄에 수빈이가 있는 위치 N과 동생이 있는 위치 K가 주어진다. N과 K는 정수이다.

## 출력

수빈이가 동생을 찾는 가장 빠른 시간을 출력한다.

## 예제 입력 1

```
5 17
```

## 예제 출력 1

```
4
```

## 힌트

수빈이가 5-10-9-18-17 순으로 가면 4초만에 동생을 찾을 수 있다.

---

## ☺︎ a/t

### 활용할 자료구조

1. 트리
  1. 각 연산을 결과로하는 트리를 만든다.
  2. 동생이 있는 K위치의 노드 Depth가 최단거리이다.
2. 연결 리스트
  1. 시간 복잡도 최악 예상
3. 그래프
  1. BFS로 연결된 노드 탐색
  2. K 발견되면 레벨 출력

---

## Snippets

```jsx

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

/***
 * @Point
 *
 * - BFS, 연결 리스트
 * - K 노드 찾을 때 까지 BFS 수행하고 레벨 출력
 * @Review
 * 노드 몇 개 생성 될지 몰라서 방문 배열 ArrayList로 생성
 * 숫자 커질 수록 방문 여부 체크할 때마다 전체 탐색 해야 함 > 당연히 시간 초과
 */
public class Main {

    //방문 배열
    static boolean[] visit;
    //n,k
    static int n, k;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        //n,k 입력받기
        String[] nk = br.readLine().split(" ");
        n = Integer.parseInt(nk[0]);
        k = Integer.parseInt(nk[1]);

        visit = new boolean[200002];
        //Call bfs
        bfs(n);
    }

    private static void bfs(int Node) {
        int[] operation = {2, 1, -1};

        Queue<Dis> q = new LinkedList<>();

        q.add(new Dis(Node, 0));

        visit[Node] = true;

        //3 operation
        //2*x, x+1, x-1
        //연산결과 방문 체크, 방문 했다면 삽입 안함
        while (!q.isEmpty()) {

            Dis x = q.poll();

            //노드값,거리, 방문 여부
            int nowNode = x.getNode();
            int nowDis = x.getDis();

            //k 찾으면 종료 하고 Distance 리턴
            if (nowNode == k) {
                System.out.print(nowDis);
                return;
            }

            //방문 했다면 삽입 안함
            for (int i = 0; i < operation.length; i++) {
                int newNode = 0;

                //곱, +- 연산 분리
                if (i == 0) {
                    newNode = operation[0] * nowNode;
                } else {
                    newNode = operation[i] + nowNode;
                }

                if (newNode < 0 || newNode >= 200002) {
                    continue;
                }
                if (!visit[newNode]) {
                    visit[newNode] = true;
                    q.add(new Dis(newNode, nowDis + 1));
                }

            }
        }
    }
    public static class Dis {
        private final int node;
        private final int dis;

        public Dis(int node, int dis) {
            this.node = node;
            this.dis = dis;
        }

        public int getNode() {
            return node;
        }

        public int getDis() {
            return dis;
        }
    }
}

```

---

## 시간 소요한 것

### 1. 방문 배열

1. 인접 노드가 몇 개 생성될지 모르기에 처음에 Dis 클래스에 별도로 visit 변수를 생성하였다.
2. 그렇게 하니, 인접 노드를 삽입하기 전에 방문 여부를 체크할 수 없었기에 Delete

    ```jsx
    if (!visit[newNode]) { // 여기서 새로운 노드 방문 여부를 체크할 수 없음
        visit[newNode] = true;
        q.add(new Dis(newNode, nowDis + 1));
    }
    ```

3. 이후 ArrayList 형식으로 방문 배열 생성하고, `list.contains(newNode)` 로 방문 여부 체크
  1. 즉, 인접 노드를 ArrayList에 추가함 → **아주 당연히 시간초과**
    1. 숫자 커질수록 방문 여부 체크할 때마다 **ArrayList**를 탐색해야 함
4. 원래대로 `int[] visit` → 1차원 배열로 생성하고 배열 길이를 설정
  1. 수빈 위치 100,000 → 2*x 연산 시 갈 수 있는 위치 Max는 **200,000**
  2. 처음에 2000001로 설정해서 Out Of Bounds 에러 계속 발생 →
  3. 문제 다 풀어놓곤 범위 에러 적당히 하자
  ->  **데이터 최대/최소 범위 확인, Int,  Long 형으로 받을 지 구분, 0이하 음수로 떨어질 경우 구분**

    ```jsx
    visit = new boolean[200002];
    ```

    ```jsx
    if (newNode < 0 || newNode >= 200002) {
        continue;
    }
    ```


</details>
</summary>

---

