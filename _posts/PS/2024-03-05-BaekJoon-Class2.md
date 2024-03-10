---
title: BaekJoon Class-2
date: 2024-03-05
categories: PS
tags:
  [
    PS
  ]
---

### Algorithms
브루트포스, 이분탐색, 에라토스테네스의 체, 스택, 큐, 덱, 정렬, 해싱, 조합론

---

# TOC

<!-- TOC -->

| 문제 제목     | At                       |
|-----------|--------------------------| 
| 부녀회장이 될테야 | [부녀회장이 될테야](#-부녀회장이-될테야) |
| 소수찾기      | [소수찾기](#-소수찾기)           |
| 팰린드롬 수    | [팰린드롬 수](#-팰린드롬-수)       |
| 정렬        | [정렬](#-단어-정렬)            |
| 설탕 배달     | [설탕배달](#-설탕-배달)          |
| 벌집        | [벌집](#-벌집)               |
| 나이순정렬     | [나이순정렬](#-나이-순-정렬)       |

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

1차 시도에서는 누적합을 구하는 로직이 테스트 케이스의 수만큼 반복되며, 이 과정에서 누적합 배열을 새로 초기화하지 않고 계속 사용했다. 이로 인해 각 테스트 케이스에서 독립적인 누적합을 계산하는 것이 아니라 이전
테스트 케이스의 결과에 영향을 받게 되었다.

콘솔을 찍어보면서, 누적합 **배열을 초기화 해야하는데 초기화 되지 않은채로 누적합이 계산되는** 것을 확인했다. 따라서, 2차 시도에서는 0층에 사람을 채우는 원본 arr 배열을 각 테스트 케이스마다 진행될 수
있도록 했다.

그리고, 누적합 계산 시 호수를 초과하는 경우 계산을 중단하는 로직을 초기화하도록 했다.
1
---

# ☻ 소수찾기

2024년 3월 6일

## 문제

주어진 수 N개 중에서 소수가 몇 개인지 찾아서 출력하는 프로그램을 작성하시오.

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
            if (i % j == 0) {
                return 0;
            }
        }
        return 1;
    }
}

```

`isPrime()` 함수에서는 입력받은 숫자 `i`가 소수인지 아닌지를 확인한다.

소수는 **1과 자기 자신 이외에는 어떤 수로도 나누어 떨어지지 않는 수**를 말한다. 따라서, 2부터 `i`의 제곱근까지의 수로 `i`를 나누어 보고 나머지가 0인 경우가 있으면 `i`는 소수가 아니므로 0을
반환하고, 그렇지 않으면 `i`는 소수이므로 1을 반환하도록 했다.

### Math.sqrt를 사용한 이유

수학적으로, 어떤 수의 약수는 그 수의 제곱근 이하에 반드시 존재하기 때문이다.

예를 들어, 36의 약수는 1, 2, 3, 4, 6, 9, 12, 18, 36이다. 제곱근은 6이며, 제곱근을 기준으로 약수들이 쌍을 이룬다. 따라서, 소수를 판별할 때는 그 수의 제곱근까지만 확인하면 구할 수
있다.

제곱근 이하의 수들을 나누었을 때, 0으로 나누어 떨어지는 경우가 있으면 그 수가 자기 자신 이외의 다른 약수를 가진다는 것을 의미하므로, 그 수는 소수가 아니다.

---

# ☻ 팰린드롬 수

2024년 3월 6일

## 문제

어떤 단어를 뒤에서부터 읽어도 똑같다면 그 단어를 팰린드롬이라고 한다. 'radar', 'sees'는 팰린드롬이다.

수도 팰린드롬으로 취급할 수 있다. 수의 숫자들을 뒤에서부터 읽어도 같다면 그 수는 팰린드롬수다. 121, 12421 등은 팰린드롬수다. 123, 1231은 뒤에서부터 읽으면 다르므로 팰린드롬수가 아니다. 또한
10도 팰린드롬수가 아닌데, 앞에 무의미한 0이 올 수 있다면 010이 되어 팰린드롬수로 취급할 수도 있지만, 특별히 이번 문제에서는 무의미한 0이 앞에 올 수 없다고 하자.

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

# ☻ 단어 정렬

2024년 3월 6일

## 문제

알파벳 소문자로 이루어진 N개의 단어가 들어오면 아래와 같은 조건에 따라 정렬하는 프로그램을 작성하시오.

1. 길이가 짧은 것부터
2. 길이가 같으면 사전 순으로

단, 중복된 단어는 하나만 남기고 제거해야 한다.

## 입력

첫째 줄에 단어의 개수 N이 주어진다. (1 ≤ N ≤ 20,000) 둘째 줄부터 N개의 줄에 걸쳐 알파벳 소문자로 이루어진 단어가 한 줄에 하나씩 주어진다. 주어지는 문자열의 길이는 50을 넘지 않는다.

## 출력

조건에 따라 정렬하여 단어들을 출력한다.

## 예제 입력 1

```
13
but
i
wont
hesitate
no
more
no
more
it
cannot
wait
im
yours
```

## 예제 출력 1

```
i
im
it
no
but
more
wait
wont
yours
cannot
hesitate
```

---

## ☺︎ a/t

정렬해야 할 우선순위

1. 길이가 짧은 것 2. 길이가 같으면, 사전순으로 ASC 정렬

고려해야할 것 : 길이가 같을 때, 사전순으로 알파벳을 판별

## ☺︎ Snippets

> 1차 시도
>

```java
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());

        String[] arr = new String[n];

        for (int i = 0; i < n; i++) {
            String s = br.readLine();
            arr[i] = s;
        }
        //sort by length
        for (int i = 0; i < arr.length; i++) {
            for (int j = i + 1; j < arr.length; j++) {
                String temp;
                String string1 = arr[i];
                String string2 = arr[j];
                int stringLen1 = string1.length();
                int stringLen2 = string2.length();

                //문자열 길이 버블 정렬
                String tempString;
                if (stringLen1 > stringLen2) {
                    tempString = arr[i];
                    arr[i] = arr[j];
                    arr[j] = tempString;
                }
            }
        }

        StringBuilder sb = new StringBuilder();
        for (int i = 1; i < arr.length; i++) {
            if (!arr[i - 1].equals(arr[i])) {
                sb.append(arr[i]).append("\n");
            }
        }
        System.out.println(sb.toString().trim());
    }
}

```

버블 정렬로 길이 순으로 정렬을 했다.

### 사전 순 정렬 실패 이유

이후 동일한 길이를 ASC 순으로 정렬을 해야했는데 이 부분에서 구현이 되지 않았다.

```java
  for (int j = 0; j < list.size() - 1; j++) {

            //길이가 같은 범위 설정 In list
            int len = list.get(j).length();
            int first = j;
            if (len == list.get(j + 1).length()) {
                while (len == list.get(j + 1).length()) {
                    j++;
                }
                if (len < list.get(j + 1).length()) {

                }

            }
            sort(list.subList(first, j + 1));
        }
```
### sublist

sublist를 잘못사용했다. sublists는 원본 리스트에 대한 뷰일 뿐이므로, 원본 리스트에 실제로 반영되지 않는다.

그리고, 길이가 같은 단어를 찾는 과정에서 While문을 사용했는데, 이 때 j의 값이 리스트 크기를 넘어서면서 IndexOutOfBoundsException이 발생했다.

> 2차 시도
>

```java

public class Main2 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(br.readLine());
        String[] arr = new String[n];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = br.readLine();
        }
        Arrays.sort(arr, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {

                //단어 길이가 같은 경우
                if (o1.length() == o2.length()) {
                    return o1.compareTo(o2); //사전 순 정렬
                    //그 외
                } else {
                    return o1.length() - o2.length();
                }
            }
        });
        System.out.println(arr[0]);
        //중복 아닌 단어 출력
        for (int i = 1; i < arr.length; i++) {
            if (!arr[i].equals(arr[i - 1])) {
                System.out.println(arr[i]);
            }
        }
    }
}

