---
title: BaekJoon 0x05 Stack
date: 2024-02-15
categories: Algorithms
tags:
  [
    Algorithms
      Baekjoon
  ]
---

# [BaekJoon](https://www.acmicpc.net/)

| 문제 분류  |  문제   |     Section     |
|:------:|:-----:|:---------------:|
| 연습 문제  | 10828 |   [스택](#-스택)    |
| 기본 문제✔ | 10773 |   [제로](#-제로)    |
| 응용 문제✔ | 1874  | [스택수열](#-스택-수열) |
| 응용 문제✔ | 2493  |    [탑](#-탑)     |
| 응용 문제  | 6198  |                 |
| 응용 문제  | 17298 |                 |
| 응용 문제  | 3015  |                 |
| 응용 문제  | 6549  |                 |

---

2024년 2월 15일

# ☻ 스택

### BufferdReader 출력 비우기

## 문제

정수를 저장하는 스택을 구현한 다음, 입력으로 주어지는 명령을 처리하는 프로그램을 작성하시오.

명령은 총 다섯 가지이다.

- push X: 정수 X를 스택에 넣는 연산이다.
- pop: 스택에서 가장 위에 있는 정수를 빼고, 그 수를 출력한다. 만약 스택에 들어있는 정수가 없는 경우에는 -1을 출력한다.
- size: 스택에 들어있는 정수의 개수를 출력한다.
- empty: 스택이 비어있으면 1, 아니면 0을 출력한다.
- top: 스택의 가장 위에 있는 정수를 출력한다. 만약 스택에 들어있는 정수가 없는 경우에는 -1을 출력한다.

## 입력

첫째 줄에 주어지는 명령의 수 N (1 ≤ N ≤ 10,000)이 주어진다. 둘째 줄부터 N개의 줄에는 명령이 하나씩 주어진다. 주어지는 정수는 1보다 크거나 같고, 100,000보다 작거나 같다. 문제에 나와있지
않은 명령이 주어지는 경우는 없다.

## 출력

출력해야하는 명령이 주어질 때마다, 한 줄에 하나씩 출력한다.

## 예제 입력 1

```
14
push 1
push 2
top
size
empty
pop
pop
pop
size
empty
pop
push 3
empty
top
```

## 예제 출력 1

```
2
2 
0
2
1
-1
0
1
-1
0
3
```

# ☺︎ a/t

1. n회만큼의 연산을 반복
    1. 입력: 매회 반복마다 토큰 초기화하며, StringTokenizer [0]번째 토큰으로 연산 수행
    2. 두 번째 토큰 Input
2. 한 줄에 하나씩 출력해야 하므로, 각 연산을 완료 후 출력 후 해당 반복 종료

# ☺︎ Snippets

> 콘솔에 출력이 되지 않음
>

```java
public class Main {
    public static void main(String[] args) throws IOException {
        Stack<Integer> stk = new Stack<>();
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());

        for (int i = 0; i < n; i++) {

            // 토큰에 input 저장
            StringTokenizer st = new StringTokenizer(br.readLine(), " ");
            int count = st.countTokens();
            String[] arr = new String[count];

            // Converting each token to array
            for (int x = 0; x < arr.length; x++) {
                arr[x] = st.nextToken();
            }

            // 연산 start
            if (arr[0] == "push") {
                Integer ele = Integer.parseInt(arr[1]);
                stk.push(ele);

            } else if (arr[0] == "pop") {
                if (stk.pop() == -1) {
                    System.out.println("-1");
                } else {
                    Integer popped = stk.pop();
                    System.out.print(popped);
                }

            } else if (arr[0] == "top") {
                if (stk.peek() == 0) {
                    System.out.println("-1");
                } else {
                    Integer peeked = stk.peek();
                    System.out.print(peeked);
                }

            } else if (arr[0] == "size") {
                Integer size = stk.size();
                System.out.print(size);

            } else if (arr[0] == "empty") {
                if (stk.isEmpty()) {
                    System.out.println("1");
                } else {
                    System.out.println("0");
                }
            }
        }
    }
}
```

> 2차
>

```java
public class Main {
    public static void main(String[] args) throws IOException {
        Stack<Integer> stk = new Stack<>();
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());

        for (int i = 0; i < n; i++) {

            // 토큰에 input 저장
            StringTokenizer st = new StringTokenizer(br.readLine(), " ");
            int count = st.countTokens();
            String[] arr = new String[count];

            // Converting each token to array
            for (int x = 0; x < arr.length; x++) {
                arr[x] = st.nextToken();
            }

            // 연산 start
            if (arr[0].equals("push")) {
                Integer ele = Integer.parseInt(arr[1]);
                stk.push(ele);

            } else if (arr[0].equals("pop")) {
                if (stk.isEmpty()) {
                    System.out.println("-1");
                } else {
                    int popped = stk.pop();
                    System.out.println(popped);
                }

            } else if (arr[0].equals("top")) {
                if (stk.isEmpty()) {
                    System.out.println("-1");
                } else {
                    int peeked = stk.peek();
                    System.out.println(peeked);
                }

            } else if (arr[0].equals("size")) {
                int size = stk.size();
                System.out.println(size);

            } else if (arr[0].equals("empty")) {
                if (stk.isEmpty()) {
                    System.out.println("1");
                } else {
                    System.out.println("0");
                }
            }
        }
    }
}
```

> 콘솔
>

```jsx
2
2
0
2
1
- 1
0
1
- 1
0

3
```

마지막 push 3 이후, peek 명령 직후 아무것도 출력되지 않고, 다음 명령을 입력하고 엔터를 누를 때 비로소 결과 값 ‘3’이 출력된다.

# 표준 출력 버퍼가 ‘\n’을 대기

'peek' 연산은 스택의 맨 위에 있는 정수를 출력하는 연산이다. 예를 들어, 'push 1', 'push 2', 'push 3' 명령을 수행한 후에 'peek' 명령을 수행하면 '3'이 출력되어야 한다.
그러나 'peek' 명령을 수행한 직후에는 아무것도 출력되지 않고, 다음 명령을 입력하고 엔터를 누를 때 비로소 '3'이 출력되었다.

이는 표준 출력 버퍼가 개행 문자('\n')를 만날 때까지 출력을 저장해 두었다가 한 번에 출력하기 때문이다. ‘peek’ 는 개행문자를 출력하지 않으므로, peek 명령의 결과는 버퍼에 저장되어 있다. peek
명령 이후에 개행 문자를 출력하거나, flush() 명령을 사용해서 수동으로 버퍼를 비워줘야 한다.

> 이전의 ‘peek’ 명령은 정상적으로 실행되었다.
>

그 명령 다음에 바로 출력을 요구하는 명령이 있었고, 다음 명령을 수행하면서 버퍼에 저장된 ‘peek’ 명령의 결과가 출력되었다.

> 그러나, 마지막 턴에서는
>

‘peek’ 명령 후에 더 이상 출력을 요구하는 명령이 없었기 때문에, peek 명령의 결과가 버퍼에 남아있게 된다. 버퍼는 일반적으로 줄바꿈 발생, 출력 버퍼 가득 참, 프로그램 종료 시 비워진다. 따라서,
‘top’ 명령을 수행한 후 엔터를 입력해야 출력 버퍼가 비워지고 ‘3’이 출력되었던 것이다. → flush() 호출하여 출력 버퍼 수동으로 비워준다.


---
2024년 2월 15일

# ☻ 제로

| 시간 제한 | 메모리 제한 | 제출    | 정답    | 맞힌 사람 | 정답 비율   |
|-------|--------|-------|-------|-------|---------|
| 1 초   | 256 MB | 88555 | 59982 | 48945 | 68.002% |

## 문제

나코더 기장 재민이는 동아리 회식을 준비하기 위해서 장부를 관리하는 중이다.

재현이는 재민이를 도와서 돈을 관리하는 중인데, 애석하게도 항상 정신없는 재현이는 돈을 실수로 잘못 부르는 사고를 치기 일쑤였다.

재현이는 잘못된 수를 부를 때마다 0을 외쳐서, 가장 최근에 재민이가 쓴 수를 지우게 시킨다.

재민이는 이렇게 모든 수를 받아 적은 후 그 수의 합을 알고 싶어 한다. 재민이를 도와주자!

## 입력

첫 번째 줄에 정수 K가 주어진다. (1 ≤ K ≤ 100,000)

이후 K개의 줄에 정수가 1개씩 주어진다. 정수는 0에서 1,000,000 사이의 값을 가지며, 정수가 "0" 일 경우에는 가장 최근에 쓴 수를 지우고, 아닐 경우 해당 수를 쓴다.

정수가 "0"일 경우에 지울 수 있는 수가 있음을 보장할 수 있다.

## 출력

재민이가 최종적으로 적어 낸 수의 합을 출력한다. 최종적으로 적어낸 수의 합은 231-1보다 작거나 같은 정수이다.

## 예제 입력 1

```
4
3
0
4
0

```

## 예제 출력 1

```
0

```

## 예제 입력 2

```
10
1
3
5
4
0
0
7
0
0
6
```

## 예제 출력 2

```
7
```

## 힌트

예제 2의 경우를 시뮬레이션 해보면,

- [1]
- [1,3]
- [1,3,5]
- [1,3,5,4]
- [1,3,5] (0을 불렀기 때문에 최근의 수를 지운다)
- [1,3] (0을 불렀기 때문에 그 다음 최근의 수를 지운다)
- [1,3,7]
- [1,3] (0을 불렀기 때문에 최근의 수를 지운다)
- [1] (0을 불렀기 때문에 그 다음 최근의 수를 지운다)
- [1,6]

합은 7이다.

# ☺︎ Snippets

> 성공
>

```java
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int k = sc.nextInt();

        //Declare 스택
        Stack<Integer> stack = new Stack<>();

        /**
         * for문으로 stack에 값 입력
         * 만약, "0"이면 가장 최근에 stack에 입력된 값 pop, 0일 경우, !stack.isEmpty 보장
         * 아니면, push
         */
        for (int i = 0; i < k; i++) {
            int num = sc.nextInt();
            if (num == 0) {
                stack.pop();
            } else {
                stack.push(num);
            }
        }

        /**
         * print: 스택의 모든 원소 합
         */
        int answer = 0;
        for (int element : stack) {
            answer += element;
        }
        System.out.println(answer);
    }
}
```



> BufferdReader로 풀기

```jsx
식은
동일
BufferedReader
br = new BufferedReader(new InputStreamReader(System.in));
int
k = Integer.parseInt(br.readLine());
```

> 메모리, 시간

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/95ec89fe-541d-42df-a054-4e5c9fb0ae3f)

위가 BufferdReader, 아래가 Scanner

---

# ☻ 스택 수열

| 시간 제한 | 메모리 제한 | 제출 | 정답 | 맞힌 사람 | 정답 비율 |
| --- | --- | --- | --- | --- | --- |
| 2 초 | 128 MB | 152238 | 59182 | 41275 | 37.941% |

## 문제

스택 (stack)은 기본적인 자료구조 중 하나로, 컴퓨터 프로그램을 작성할 때 자주 이용되는 개념이다. 스택은 자료를 넣는 (push) 입구와 자료를 뽑는 (pop) 입구가 같아 제일 나중에 들어간 자료가 제일 먼저 나오는 (LIFO, Last in First out) 특성을 가지고 있다.

1부터 n까지의 수를 스택에 넣었다가 뽑아 늘어놓음으로써, 하나의 수열을 만들 수 있다. 이때, 스택에 push하는 순서는 반드시 오름차순을 지키도록 한다고 하자. 임의의 수열이 주어졌을 때 스택을 이용해 그 수열을 만들 수 있는지 없는지, 있다면 어떤 순서로 push와 pop 연산을 수행해야 하는지를 알아낼 수 있다. 이를 계산하는 프로그램을 작성하라.

## 입력

첫 줄에 n (1 ≤ n ≤ 100,000)이 주어진다. 둘째 줄부터 n개의 줄에는 수열을 이루는 1이상 n이하의 정수가 하나씩 순서대로 주어진다. 물론 같은 정수가 두 번 나오는 일은 없다.

## 출력

입력된 수열을 만들기 위해 필요한 연산을 한 줄에 한 개씩 출력한다. push연산은 +로, pop 연산은 -로 표현하도록 한다. 불가능한 경우 NO를 출력한다.

## 예제 입력 1

```
8
4
3
6
8
7
5
2
1

```

## 예제 출력 1

```
+
+
+
+
-
-
+
+
-
+
+
-
-
-
-
-
```

## 예제 입력 2

```
5
1
2
5
3
4
```

## 예제 출력 2

```
NO
```

## 힌트

1부터 n까지에 수에 대해 차례로 [push, push, push, push, pop, pop, push, push, pop, push, push, pop, pop, pop, pop, pop] 연산을 수행하면 수열 [4, 3, 6, 8, 7, 5, 2, 1]을 얻을 수 있다.

# ☺︎ a/t

1. 콘솔에 한 줄에 하나씩 입력되는 순서대로 stack에 들어가야 함
2. 스택에 순서대로 값을 넣기 위해
   1. 배열에 숫자들을 입력해두고, 꺼내서 Stack에 Push 하며 “+”를 출력
   2. 만약 4,3,6 순서대로 스택에 입력하고 싶다면 4까지 넣은 이후 4를 pop, 이후 3을 pop, 그리고 6은 마지막 입력값 다음인 5-6을 입력 후 6 pop

# ☺︎ Snippets

> 1차
>

```java
public class Main {
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();

        int start = 1;

        for (int i = 0; i < n; i++) {
            int x = sc.nextInt();

            while (start <= x) {
                stack.push(start);
                System.out.println("+");

                // peek 4일 때, x와 동일하니 pop
                while (stack.peek() != x) {
                    System.out.println("-");
                }
                start++;
            }
            start = start + x;
        }
    }
}
```

어떻게 풀어가야 할 지는 알겠는데, 구현하는게 어려웠다.

> 2차 메모리 초과
>

```java
import java.util.Scanner;
import java.util.Stack;

/**
 * start 변수를 언제 초기화해야 하는지
 */

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        StringBuilder sb = new StringBuilder();
        Stack<Integer> stack = new Stack<>();
        int start = 0;

        int n = sc.nextInt();
        for (int k = 0; k < n; k++) {
            int x = sc.nextInt();

            // start < x라면, start 부터 x 까지 삽입해줘야 함
            // 어디까지 입력 받았는지 알기 위해, start를 x로 초기화 (x까지 입력했기 때문에 5부터 push해주기 위함)
            // 4까지 입력, 6을 push하기 위해 next 5부터 입력되어야 함
            if (start < x) {
                for (int i = start + 1; i <= x; i++) {
                    stack.push(i);
//                    System.out.println("Push: " + i);
                    sb.append("+").append("\n");
                }
                start = x;
            }
            // stack의 peek과, x과 같은 경우 Pop
            if (stack.peek() == x) {
                stack.pop();
                sb.append("-").append("\n");
            } else {
                sb.append("NO").append("\n");
            }
        }
        System.out.println(sb);
    }
}
```

- **Things to Improve**
   - start 변수 초기화 시점 (4까지 입력되었고, pop을 완료했다. 이후 6을 push하기 위해선 5~6이 push 되어야 하므로, x값으로 reset 해주어야 한다.
      - 다음에 push할 값이 b/4 턴의 x보다 작아도 관게없다. 오름차순으로 stack에 push 되었을 것이다.
         - start<x로 e.i. 4 push 이후, 다음 Input이 3이다. `start < x`로 체크하고,  `stack.peek()==x`에서 true일 것이므로 pop될 수 있다.

> 3차
>

```java
public class Main {
    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();
        Stack<Integer> stack = new Stack<>();
        int start = 0;

        int n = Integer.parseInt(br.readLine());
        for (int k = 0; k < n; k++) {
            int x = Integer.parseInt(br.readLine());

            // start < x라면, start 부터 x 까지 삽입해줘야 함
            // 어디까지 입력 받았는지 알기 위해, start를 x로 초기화 (x까지 입력했기 때문에 5부터 push해주기 위함)
            // 4까지 입력, 6을 push하기 위해 next 5부터 입력되어야 함
            if (start < x) {
                for (int i = start + 1; i <= x; i++) {
                    stack.push(i);
//                    System.out.println("Push: " + i);
                    sb.append("+").append("\n");
                }
                start = x;
            }
            // stack의 peek과, x과 같은 경우 Pop
            if (stack.peek() == x) {
                stack.pop();
                sb.append("-").append("\n");
            } else {
                System.out.println("NO");
                return;
            }
        }
        System.out.println(sb);
    }
}
```

- Things to Improve
   - “NO” 출력할 때도 sb에 추가함. → return하기에 시스템 종료되는데, sb에 추가함.

---


# ☻ 탑

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/3e88edc8-a938-41c3-a6ea-a4079b36f469)

2024년 2월 23일 2024년 2월 24일 2024년 2월 26일

# **☻ 탑**

| 시간 제한 | 메모리 제한 | 제출 | 정답 | 맞힌 사람 | 정답 비율 |
| --- | --- | --- | --- | --- | --- |
| 1.5 초 | 128 MB | 72980 | 25052 | 16747 | 32.324% |

## 문제

KOI 통신연구소는 레이저를 이용한 새로운 비밀 통신 시스템 개발을 위한 실험을 하고 있다. 실험을 위하여 일직선 위에 N개의 높이가 서로 다른 탑을 수평 직선의 왼쪽부터 오른쪽 방향으로 차례로 세우고, 각 탑의 꼭대기에 레이저 송신기를 설치하였다. 모든 탑의 레이저 송신기는 레이저 신호를 지표면과 평행하게 수평 직선의 왼쪽 방향으로 발사하고, 탑의 기둥 모두에는 레이저 신호를 수신하는 장치가 설치되어 있다. 하나의 탑에서 발사된 레이저 신호는 가장 먼저 만나는 단 하나의 탑에서만 수신이 가능하다.

예를 들어 높이가 6, 9, 5, 7, 4인 다섯 개의 탑이 수평 직선에 일렬로 서 있고, 모든 탑에서는 주어진 탑 순서의 반대 방향(왼쪽 방향)으로 동시에 레이저 신호를 발사한다고 하자. 그러면, 높이가 4인 다섯 번째 탑에서 발사한 레이저 신호는 높이가 7인 네 번째 탑이 수신을 하고, 높이가 7인 네 번째 탑의 신호는 높이가 9인 두 번째 탑이, 높이가 5인 세 번째 탑의 신호도 높이가 9인 두 번째 탑이 수신을 한다. 높이가 9인 두 번째 탑과 높이가 6인 첫 번째 탑이 보낸 레이저 신호는 어떤 탑에서도 수신을 하지 못한다.

탑들의 개수 N과 탑들의 높이가 주어질 때, 각각의 탑에서 발사한 레이저 신호를 어느 탑에서 수신하는지를 알아내는 프로그램을 작성하라.

## 입력

첫째 줄에 탑의 수를 나타내는 정수 N이 주어진다. N은 1 이상 500,000 이하이다. 둘째 줄에는 N개의 탑들의 높이가 직선상에 놓인 순서대로 하나의 빈칸을 사이에 두고 주어진다. 탑들의 높이는 1 이상 100,000,000 이하의 정수이다.

## 출력

첫째 줄에 주어진 탑들의 순서대로 각각의 탑들에서 발사한 레이저 신호를 수신한 탑들의 번호를 하나의 빈칸을 사이에 두고 출력한다. 만약 레이저 신호를 수신하는 탑이 존재하지 않으면 0을 출력한다.

## 예제 입력 1

```
5
6 9 5 7 4
```

## 예제 출력 1

```
0 0 2 2 4
```

---

# ☺︎ a/t

30m

1. 일직선 위 N개의 높이가 다른 탑 왼쪽부터 오른쪽부터 있고, 하나의 탑에서 발사된 레이저 신호는 가장 먼저 만나는 단 하나의 탑에서만 수신한다.
2. 배열로 풀 수 있겠다는 생각을 했다.
    1. For문을 배열 끝에서 돌면서 배열의 한 원소씩 순회한 후 해당 값보다 크다면 그 배열의 인덱스 값을 출력한다.
3. 스택으로 풀 것이다.
    1. 우선 정렬은 하지 않는다. 해당 탑이 위치한 번호를 출력해야 하므로.
    2. 스택에 각 탑을 저장한다. 쌓여진 스택에서 Peek를 하면서, 해당 Peek값보다 크다면 신호를 발사할 수 있다. 계속 peek을 하면서, 밑에 쌓여진 스택 중에서 발사할 수 있는, 즉 해당 값보다 크다면 해당 스택의 인덱스를 가져온다.
    3. 발사할 수 있는 탑이 없다면 0을 출력한다.
4. 여기서 고민해야할 것, 스택의 peek를 통해 전체 스택을 조회할 수 있는지? for, while문을 돈다고 해도, 실제로 pop을 하지 않는 이상  peek는 똑같은 게 아닌지?

---

# ☺︎ Snippets

풀이법은 감이 잡혔으나, 어떻게 구현을 해야할지 정리되지 않았다.

- 발사 가능한 탑이 발견되면 해당 top을 pop 하고 다음 원소로 넘어가야 한다.
- 만약 발사 가능한 탑이 없다면 0을 출력하고 다음 원소로 넘어가야 한다.

즉, top에서 순회할 때 작은 원소를 찾았을 때도 Sb에 0을 추가하는 것이 원인이다. While문의 범위를 start로 지정하면 안 된다. 1까지 찾아갔는데도 발견하지 못 한다면 start는 계속되고, 스택 전체 순회를 위한 While문이 종료된다.

- While문의 범위를 전체 순회할 수 있도록 수정해야 하고, start가 감소하는 범위를 다시 설정해야 한다.

지금 문제점은 peek를 기준으로 스택의 전체 원소를 순회한 후, 큰 값(레이저를 발사할 수 있는)이 없으면 pop과 함께 0을 출력해야 한다.

```jsx
[지금 Top]: 5
--For Loop Start--
Stack 길이 : 3 // 탐색위치:  2// 탐색원소: 5
--For Loop Start--
Stack 길이 : 3 // 탐색위치:  1// 탐색원소: 9
Bigger than peeked!
현재 I: 2// 현재 J: 1
Idx of Bigger than top: 1/ 요소: 9

0 3 0 0 1 0 1 
```

### 문제

콘솔을 보면, 9와 비교한 이후 pop을 한다. 그리고 9의 인덱스 값인 1을 StringBuilder에 더해준다. 이후 9를 peek해야 하는데 종료가 된다.

### 원인

- 두 번째 for문에서 5를 기준으로 순회: 5에서 발사할 수 있는 **탑인 9의 위치는 1**
- 첫 번째 for문 i인 현재 5의 위치는 2
    - 첫 번째 for문의 i는 0에서 스택의 사이즈(5) 이하까지 계속 증가한다.
    - 그리고 5가 9를 찾고 pop 되었을 때, 스택의 사이즈는 2가 되었고, i가 2인 5에서 전체 for문이 종료되는 것이다.
    - 따라서 스택을 전체 순회하는 for문을 스택이 **isEmpty** 일 때 까지로 수정해야 할 것같다.

2024년 2월 24일

답은 출력되나 시간 초과였다.

```jsx
public class Main {
    public static void main(String[] args) throws IOException {

        Stack<Integer> stack = new Stack<>();

        StringBuilder sb = new StringBuilder();
        Scanner sc = new Scanner(System.in);
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(br.readLine());
        //스택에 push
        String[] tokens = br.readLine().split(" ");
        for (String token : tokens) {
            if (!token.trim().isEmpty()) {
                stack.push(Integer.parseInt(token.trim()));
            }
        }

        while (!stack.isEmpty()) {
            if (stack.isEmpty()) {
                break;
            }

            int peek = stack.peek();

            for (int j = stack.size() - 1; j >= 0; j--) {
                if (peek < stack.get(j)) {
                    int idx = stack.indexOf(stack.get(j)) + 1;
                    stack.pop();
                    sb.append(idx).append(" ");
                    //최대값 찾으면 I기준으로 탐색하는 For문 탈출
                    break;
                }
            }
            if (peek == stack.peek()) {
                stack.pop();
                sb.append("0").append(" ");
            }
        }
        System.out.println(sb.reverse().toString().trim());
        br.close();
    }
}
```

### 코드에서 병목 현상 찾기

특히 루프 수행하는 부분을 확인. 루프는 시간이 많이 소요될 수 있으므로 병목 현상이 발생할 가능성이 높다. **탑의 높이를 입력받는 즉시 발사 가능 여부를 확인하고**, 가능한 경우 바로 해당 정보를 저장하는 방식으로 진행하면 스택을 순회하는 과정을 생략할 수 있다.

처음엔 우선 전체 다 Stack에 Push하고 스택 전체를 순회했고 시간초과 였다.

---

# ☺︎ idea

- Top보다 크거나 작을 때, 언제 push하고 pop 해야 하는지 구현하는 것에 시간을 꽤 소요했다.
    - 현재 Top 보다 Next가 크다면, 현재의 Top을 삭제하고 0을 기록한다.
        - 다음에 입력될 탑들은 Next (더 큰 탑)에 레이저를 발사할 것이므로, 현재 기준으로 Max인 탑은 필요없다.
    - 현재 Top 보다 Next가 작다면, 현재 Top의 인덱스를 기록하고, Push한다.
        - 이후의 탑보다 Next가 크다면, 이 탑에 레이저를 발사해야 하므로 Push 해줘야 한다.
- Stack의 인덱스와 높이 (원소)를 저장할 클래스를 별도로 생성하고, static으로 변수들을 선언한다.

---

```jsx

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Stack;
import java.util.StringTokenizer;

public class Main {

    //Static 변수들 선언
    // 토큰 값들 저장할 배열 생성, 정답 변수, 스택
    public static int answer;
    public static String[] tokens;
    public static Stack<Tower> stack = new Stack<>();

    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();

        // 탑의 개수 n 입력
        int n = Integer.parseInt(br.readLine());

        //토큰 값들 저장할 배열 객체 생성
        tokens = new String[n];

        //토큰에 n개 탑 입력
        StringTokenizer stk = new StringTokenizer(br.readLine(), " ");
        for (int i = 0; i < tokens.length; i++) {
            tokens[i] = stk.nextToken();
//            System.out.println("토큰: " + tokens[i]);
        }

        // 탑 스택에 push or not
        for (int i = 0; i < tokens.length; i++) {

            //탑 불러오기
            int height = Integer.parseInt(tokens[i]);
//            System.out.println("New 탑: " + height);

            //스택 비어있지않으면
            while (!stack.isEmpty()) {
                /**
                 * 9일 때 5, 9>5이니 9의 idx를 저장하고 5 push
                 * 5일 때 7, 5<7이니 pop을 해주고 나감
                 * 다시 돌아오면 top은 9, 9>7이니 idx를 저장하고 5 push
                 */
                if (stack.peek().height >= height) {

                    //현재 Top이 더 클 때, idx 기록
                    sb.append(stack.peek().idx + 1).append(" ");
                    //stack에 new 탑 push
                    stack.push(new Tower(i, height));
                    break;
                    //다음 탑으로 이동 종료
                }
                stack.pop();

            }

            //맨 처음, 스택 비어있다면 0입력, stack에 첫 번째 push
            if (stack.isEmpty()) {

//                System.out.println("토큰: " + tokens[i]);

                //6을 push할 것, idx는 0 token 0번지에 6저장
                stack.push(new Tower(i, Integer.parseInt(tokens[i])));
                sb.append("0").append(" ");
            }
        }
        System.out.print(sb.toString());
    }

    // 스택 원소 idx, height 저장
    static class Tower {
        int idx;
        int height;

        public Tower(int idx, int height) {
            this.idx = idx;
            this.height = height;
        }
    }

}

```