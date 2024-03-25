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

2024년 3월 10일

## 문제

준규가 가지고 있는 동전은 총 N종류이고, 각각의 동전을 매우 많이 가지고 있다.

동전을 적절히 사용해서 그 가치의 합을 K로 만들려고 한다. 이때 필요한 동전 개수의 최솟값을 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 N과 K가 주어진다. (1 ≤ N ≤ 10, 1 ≤ K ≤ 100,000,000)

둘째 줄부터 N개의 줄에 동전의 가치 Ai가 오름차순으로 주어진다. (1 ≤ Ai ≤ 1,000,000, A1 = 1, i ≥ 2인 경우에 Ai는 Ai-1의 배수)

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
        while (j >= 0) {
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

~~굳이 복잡하게 생각하니 코드 작성이 안 됐고, 필요한 것만 생각해서  흐름대로 코드를 썼고 성공. 후 ㅜ~~

---

## 풀이 정리

### 목표

주어진 동전으로, K라는 목표 값을 달성해야 한다.(즉, 주어진 동전을 최소한으로 사용해서 0원으로 만들어야 한다.)

5kg의 배낭을 최대한 많이 사용해서 봉지의 수를 최소화 해야 했던 설탕 배달이랑 유사한 문제로, 그리디 알고리즘이다.

### 그리디

각 단계에서 항상 **'가장 큰 가치의 동전으로 나누는' 최적의 선택**을 해야한다. 각 단계에서 최선의 선택을 함으로써, 전체적으로 최적의 결과를 도출해내는 것이 목표이기 때문임.

### 어떻게 풀었나

`j`가 0 이상인 동안 while 반복문이 계속된다. 반복문 안에서 j 인덱스에 있는 동전의 가치와, 목표 가치가 계속 비교된다. → 주어진 동전들로 0원을 만들어야 하기 때문에, k가 0이 될 때까지 이 반복문은 종료될 수 없다.

### 시간 많이 소요했던 부분

제어문 컨트롤 하는 것

### if, j 인덱스의 동전 가치가 k ≤ 라면

k 라는 목표 금액을 해당 동전으로 나눌 수 있다.

- 동전들이 오름차순으로 주어졌으니, **역순으로 탐색했다. 때문에 탐색 순서는 가장 가치가 큰 동전이 먼저 올 것이니, 나눌 수 있는 만큼 나눠준다.** 그리고 그 몫을 `count`에 더한다.
- `remain:` 동전으로 나누고 남은 가치, 즉 다른 동전으로 나눠야 할 금액
    - k라는 목표값을 remain 값으로 갱신해준다.
- 만약, k가 0이 되면, 즉 모든 동전을 나눈 결과가 목표 가치와 정확히 일치하면, 현재까지 count를 출력하고 메소드를 종료한다.

### 동전 가치 > k 라면

현재 동전 가치가 k보다 크므로, 해당 동전으로 k를 만들 수 없다. 그럼 위의 If문에 해당하지 않으므로 **j—;** 다음 동전으로 넘어간다.