```

CompareTo 메소드를 사용해서 풀어야 했다.

---

# ☻ 설탕 배달

2024년 3월 7일 2024년 3월 8일

## 문제

상근이는 요즘 설탕공장에서 설탕을 배달하고 있다. 상근이는 지금 사탕가게에 설탕을 정확하게 N킬로그램을 배달해야 한다. 설탕공장에서 만드는 설탕은 봉지에 담겨져 있다. 봉지는 3킬로그램 봉지와 5킬로그램 봉지가 있다.

상근이는 귀찮기 때문에, 최대한 적은 봉지를 들고 가려고 한다. 예를 들어, 18킬로그램 설탕을 배달해야 할 때, 3킬로그램 봉지 6개를 가져가도 되지만, 5킬로그램 3개와 3킬로그램 1개를 배달하면, 더 적은 개수의 봉지를 배달할 수 있다.

상근이가 설탕을 정확하게 N킬로그램 배달해야 할 때, 봉지 몇 개를 가져가면 되는지 그 수를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 N이 주어진다. (3 ≤ N ≤ 5000)

## 출력

상근이가 배달하는 봉지의 최소 개수를 출력한다. 만약, 정확하게 N킬로그램을 만들 수 없다면 -1을 출력한다.

## 예제 입력 1

```
18
```

## 예제 출력 1

```
4
```

## 예제 입력 2

```
4
```

## 예제 출력 2

```
-1
```

## 예제 입력 3

```
6

