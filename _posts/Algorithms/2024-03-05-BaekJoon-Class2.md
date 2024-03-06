---
title: BaekJoon Class-2
date: 2024-03-05
categories: Algorithms
tags:
  [
    Algorithms
      BaekJoon
  ]
---

# TOC
<!-- TOC -->
* [☻ 부녀회장이 될테야](#-부녀회장이-될테야)
    * [1차 시도에서 실패했던 이유](#1차-시도에서-실패했던-이유)

<!-- TOC -->



---

# ☻ 부녀회장이 될테야

2024년 2월 29일 2024년 3월 1일 2024년 3월 5일

## 문제

평소 반상회에 참석하는 것을 좋아하는 주희는 이번 기회에 부녀회장이 되고 싶어 각 층의 사람들을 불러 모아 반상회를 주최하려고 한다.

이 아파트에 거주를 하려면 조건이 있는데, “a층의 b호에 살려면 자신의 아래(a-1)층의 1호부터 b호까지 사람들의 수의 합만큼 사람들을 데려와 살아야 한다” 는 계약 조항을 꼭 지키고 들어와야 한다.

아파트에 비어있는 집은 없고 모든 거주민들이 이 계약 조건을 지키고 왔다고 가정했을 때, 주어지는 양의 정수 k와 n에 대해 k층에 n호에는 몇 명이 살고 있는지 출력하라. 단, 아파트에는 0층부터 있고 각층에는
1호부터 있으며, 0층의 i호에는 i명이 산다.

## 입력

첫 번째 줄에 Test case의 수 T가 주어진다. 그리고 각각의 케이스마다 입력으로 첫 번째 줄에 정수 k, 두 번째 줄에 정수 n이 주어진다

## 출력

각각의 Test case에 대해서 해당 집에 거주민 수를 출력하라.

## 제한

- 1 ≤ k, n ≤ 14

## 예제 입력 1

```
2
1
3
2
3
```

## 예제 출력 1

```
6
10
```

---

### ☺︎ at

“a층의 b호에 살려면 자신의 아래(a-1)층의 1호부터 b호까지 사람들의 수의 합” 이라고 했으므로, a층 b호는 a-1층의 b호까지의 **누적합** 알고리즘을 사용하면 될 것이라 판단했다.

> 1차 시도
>

```java
public class 부녀회장이될테야 {
    static int[] s3array;
    static int k;
    static int n;

    public static void main(String[] args) throws IOException {
        Scanner sc = new Scanner(System.in);
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();
        //14호까지 0층에 사람들을 채워준다

        int[] array = new int[15];
        for (int i = 0; i < array.length; i++) {
            array[i] = i + 1;
        }

        int tc = sc.nextInt();

        for (int x = 1; x <= tc; x++) {
            k = sc.nextInt();
            n = sc.nextInt();

            //3층이면 1,2,3 누적합을 구해와야 함
            for (int st = 1; st <= k; st++) {
                s3array = new int[array.length];

                //누적합의 0번은 원본 0번째와 항상 동일하므로 초기화
                s3array[0] = array[0];
                //1층 누적합
                for (int i = 1; i < 14; i++) {
                    s3array[i] = s3array[i - 1] + array[i];
                }
                array = s3array;
            }
            sb.append(s3array[n - 1]).append("\n");
        }
        System.out.println(sb.toString().trim());
    }
}
```

누적합으로 풀면 될 것이라 생각했는데 틀렸다.

---

> 2차 시도
>

```java
public class Main {
    static int[] s3array;
    static int k;
    static int n;

    public static void main(String[] args) throws IOException {
        Scanner sc = new Scanner(System.in);
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();

        // 0층 1- 14호 사람 채운다
        int[] array = new int[15];

        /**
         * a/t
         * 3층 3호 사람 수를 구하기 위해 누적합을 0층부터 차례로 구해야 하는지
         * 호근 다른 방법으로 구할 수 있는지 체크
         */
        int tc = sc.nextInt();
        s3array = new int[15];
        for (int i = 1; i <= tc; i++) {

            int k = sc.nextInt();
            int n = sc.nextInt();

            int start = 1;

            for (int p = 0; p < array.length; p++) {
                array[p] = p + 1;
            }
            s3array[0] = array[0];

            while (start <= k) {

                for (int x = 1; x <= array.length; x++) {
                    if (x > n) {
                        break;
                    }
                    s3array[x] = s3array[x - 1] + array[x];
                }
                array = s3array;
                //누적합으로 반복하니 sb에 전체 추가됨, 3층 10호일 땐 3층 10호만 count 해야하는데 1,2층까지 같이 count
                start++;
            }
            sb.append(s3array[n - 1]).append("\n");
        }
        System.out.println(sb.toString().trim());
    }
}
```

누적합을 사용해서 계산했다.

### 1차 시도에서 실패했던 이유

1차 시도에서는 누적합을 구하는 로직이 테스트 케이스의 수만큼 반복되며, 이 과정에서 누적합 배열을 새로 초기화하지 않고 계속 사용했다. 이로 인해 각 테스트 케이스에서 독립적인 누적합을 계산하는 것이 아니라 이전 테스트 케이스의 결과에 영향을 받게 되었다.

콘솔을 찍어보면서, 누적합 **배열을 초기화 해야하는데 초기화 되지 않은채로 누적합이 계산되는** 것을 확인했다. 따라서, 2차 시도에서는 0층에 사람을 채우는 원본 arr 배열을 각 테스트 케이스마다 진행될 수 있도록 했다.

그리고, 누적합 계산 시 호수를 초과하는 경우 계산을 중단하는 로직을 초기화하도록 했다.

---

# ☻ 소수찾기

2024년 3월 6일

## 문제

주어진 수 N개 중에서 소수가 몇 개인지 찾아서 출력하는 프로그램을 작성하시오.

## 입력

첫 줄에 수의 개수 N이 주어진다. N은 100이하이다. 다음으로 N개의 수가 주어지는데 수는 1,000 이하의 자연수이다.

## 출력

주어진 수들 중 소수의 개수를 출력한다.

## 예제 입력 1

```
4
1 3 5 7

10
1 2 3 4 5 6 7 8 9 10
```

## 예제 출력 1

```
3
```

---

## ☺︎ Snippets

```java
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(br.readLine());
        int[] arr = new int[n];
        StringTokenizer stk = new StringTokenizer(br.readLine(), " ");

        int st = 0;
        int count = 0;
        while (stk.hasMoreTokens()) {
            arr[st] = Integer.parseInt(stk.nextToken());
            System.out.println("Arr:" + arr[st]);
            if (isPrime(arr[st]) == 1) {
                count++;
            }
            st++;
        }
        System.out.println(count);
    }

    private static int isPrime(int i) {    

        //1은 소수가 아님
        if (i == 1) {
            return 0;
        }
        if (i == 2) {
            return 1;
        }
        for (int j = 2; j <= Math.sqrt(i); j++) {
            if (i % j == 0){
                return 0;
            }
        }
        return 1;
    }
}

```

`isPrime()` 함수에서는 입력받은 숫자 `i`가 소수인지 아닌지를 확인한다.

소수는 **1과 자기 자신 이외에는 어떤 수로도 나누어 떨어지지 않는 수**를 말한다. 따라서, 2부터 `i`의 제곱근까지의 수로 `i`를 나누어 보고 나머지가 0인 경우가 있으면 `i`는 소수가 아니므로 0을 반환하고, 그렇지 않으면 `i`는 소수이므로 1을 반환하도록 했다.

### Math.sqrt를 사용한 이유

수학적으로, 어떤 수의 약수는 그 수의 제곱근 이하에 반드시 존재하기 때문이다.

예를 들어, 36의 약수는 1, 2, 3, 4, 6, 9, 12, 18, 36이다. 제곱근은 6이며, 제곱근을 기준으로 약수들이 쌍을 이룬다. 따라서, 소수를 판별할 때는 그 수의 제곱근까지만 확인하면 구할 수 있다.

제곱근 이하의 수들을 나누었을 때, 0으로 나누어 떨어지는 경우가 있으면 그 수가 자기 자신 이외의 다른 약수를 가진다는 것을 의미하므로, 그 수는 소수가 아니다.

---

# ☻ 팰린드롬 수

2024년 3월 6일

## 문제

어떤 단어를 뒤에서부터 읽어도 똑같다면 그 단어를 팰린드롬이라고 한다. 'radar', 'sees'는 팰린드롬이다.

수도 팰린드롬으로 취급할 수 있다. 수의 숫자들을 뒤에서부터 읽어도 같다면 그 수는 팰린드롬수다. 121, 12421 등은 팰린드롬수다. 123, 1231은 뒤에서부터 읽으면 다르므로 팰린드롬수가 아니다. 또한 10도 팰린드롬수가 아닌데, 앞에 무의미한 0이 올 수 있다면 010이 되어 팰린드롬수로 취급할 수도 있지만, 특별히 이번 문제에서는 무의미한 0이 앞에 올 수 없다고 하자.

## 입력

입력은 여러 개의 테스트 케이스로 이루어져 있으며, 각 줄마다 1 이상 99999 이하의 정수가 주어진다. 입력의 마지막 줄에는 0이 주어지며, 이 줄은 문제에 포함되지 않는다.

## 출력

각 줄마다 주어진 수가 팰린드롬수면 'yes', 아니면 'no'를 출력한다.

## 예제 입력 1

```
121
1231
12421
0
```

## 예제 출력 1

```
yes
no
yes
```

---

## ☺︎ Snippets

> 1차 시도
>

```java

public class Main {
    public static void main(String[] args) throws IOException {
        Scanner sc = new Scanner(System.in);

        while (true) {
            String s = sc.next();

            if (s.equals("0")) {
                break;
            }

            //배열 생성
            String[] strings = new String[s.length()];
            for (int i = 0; i < strings.length; i++) {
                strings[i] = String.valueOf(s.charAt(i));
            }

            // 0 & strings.len-1
            // 1 & strings.len-2
            // start  < mid 일 때 까지
            for (int j = 0; j < strings.length / 2; j++) {

                // 같지 않다면 No 출력하고 종료하기
                if (!strings[j].equals(strings[strings.length - 1 - j])) {
                    System.out.println("No");
                    break;
                }
                if (j == strings.length / 2 - 1) {
                    System.out.println("S  : " + s + " : Yes");
                }
              
            }
        }
    }
}
```

> 2차 시도
>

### 실패했던 이유

1. 1자리일 때 고려하지 않았던 것
2. 제어문을 컨트롤 하는 것
  1. return, continue, break
  2. 특히나 1자리일 때 break;을 걸어서, 1자리가 입력되면 전체 메소드가 종료되었다. 1 121 1241 이렇게 올 수 있는 것인데, 바로 프로그램이 종료되었다. 이를 continue로 변경하고 해결
  3. 또한 팰린드롬 수를 판별하는 For loop에서 문자들을 비교하며, 문자열이 다르면 “no”를 출력하고 break을 걸었다. → 어차피 다르므로 계속 검사할 이유가 없다.
  4. 그리고 j를 계속 비교해서, break 에 걸리지 않고 len /2-1 까지 도달했다면 전체 비교 완료한 것이므로 yes를 출력한다.

```java
public class Main {
    public static void main(String[] args) throws IOException {
        Scanner sc = new Scanner(System.in);

        while (true) {
            String s = sc.next();

            if (s.equals("0")) {
                return;
            }
            if (s.length() == 1) {
                System.out.println("yes");
                continue;
            }

            //배열 생성
            String[] strings = new String[s.length()];
            for (int i = 0; i < strings.length; i++) {
                strings[i] = String.valueOf(s.charAt(i));
            }

            // 0 & strings.len-1
            // 1 & strings.len-2
            // start  < mid 일 때 까지
            for (int j = 0; j < strings.length / 2; j++) {
                // 같지 않다면 No 출력하고 종료하기
                if (!strings[j].equals(strings[strings.length - 1 - j])) {
                    System.out.println("no");
                    break;
                }
                if (j == strings.length / 2 - 1) {
                    System.out.println("yes");
                }

            }
        }
    }
}

```

---


