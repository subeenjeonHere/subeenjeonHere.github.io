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