```

## 예제 출력 3

```
2

```

## 예제 입력 4

```
9

```

## 예제 출력 4

```
3

```

## 예제 입력 5

```
11
```

## 예제 출력 5

```
3
```

---

## ☺︎ at

최적의 선택을 하는 그리디 알고리즘. **목표는 가장 적은 봉지수로 배달하는 것이다.** 가장 적은 봉지수로 배달하기 위해선 5kg 봉지를 최대한 많이 사용해야 한다.

---

### 고려해야 할 것

1. 5kg만 사용해야 할 경우
2. 3kg만 사용해야 할 경우
  1. 예외로 3의 배수인 18kg를 옮길 때, 6의 배수이기에 5kg를 3개 사용하고 3kg를 1개 사용하여→ 최적의 해를 도출하도록 해주어야 한다.
3. 5kg와 3kg의 조합일 경우
4. 배달할 수 없는 경우 -1을 리턴

---

### 정리하면

우선적으로 5kg를 많이 사용하도록 해준다. 그리고 5kg 봉지만 사용해서 나눠 떨어지지 않는 경우 3kg 봉지를 사용해야 한다. 이 때 5kg 봉지를 **하나씩 줄이면서 3kg 봉지를 사용**할 수 있는지 확인해보자. 이 모든 경우를 확인했음에도 정확히 N kg를 만들 수 없다면 그 때 -1을 반환하자.

---

## ☺︎ Snippets

> 1차 시도
>

```java
public class Main2 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int result = 0;
        int temp = 0;
        int cnt3 = 0;
        int cnt5 = 0;

        // 5kg를 하나씩 뺀다
        do {
            n = n - 5;
            System.out.println("N1: " + n);
            //n이 3으로 나눠 떨어지면 n/3 cal
            // @9일 때, 5를 빼버리니 5kg를 사용하지 않고 3kg만 사용해야 하는 경우 체크하지 못 함
            if (n % 3 == 0) {

                cnt3 = n / 3;
                System.out.println("N2: " + n);
                //8-5 3이 되어 5kg를 사용했는데, 3kg를 사용해야 할 때 cnt5를 거치지 않으므로 1 추가
                cnt5 += 1;
                break;
            }
            //n이 음수가 될 경우
            if (n < 0) {
                System.out.println("N3: " + n);
                n = n + 5;
                break;
            }
            cnt5++;
        } while (n > 0);
        System.out.println("결과: " + cnt3 + "//" + cnt5);
    }
}
```

> 2차 시도
>

```java
public class Main3 {
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int cnt5 = 0;
        int cnt3 = 0;

        cnt5 = n / 5;

        int res = n % 5;

        while (res >= 0) {
            if (res % 3 == 0) {
                cnt3 = res / 3;
                break;
            }
            if (res == n) {
                System.out.println("-1");
                return;
            }
            cnt5--;
            res += 5;
        }
        System.out.println(cnt3 + cnt5);
    }
}
```

코드를 수정하고 통과했다.

1. 우선 5kg를 가장 많이 사용하는 것이 목표이다. 따라서 5kg를 사용할 수 있는 만큼 사용하기 위해 5로 나눠준다.
2. 그리고 나머지 ≥0 될 때 까지 (5kg로 운반할 수 없다면 3kg를 사용해야 하니까) **While**문 을 수행한다.

- While문 내의 로직 구현이 중요하다.

  1. 우선 5로 몫을 나눴으니, 3kg 봉지 추가 사용 여부는 if(res % 3 ==0)을 통해 확인한다.
     - 나눠 떨어진다면 5kg를 더 이상 사용할 수 없고, 3kg를 사용해서 전체를 운반할 수 있다는 말이므로 나머지를 3kg로 나눠주고 결과를 출력한다.
  2. **그리고 나머지가 남아는 있는데, 3kg로 나눠 떨어지지 않는다면?**
     - 11kg를 운반할 때, 5kg를 두 개 사용한다는 가정하에 While문을 들어오지만, 사실 5Kg를 하나만 사용하고 3kg를 두 개 사용해서 운반할 수 있다. 따라서 5kg 봉지를 하나 줄이고, 이 줄인 5kg 봉지를 다시 나머지에 추가 해준다. **이 부분을 어떻게 구현해내야 하는지, 아이디어를 떠올리기 쉽지 않았다.**
  3. 그리고 4kg, 7kg인 경우?
     - 이는 5kg도, 3kg로도 운반할 수 없다. 어쨌든 5로 수행해본다. 3kg로도 나눠 떨어지지 않으므로, cnt5—와 res += 5를 거쳐 원복될 것이다. 이 때 운반할 수 없으므로 -1을 출력해준다.

---

# ☻ 벌집

2024년 3월 8일

## 문제

![https://www.acmicpc.net/JudgeOnline/upload/201009/3(2).png](https://www.acmicpc.net/JudgeOnline/upload/201009/3(2).png)

위의 그림과 같이 육각형으로 이루어진 벌집이 있다. 그림에서 보는 바와 같이 중앙의 방 1부터 시작해서 이웃하는 방에 돌아가면서 1씩 증가하는 번호를 주소로 매길 수 있다. 숫자 N이 주어졌을 때, 벌집의 중앙 1에서 N번 방까지 최소 개수의 방을 지나서 갈 때 몇 개의 방을 지나가는지(시작과 끝을 포함하여)를 계산하는 프로그램을 작성하시오. 예를 들면, 13까지는 3개, 58까지는 5개를 지난다.

## 입력

첫째 줄에 N(1 ≤ N ≤ 1,000,000,000)이 주어진다.

## 출력

입력으로 주어진 방까지 최소 개수의 방을 지나서 갈 때 몇 개의 방을 지나는지 출력한다.

## 예제 입력 1

```
13
```

## 예제 출력 1

```
3
```

---

## ☺︎ Snippets

```java
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();

        int cnt = 0;
        int num = 1;

        for (int i = 1; i <= n; i++) {
            if (n == 1) {
                System.out.println("1");
                return;
            }
            cnt += i;
            num = 6 * cnt;

            if (n <= num + 1) {
                System.out.println(i + 1);
                return;
            }
        }
    }
}
```

벌집 형태의 그래프에서 정해진 규칙에 따라 최소 거리를 찾는 문제이다. 벌집의 중앙에서 특정 방까지 가는데 거쳐야 하는 방의 최소 개수를 찾아야 하는데, 바로 등차수열 개념 사용해서 해결할 수 있었다.

### 시간 소요한 부분

```java
cnt += i;
num = 6 * cnt;
```

등차수열을 처음에 떠올렸긴 한데, 이 1 3 6 10으로 증가하는 패턴을 구현해내는 데 생각보다 시간이 많이 걸렸다.

처음에 while 문을 사용했었는데, for 문으로 수정했다. for에서 i는 벌집 중앙에서 → 목표 셀까지 도달하기 위해 필요한 단계 수를 나타낸다.

6의 배수만큼 층이 증가한다고 했다. 1층엔 1 ~7 개의 셀이 있을거고 1번만 이동하면 된다. 2층엔 8 ~ 19 셀이 있을 것이다. 이를 방번호로 체크해주면서, 각 층마다 누적된 셀의 개수를 판별할 수 있다. ( ≤ 이하라면) 으로 판별했다.

`cnt += i; num = 6 * cnt;`

---

# ☻ 나이 순 정렬

2024년 3월 9일

## 문제

온라인 저지에 가입한 사람들의 나이와 이름이 가입한 순서대로 주어진다. 이때, 회원들을 나이가 증가하는 순으로, 나이가 같으면 먼저 가입한 사람이 앞에 오는 순서로 정렬하는 프로그램을 작성하시오.

## 입력

첫째 줄에 온라인 저지 회원의 수 N이 주어진다. (1 ≤ N ≤ 100,000)

둘째 줄부터 N개의 줄에는 각 회원의 나이와 이름이 공백으로 구분되어 주어진다. 나이는 1보다 크거나 같으며, 200보다 작거나 같은 정수이고, 이름은 알파벳 대소문자로 이루어져 있고, 길이가 100보다 작거나 같은 문자열이다. 입력은 가입한 순서로 주어진다.

## 출력

첫째 줄부터 총 N개의 줄에 걸쳐 온라인 저지 회원을 나이 순, 나이가 같으면 가입한 순으로 한 줄에 한 명씩 나이와 이름을 공백으로 구분해 출력한다.

## 예제 입력 1

```
3
21 Junkyu
21 Dohyun
20 Sunyoung

```

## 예제 출력 1

```
20 Sunyoung
21 Junkyu
21 Dohyun
```

---

## ☺︎ a/t

### 2차원 배열을 정렬하는 방법

1. 배열의 각 요소를 순회하기 위해 반복문을 사용한다.
2. 현재 요소와 다음 요소를 비교한다. 이 비교는 첫 번째 요소를 기준으로 수행되며, 첫 번째 요소가 같은 경우 두 번째 요소를 기준으로 비교하게 된다.
3. 현재 요소가 다음 요소보다 큰 경우, 두 요소를 교환한다. 이를 스왑이라하며, 일시적인 변수를 사용하여 두 요소의 위치를 교환한다.
4. 모든 요소를 순회한 후, 배열은 첫 번째 요소를 기준으로 오름차순 정렬되며, 첫 번째 요소가 동일한 경우 두 번째 요소를 기준으로 오름차순 정렬된 상태가 된다.

이 과정을 반복하면서 배열의 모든 요소를 순회하고, 필요한 경우 요소의 위치를 교환하면 2차원 배열을 정렬할 수 있다.

---

## ☺︎ Snippets

> 1차
>

```java
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        String[][] arr = new String[n][2];

        int rows = arr.length;
        int cols = arr[0].length;
        //배열 저장
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                arr[i][j] = String.valueOf(sc.next());
            }
        }
        /**
         * @Note 2차원 배열 ArrayList에 저장
         */
        ArrayList<ArrayList<String>> arrayList = new ArrayList<>();
        for (String[] row : arr) {
            //각 행을 나타내는 ArrayList<String>을 데이터 추가
            ArrayList<String> rowData = new ArrayList<>(Arrays.asList(row));
            arrayList.add(rowData);
        }
        // 추가: 가입일 Idx
        for (int i = 0; i < arrayList.size(); i++) {
            arrayList.get(i).add(String.valueOf(i));
        }

        // 정렬 나이순, 나이 같으면 가입순
        // 나이가 같다면, idx를 보고 판단
        for (int i = 0; i < arrayList.size(); i++) {
            for (int j = i + 1; j < arrayList.size(); j++) {
                int age1 = Integer.parseInt(arrayList.get(i).get(0));
                int age2 = Integer.parseInt(arrayList.get(j).get(0));
                int idx1 = Integer.parseInt(arrayList.get(i).get(2));
                int idx2 = Integer.parseInt(arrayList.get(j).get(2));

//                System.out.println("get(i): " + arrayList.get(i));
//                System.out.println("get(2): " + arrayList.get(i).get(2));

                //나이 순 오름차순 정렬
                if ((age1 == age2 && idx1 > idx2) || age1 > age2) {
                    //Swap
//                    ArrayList<String> temp = new ArrayList<>(arrayList.get(i));
//                    arrayList.set(i, new ArrayList<>(arrayList.get(j)));
//                    arrayList.set(j, new ArrayList<>(temp));
                    Collections.swap(arrayList, i, j);
                }
            }
        }

        for (ArrayList<String> row : arrayList) {
            row.remove(2);
        }
        StringBuilder sb = new StringBuilder();
        for (int p = 0; p < arrayList.size(); p++) {
            for (int q = 0; q < arrayList.get(p).size(); q++) {
                sb.append(arrayList.get(p).get(q)).append(" ");
            }

            sb.append("\n");
        }
        System.out.print(sb.toString().trim());
    }
}

```

시간초과다. 코드의 병목 현상은 아마 나이, 가입일 순으로 정렬하는 과정에서 발생했을 것이다.

### 시간 복잡도 다시 생각하기

버블 정렬 형태로 작동한다. 버블 정렬의 시간 복잡도는 일반적으로 O(n^2)이다. 이때 'n'은 정렬해야 하는 요소의 수, 즉 사람 수를 의미한다.

- 각 회원을 모든 회원들과 비교한다. 이중 for loop를 통해 구현되며 이로 인해 O(n^2)의 시간 복잡도가 발생한다.
- 또한, swap 연산을 사용하여 회원의 위치를 바꾼다. 이는 각 회원을 다른 모든 회원과 비교할 때마다 발생하므로 O(n^2)의 시간 복잡도를 가진다.

따라서 이 알고리즘의 전체 시간 복잡도는 O(n^2)이다.

그럼, 문제에선 최대 100,000명까지 회원이 있을 수 있으므로, n^2의 연산이 수행된다. 따라서 회원 수가 100,000인 경우, 최대 10,000,000,000번의 연산이 수행된다.

### 어떻게 줄여야 하나

시간 복잡도 면에서 비효율적인 버블정렬 말고 다른 정렬 방식을 사용한다.

```java
public class Main {
    public static void main(String[] args) {
    
arrayList.sort(new Comparator<ArrayList<String>>() {
            @Override
            public int compare(ArrayList<String> o1, ArrayList<String> o2) {
                int a1 = Integer.parseInt(o1.get(0));
                int a2 = Integer.parseInt(o2.get(0));
                return Integer.compare(a1, a2);
            }
        });
        arrayList.sort(new Comparator<ArrayList<String>>() {
            @Override
            public int compare(ArrayList<String> o1, ArrayList<String> o2) {
                int age1 = Integer.parseInt(o1.get(0));
                int age2 = Integer.parseInt(o2.get(0));
                int idx1 = Integer.parseInt(o1.get(2));
                int idx2 = Integer.parseInt(o2.get(2));
                if (age1 == age2 && idx1 > idx2) {
                    return Integer.compare(idx1, idx2);
                }
                return 0;

            }
        });
        
        }
      }
```

Arrays.sort() 메소드를 사용한다. 분할 정복을 사용해 데이터를 더 작은 단위로 분할하고, 각각을 정렬한 후 다시 병합하는 방식으로 작동한다. 버블정렬보다 빠르게 O(n log n) 시간 복잡도를 갖는다.

따라서, 같은 양의 데이터를 정렬할 때 `Arrays.sort()` 메소드는 버블 정렬보다 훨씬 빠르게 동작한다.

---

# ☻ 큐

2024년 3월 9일

## 문제

정수를 저장하는 큐를 구현한 다음, 입력으로 주어지는 명령을 처리하는 프로그램을 작성하시오.

명령은 총 여섯 가지이다.

- push X: 정수 X를 큐에 넣는 연산이다.
- pop: 큐에서 가장 앞에 있는 정수를 빼고, 그 수를 출력한다. 만약 큐에 들어있는 정수가 없는 경우에는 -1을 출력한다.
- size: 큐에 들어있는 정수의 개수를 출력한다.
- empty: 큐가 비어있으면 1, 아니면 0을 출력한다.
- front: 큐의 가장 앞에 있는 정수를 출력한다. 만약 큐에 들어있는 정수가 없는 경우에는 -1을 출력한다.
- back: 큐의 가장 뒤에 있는 정수를 출력한다. 만약 큐에 들어있는 정수가 없는 경우에는 -1을 출력한다.

## 입력

첫째 줄에 주어지는 명령의 수 N (1 ≤ N ≤ 10,000)이 주어진다. 둘째 줄부터 N개의 줄에는 명령이 하나씩 주어진다. 주어지는 정수는 1보다 크거나 같고, 100,000보다 작거나 같다. 문제에 나와있지 않은 명령이 주어지는 경우는 없다.

## 출력

출력해야하는 명령이 주어질 때마다, 한 줄에 하나씩 출력한다.

## 예제 입력 1

```
15
push 1
push 2
front
back
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
front
```

## 예제 출력 1

```
1
2
2
0
1
2
-1
0
1
-1
0
3
```

---

## ☺︎ Snippets

```java
public class Main {
    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();
        Queue<Integer> q = new LinkedList<>();

        int n = Integer.parseInt(br.readLine());

        for (int i = 0; i < n; i++) {
            String[] arr = new String[2];
            StringTokenizer stk = new StringTokenizer(br.readLine(), " ");
            int st = 0;
            while (stk.hasMoreTokens()) {
                arr[st] = stk.nextToken();
                st++;
            }
            if (arr[0].equals("push")) {
                int ele = Integer.parseInt(arr[1]);
                q.offer(ele);
            } else if (arr[0].equals("front")) {
                if (q.isEmpty()) {
                    System.out.println("-1");
                } else {
                    int peeked = q.peek();
                    System.out.println(peeked);
                }
                //큐는 맨 뒤 원소 반납하는 메소드 없음
            } else if (arr[0].equals("back")) {
                if (q.isEmpty()) {
                    System.out.println("-1");
                } else {
                    back(q);
                }
            } else if (arr[0].equals("empty")) {
                if (q.isEmpty()) {
                    System.out.println("1");
                } else {
                    System.out.println("0");
                }
            } else if (arr[0].equals("pop")) {
                if (!q.isEmpty()) {
                    int peeked = q.peek();
                    System.out.println(peeked);
                    q.poll();
                } else {
                    System.out.println("-1");
                }
            } else if (arr[0].equals("size")) {
                int size = q.size();
                System.out.println(size);
            }
        }
    }

    //맨 뒤 원소 반환
    private static void back(Queue<Integer> q) {
        StringBuilder sb = new StringBuilder();
        int len = q.size();
        int last = 0;
        for (int i = 1; i <= len; i++) {
            last = q.poll();
            q.offer(last);
            //i가 2일 때 마지막 원소가 최상단에 옴
            //q 사이즈는 2
        }
        System.out.println(last);
    }
}
```

큐 자료구조를 공부할 수 있었던 기본 문제다.

---

### 큐와 스택 다시 정리

큐(Queue)와 스택(Stack)은 둘 다 컬렉션의 한 유형이며, 데이터를 관리하는 방식에 따라 차이가 있다.

### 스택(Stack)

데이터의 **추가와 제거가 동일한 쪽에서 이루어지는 후입선출(LIFO**, Last In First Out) 방식의 자료구조이다. 즉, 가장 마지막에 들어간 항목이 가장 먼저 나오게 된다. 스택에 데이터를 추가하는 것을 '푸시(push)'라고 하며, 데이터를 제거하는 것을 '팝(pop)'이라고 한다.

### 큐(Queue)

데이터의 추가가 **한 쪽에서 이루어지고 제거는 반대 쪽에서 이루어지는 선입선출(FIFO,** First In First Out) 방식의 자료구조이다. 즉, 가장 먼저 들어간 항목이 가장 먼저 나오게 된다. 큐에 데이터를 추가하는 것을 '인큐(enqueue)'라고 하며, 데이터를 제거하는 것을 '디큐(dequeue)'라고 한다.

### 어떤 상황에서 어떤 자료구조를 사용해야 하는가?

> 큐(Queue)
>
1. 여러 사용자가 공유하는 자원을 관리할 때. 예를 들어 CPU 스케줄링, 디스크 스케줄링 등
2. 데이터가 비동기적으로 (송신 속도와 수신 속도가 반드시 일치하지 않음) 두 프로세스 사이에서 전송될 때. 예를 들어 IO 버퍼, 파이프, 파일 IO 등

> 스택(Stack)
>
1. **마지막에 입력된 데이터가 먼저 처리**되어야 하는 상황. 예를 들어, 웹 브라우저의 '뒤로 가기' 버튼은 이전에 방문한 모든 URL을 스택에 저장한다.
2. 새 페이지를 방문할 때마다 해당 페이지의 URL이 스택의 맨 위에 추가되고, '뒤로 가기' 버튼을 누르면 현재 URL이 스택에서 제거된다.