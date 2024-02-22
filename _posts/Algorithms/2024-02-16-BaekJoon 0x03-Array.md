---
title: BaekJoon 0x03 Array
date: 2024-02-16
categories: Algorithms
tags:
  [
    Algorithms
      Baekjoon
  ]
---

# [BaekJoon](https://www.acmicpc.net/)

| 문제 분류  |  문제   |       문제 제목        |
|:------:|:-----:|:------------------:|
| 연습 문제  | 10808 | [알파벳 개수](#-알파벳-개수) |
| 기본 문제✔ | 2577  | [숫자의 개수](#-숫자의-개수) |
| 기본 문제✔ | 1475  |   [방 번호](#-방-번호)   |
| 기본 문제✔ | 3273  | [두 수의 합](#-두-수의-합) |
| 기본 문제  | 10807 |  [개수 세기](#-개수-세기)  |
| 기본 문제  | 13300 |   [방 배정](#-방-배정)   |
| 기본 문제  | 11328 |     [Strfry]()     |
| 기본 문제  | 1919  |    [애너그램 만들기]()    |

---

# ☻ 알파벳 개수

2024년 2월 16일

| 시간 제한 | 메모리 제한 | 제출    | 정답    | 맞힌 사람 | 정답 비율   |
|-------|--------|-------|-------|-------|---------|
| 1 초   | 256 MB | 49343 | 33524 | 26978 | 68.519% |

## 문제

알파벳 소문자로만 이루어진 단어 S가 주어진다. 각 알파벳이 단어에 몇 개가 포함되어 있는지 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 단어 S가 주어진다. 단어의 길이는 100을 넘지 않으며, 알파벳 소문자로만 이루어져 있다.

## 출력

단어에 포함되어 있는 a의 개수, b의 개수, …, z의 개수를 공백으로 구분해서 출력한다.

## 예제 입력 1

```
baekjoon
```

## 예제 출력 1

```
1 1 0 0 1 0 0 0 0 1 1 0 0 1 2 0 0 0 0 0 0 0 0 0 0 0
```

# ☺︎ a/t

1. 아스키코드 활용
2. String과 알파벳 소문자가 저장된 배열을 비교 → 값이 일치한다면 카운팅 배열 1씩 증가

# ☺︎ Snippets

> 1차
>

```java
public class Main {
    public static void main(String[] args) {
        int[] arr = new int[25];
        int idx = 0;
        StringBuilder sb = new StringBuilder();

        // a-z 저장할 배열
        for (int i = idx; i < arr.length; i++) {
            arr[idx] = (char) i;
            idx++;
        }

        // String 입력
        Scanner sc = new Scanner(System.in);
        String S = sc.next();
        String[] Strings = new String[S.length()];

        // arr 길이까지 반복, Str.charAt(i)와 arr[i]가 같은 경우 a-z의 개수를 카운팅 할 temp 배열을 1씩 카운팅
        int[] counting = new int[25];

        for (int j = 0; j < S.length(); j++) {
            System.out.println(S.charAt(j));
            //만약 String j번째와, 소문자 저장된 전체 배열 일치한다면 temp[j] ++
            for (int k = 0; k < arr.length; k++) {
                if (S.charAt(j) == arr[k]) {
                    counting[k]++;
                }
            }
        }
        int start = 0;
        while (start < arr.length) {
            sb.append(counting[start]).append(" ");
            start++;
        }
        System.out.println(sb);
    }
}
```

> 2차 성공
>

```java
import java.util.Scanner;
import java.util.logging.XMLFormatter;

public class Main {
    public static void main(String[] args) {
        char[] arr = new char[26];
        StringBuilder sb = new StringBuilder();

        // a-z 저장할 배열
        int a = 97;
        for (int i = 0; i < arr.length; i++) {
            arr[i] = (char) a;
            a++;
        }
        // String 입력
        Scanner sc = new Scanner(System.in);
        String S = sc.next();

        // arr 길이까지 반복, Str.charAt(i)와 arr[i]가 같은 경우 a-z의 개수를 카운팅 할 temp 배열을 1씩 카운팅
        int[] counting = new int[26];
        for (int i = 0; i < S.length(); i++) {
            for (int k = 0; k < arr.length; k++) {
                if (S.charAt(i) == arr[k]) {
                    counting[k]++;
                }
            }
        }

        int start = 0;
        while (start < arr.length) {
            sb.append(counting[start]).append(" ");
            start++;
        }
        System.out.println(sb);
    }
}
```

---

# ☻ 숫자의 개수

2024년 2월 16일

| 시간 제한 | 메모리 제한 | 제출     | 정답     | 맞힌 사람 | 정답 비율   |
|-------|--------|--------|--------|-------|---------|
| 1 초   | 128 MB | 190013 | 114185 | 93243 | 59.709% |

## 문제

세 개의 자연수 A, B, C가 주어질 때 A × B × C를 계산한 결과에 0부터 9까지 각각의 숫자가 몇 번씩 쓰였는지를 구하는 프로그램을 작성하시오.

예를 들어 A = 150, B = 266, C = 427 이라면 A × B × C = 150 × 266 × 427 = 17037300 이 되고, 계산한 결과 17037300 에는 0이 3번, 1이 1번, 3이 2번,
7이 2번 쓰였다.

## 입력

첫째 줄에 A, 둘째 줄에 B, 셋째 줄에 C가 주어진다. A, B, C는 모두 100보다 크거나 같고, 1,000보다 작은 자연수이다.

## 출력

첫째 줄에는 A × B × C의 결과에 0 이 몇 번 쓰였는지 출력한다. 마찬가지로 둘째 줄부터 열 번째 줄까지 A × B × C의 결과에 1부터 9까지의 숫자가 각각 몇 번 쓰였는지 차례로 한 줄에 하나씩
출력한다.

## 예제 입력 1

```
150
266
427
```

## 예제 출력 1

```
3
1
0
2
0
0
0
2
0
0
```

# ☺︎ a/t

1. BufferdReader에 개행 입력받고, StringTokenizer에 저장, 값 계산
2. 결과값 token을 공백없이 자름 → int[] 배열에 저장
3. while int 배열 끝까지 반복
    1. for문 0~9 아스키코드 순회
        1. arr[i] == 아스키코드 같다면 카운팅, 아스키 `0~9는 48~57`

# ☺︎ Snippets

> 1차

```java
public class Main {
    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int a = Integer.parseInt(br.readLine());
        int b = Integer.parseInt(br.readLine());
        int c = Integer.parseInt(br.readLine());
        int[] cnt = new int[9];

        String str = String.valueOf(a * b * c);
        char[] charArray = str.toCharArray();

        // 0~9 ASCII= 48~57
        char asc = 48;
        int value;
        while (asc <= 57) {
            for (int i = 0; i < charArray.length; i++) {
                if (charArray[i] == asc) {
                    value = Integer.parseInt(String.valueOf(asc));
                    cnt[value]++;
                }
            }
            asc++;
        }

        for (int ele : cnt) {
            System.out.println(ele);
        }
    }
}
```

```
workspace / back_joon / out / production / ct
back_joon.x03배열.숫자의개수.Main
150
266
427
3
1
0
2
0
0
0
2
0
```

값은 맞는데 틀렸다고 나온다.

### **멍청한 실수**

0~9까지를 카운팅하기 위한 배열의 길이는 10이 되어야 한다.

```
int[] cnt = new int[10];
```

수정하였고, 통과 완료.

---

# ☻ 방 번호

2024년 2월 16일 2024년 2월 21일

| 시간 제한 | 메모리 제한 | 제출    | 정답    | 맞힌 사람 | 정답 비율   |
|-------|--------|-------|-------|-------|---------|
| 2 초   | 128 MB | 65801 | 29916 | 22385 | 44.096% |

## 문제

다솜이는 은진이의 옆집에 새로 이사왔다. 다솜이는 자기 방 번호를 예쁜 플라스틱 숫자로 문에 붙이려고 한다.

다솜이의 옆집에서는 플라스틱 숫자를 한 세트로 판다. 한 세트에는 0번부터 9번까지 숫자가 하나씩 들어있다. 다솜이의 방 번호가 주어졌을 때, 필요한 세트의 개수의 최솟값을 출력하시오. (6은 9를 뒤집어서 이용할
수 있고, 9는 6을 뒤집어서 이용할 수 있다.)

## 입력

첫째 줄에 다솜이의 방 번호 N이 주어진다. N은 1,000,000보다 작거나 같은 자연수이다.

## 출력

첫째 줄에 필요한 세트의 개수를 출력한다.

## 예제 입력 1

```
9999
```

## 예제 출력 1

```
2
```


# ☺︎ a/t

1. 6,9 제외 같은 값이 2개 이상이면 최소 2개 이상 필요
    1. 같은 값이 두 개 있다면 1*2 같은 값이 세 개 있다면 1*3
    2. 즉, 6과 9를 제외한 동일한 숫자 개수를 x라고 가정했을 때, 1*x 만큼의 세트가 필요
2. imp;
    1. 6과 9를 동일한 숫자로 처리할 방법
    2. 세트를 카운팅 할 방법
3. **이렇게 써보자**
    1. char 배열에 0~9까지 저장
        1. 6, 9는 같은 값으로 어떻게 취급?
            1. 만약, 6 or 9 일경우
            2. 전체 카운팅한 후 6 and 9를 찾아서 전체 개수를 더하고 / 2
    2. 이중 for문 순회하며 char 배열 비교
        1. result의 기본값은 1, char에서 같은 값 (==) 있을 경우, ++ (1씩 증가)
    3. 6,9를 어떻게 처리할 것인가
        - 6 9 9 일 경우
            - 2세트 필요
        - 6 9 9 9
            - 2세트
        - 66669
            - 3세트
        - 6, 9는 같은 값이므로 → 2로 나누고, 올림해줌

---

# ☺︎ Snippets

> 1차
>

```java
public class Main {
    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        char val = (int) 48;
        int n = Integer.parseInt(String.valueOf(val));

        int[] arr = new int[10];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = n;
            n
                    ++;
        }
        int num = Integer.parseInt(br.readLine());
        char[] chars = String.valueOf(num).toCharArray();

        int result = 1;
        for (int i = 0; i <= chars.length - 1; i++) {
            for (int j = i + 1; j < chars.length; j++) {
                if (chars[i] == chars[j]) {
                    result++;
                }
            }
        }

        int idx = 0;
        int count = 0;
        while
        (idx < chars.length) {
            if (chars[idx] == '9' && chars[idx] == '6') {

                count++;
            }
            idx++;
        }

        if (count >= 1) {
            System.out.println(result / count + 1);
        } else {
            System.out.println(result);
        }
    }
}
```

> 2차
>

```java
public class Main2 {
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        String size = sc.nextLine();
        int[] arr = new int[size.length()];

        for (int i = 0; i < arr.length; i++) {
            arr[i] = Integer.parseInt(String.valueOf(size.charAt(i)));
        }
        int[] count = new int[10];
        int maxval = 0;
        int sum = 0;

        //카운트 배열에 카운팅
        for (int i = 0; i < arr.length; i++) {

            //6,9를 제외하고 카운팅
            if (arr[i] != 6 && arr[i] != 9) {
                count[arr[i]]++;

                //6,9의 개수 세기 -> 6이거나 9라면 sum 1씩 증가
            } else if (arr[i] == 6 || arr[i] == 9) {
                sum++;
            }
            //Get max from count array
            maxval = Arrays.stream(count).max().getAsInt();
        }

        int result = 0;
        if (sum % 2 == 0) {
            result = maxval + sum / 2;
        } else if (sum % 2 == 1) {
            result = maxval + sum / 2 + 1;

        }
        System.out.println(result);
    }
}
```

### ☺︎ a/t

(6,9)를 하나의 순서쌍이라고 생각하고

1. %2 == 0일 경우엔 짝수이므로 한 세트만 있어도 된다.
2. %2 == 1일 경우엔 6과 9의 개수가 홀수이므로, 하나는 무조건 더 필요하다. 따라서 **sum/2 +1**개 필요하다.
3. 다만 다른 테스트 케이스는 통과하나 `12635` 에서 실패한다.
    1. 왜냐하면, **maxval**이 1이고, 6이 1개이므로 sum도 1 따라서 1/2+1 1이 되어 2개가 출력된다.

```
   
    if (sum == 1) {
                result = maxval;
            }
```

4. 그럼 이 조건을 추가해주니까 답이 나온다.
    1. 코드가 갈수록 난해해지고 있다. 분명이 더 간결하게 풀 수 있을 것 같은데, 우선 구현에 목표를 두고 성능은 성공 후에 생각해본다.

### 반례

1. maxval + sum 한 것이 문제
    1. 122266669
        1. 총 3세트만 있어도 되는데, 6세트가 나옴

> 3차
>

```java
public class Main3 {
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        String size = sc.nextLine();
        int[] arr = new int[size.length()];
        int[] count = new int[10];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = Integer.parseInt(String.valueOf(size.charAt(i)));
            count[arr[i]]++;
        }

        //최댓값
        int maxval = 0;
        int sum = 0;

        for (int k = 0; k < count.length; k++) {
            if (k != 6 && k != 9) {
                maxval = Math.max(count[k], maxval);
            }
        }
        for (int j = 0; j < arr.length; j++) {
            if (arr[j] == 6 || arr[j] == 9) {
                sum++;
            }
        }

        int result = 0;
        // 6 9 개수 판단
        if (sum % 2 == 0) {
            result = sum / 2;
        } else {
            result = sum / 2 + 1;
        }
        int answer = Math.max(result, maxval);
        System.out.println(answer);
    }
}
```

1. Count 배열에 6, 9를 제외한 개수 카운팅
2. 6과 9는 한 쌍을 1개로 취급하기 때문에 sum에 6, 9의 개수만큼 저장
    1. 6, 9가 홀수라면 sum / 2 + 1 세트 필요
    2. 짝수라면 sum / 2세트 필요
3. 여기서 0 ~ 9는 한 세트이기 때문에, Math.max를 사용해 최댓값을 구해야 함
    1. 만약 **122266669라면 → 총 3세트, 처음에 max + sum을 더해서 6세트가 출력되어 틀렸었다.**

---

# ☻ 두 수의 합

2024년 2월 21일

| 시간 제한 | 메모리 제한 | 제출    | 정답    | 맞힌 사람 | 정답 비율   |
|-------|--------|-------|-------|-------|---------|
| 1 초   | 128 MB | 52233 | 18726 | 13859 | 34.714% |

## 문제

n개의 서로 다른 양의 정수 a1, a2, ..., an으로 이루어진 수열이 있다. ai의 값은 1보다 크거나 같고, 1000000보다 작거나 같은 자연수이다. 자연수 x가 주어졌을 때, ai + aj = x (
1 ≤ i < j ≤ n)을 만족하는 (ai, aj)쌍의 수를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 수열의 크기 n이 주어진다. 다음 줄에는 수열에 포함되는 수가 주어진다. 셋째 줄에는 x가 주어진다. (1 ≤ n ≤ 100000, 1 ≤ x ≤ 2000000)

## 출력

문제의 조건을 만족하는 쌍의 개수를 출력한다.

## 예제 입력 1

```
9
5 12 7 10 9 1 2 3 11
13
```

## 예제 출력 1

```
3
```

---

# ☺︎ Snippets

> 1차
>

```java
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        //배열 생성
        int[] arr = new int[n];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = sc.nextInt();
        }
        int x = sc.nextInt();
        int cnt = 0;
        // a + b = x인 순서쌍 구하기, a < b
        for (int i = 0; i < arr.length - 1; i++) {
            for (int j = i + 1; j < arr.length; j++) {
                if (arr[i] + arr[j] == x) {
                    cnt++;
                }
            }
        }
        System.out.println(cnt);
    }
}
```

시간초과

이중 For문으로 풀으라고 낸 문제가 아닌 것 같다. **투 포인터로** 풀어야 한다.

> 2차
>

```java
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        //배열 생성
        int[] arr = new int[n];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = sc.nextInt();
        }
        //X 입력받기
        int x = sc.nextInt();

        //ASC 정렬
        Arrays.sort(arr);

        int cnt = 0;
        int start = 0;
        int end = arr.length - 1;
        while (end > arr.length / 2) {
            if (arr[start] + arr[end] == x) {
                System.out.println("End:" + end);
                System.out.println(arr[start] + " " + arr[end]);
                cnt++;
                end--;
            } else if (arr[start] + arr[end] < x) {
                start++;
            } else if (arr[start] + arr[end] > x) {
                end--;
            }
        }

        System.out.println(cnt);
    }
}

```

`(1 ≤ i < j ≤ n)` 문제 조건에선 **i와 j가 같을 수 없다. 그리고 서로 다른 양의 정수다.**

### 반례

도저히 안 풀려서 왜 안되는지 생각해보다가 랜덤으로 수를 만들어서 넣어봤는데, 이게 반례인 듯 하다.

```
5
13 23 2 90 3
113
```

```
End:4
23 90

End:3
90 23
2
```

원래 답은 1이 나와야 하는데 2개다. 왜냐면 **중복이 되었기 때문.**

> 3차 성공

```java
public class Main {
    public static void main(String[] args) {
        //ASC 정렬
        Arrays.sort(arr);

        int cnt = 0;
        int start = 0;
        int end = arr.length - 1;
        while (start < end) {
            if (arr[start] + arr[end] == x) {
                cnt++;
                end--;
            } else if (arr[start] + arr[end] < x) {
                start++;
            } else if (arr[start] + arr[end] > x) {
                System.out.println("Start:" + start + "// End:" + end);
                end--;
            }
        }
        System.out.println(cnt);
    }
}
```

투 포인터 알고리즘을 사용할 때 시작점(start)과 끝점(end)의 위치 설정을 잘못했기 때문이다. 그리고 1,2차때 풀었던 것처럼 start와 end를 동일하게 주고 증가시키는 게 아니라, ASC 정렬하고
중간으로 좁혀나가야 한다

시간 904ms, BufferdReader 사용했으면 더 빨랐을 듯하다.

---


# ☻ 개수 세기

2024년 2월 22일

| 시간 제한 | 메모리 제한 | 제출 | 정답 | 맞힌 사람 | 정답 비율 |
| --- | --- | --- | --- | --- | --- |
| 1 초 | 256 MB | 111569 | 68962 | 59011 | 62.783% |

## 문제

총 N개의 정수가 주어졌을 때, 정수 v가 몇 개인지 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 정수의 개수 N(1 ≤ N ≤ 100)이 주어진다. 둘째 줄에는 정수가 공백으로 구분되어져있다. 셋째 줄에는 찾으려고 하는 정수 v가 주어진다. 입력으로 주어지는 정수와 v는 -100보다 크거나 같으며, 100보다 작거나 같다.

## 출력

첫째 줄에 입력으로 주어진 N개의 정수 중에 v가 몇 개인지 출력한다.

## 예제 입력 1

```
11
1 4 1 2 4 2 4 2 3 4 4
2
```

## 예제 출력 1

```
3

```

## 예제 입력 2

```
11
1 4 1 2 4 2 4 2 3 4 4
5
```

## 예제 출력 2

```
0
```

---

# ☺︎ a/t

```java
For input string: "1 4 1 2 4 2 4 2 3 4 4"
```

br.readLine()으로 입력받으려 할 때 발생한 에러인데, 원인은 **공백이 포함**되어 있기 때문에 숫자로 변환해야 하는 문자열이 실제로 숫자가 아니기 때문. → "1 4 1 2 4 2 4 2 3 4 4"를 공백으로 분리한 후 각각을 숫자로 변환해야 한다.

---

# ☺︎ Snippets

```java
public class Main {
    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        // n 입력
        int n = Integer.parseInt(br.readLine());

        //토큰 입력
        int[] arr = new int[n];
        String[] strings;
        strings = br.readLine().split(" ");
        for (int i = 0; i < arr.length; i++) {
            arr[i] = Integer.parseInt(strings[i]);
        }

        // v 입력받기
        int v = Integer.parseInt(br.readLine());
        // 0부터 배열 순회하며 v와 같다면 cnt 증가
        int cnt = 0;
        int start = 0;
        while (start < arr.length) {
            if (arr[start] == v) cnt++;
            start++;
        }
        System.out.println(cnt);
        br.close();
    }
}
```

스캐너로 풀었을 때보다 시간이 두 배 정도 단축되었다.

---

# ☻ **방 배정**

2024년 2월 22일

### **☻ 방 배정**

| 시간 제한 | 메모리 제한 | 제출 | 정답 | 맞힌 사람 | 정답 비율 |
| --- | --- | --- | --- | --- | --- |
| 2 초 | 512 MB | 18372 | 10515 | 8760 | 59.661% |

## 문제

정보 초등학교에서는 단체로 2박 3일 수학여행을 가기로 했다. 여러 학년이 같은 장소로 수학여행을 가려고 하는데 1학년부터 6학년까지 학생들이 묵을 방을 배정해야 한다. 남학생은 남학생끼리, 여학생은 여학생끼리 방을 배정해야 한다. 또한 한 방에는 같은 학년의 학생들을 배정해야 한다. 물론 한 방에 한 명만 배정하는 것도 가능하다.

한 방에 배정할 수 있는 최대 인원 수 K가 주어졌을 때, 조건에 맞게 모든 학생을 배정하기 위해 필요한 방의 최소 개수를 구하는 프로그램을 작성하시오.

예를 들어, 수학여행을 가는 학생이 다음과 같고 K = 2일 때 12개의 방이 필요하다. 왜냐하면 3학년 남학생을 배정하기 위해 방 두 개가 필요하고 4학년 여학생에는 방을 배정하지 않아도 되기 때문이다.

| 학년 | 여학생 | 남학생 |
| --- | --- | --- |
| 1학년 | 영희 | 동호, 동진 |
| 2학년 | 혜진, 상희 | 경수 |
| 3학년 | 경희 | 동수, 상철, 칠복 |
| 4학년 |  | 달호 |
| 5학년 | 정숙 | 호동, 건우 |
| 6학년 | 수지 | 동건 |

## 입력

표준 입력으로 다음 정보가 주어진다. 첫 번째 줄에는 수학여행에 참가하는 학생 수를 나타내는 정수 N(1 ≤ N ≤ 1,000)과 한 방에 배정할 수 있는 최대 인원 수 K(1 < K ≤ 1,000)가 공백으로 분리되어 주어진다. 다음 N 개의 각 줄에는 학생의 성별 S와 학년 Y(1 ≤ Y ≤ 6)가 공백으로 분리되어 주어진다. 성별 S는 0, 1중 하나로서 여학생인 경우에 0, 남학생인 경우에 1로 나타낸다.

## 출력

표준 출력으로 학생들을 모두 배정하기 위해 필요한 최소한의 방의 수를 출력한다.

## 서브태스크

| 번호 | 배점 | 제한 |
| --- | --- | --- |
| 1 | 2 | 입력 예시로 주어진 입력만 존재한다. |
| 2 | 10 | 1학년 남학생만 참가하는 것으로 가정한다. |
| 3 | 20 | 1학년만 참가하는 것으로 가정한다. |
| 4 | 68 | 원래의 제약조건 이외에 아무 제약조건이 없다. |

## 예제 입력 1

```
16 2
1 1
0 1
1 1
0 2
1 2
0 2
0 3
1 3
1 4
1 3
1 3
0 6
1 5
0 5
1 5
1 6
```

## 예제 출력 1

```
12
```

## 예제 입력 2

```
3 3
0 3
1 5
0 6
```

## 예제 출력 2

```
3
```

---

# ☺︎ a/t

1. Input을 입력받으면서 > 2 이상이면 Count ++; 하거나
2. 전체 값을 저장한 이후 %2

---

# Snippets

> 성공
>

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer stk = new StringTokenizer(br.readLine());

        int n = Integer.parseInt(stk.nextToken());
        int k = Integer.parseInt(stk.nextToken());

        // 배열 생성
        int[][] arr = new int[2][7];

        //Input 입력하고, 2차원 배열 s/y 개수 Counting
        int start = 1;

        while (start <= n) {
            StringTokenizer input = new StringTokenizer(br.readLine());
            int s = Integer.parseInt(input.nextToken());
            int y = Integer.parseInt(input.nextToken());
            arr[s][y]++;
            start++;
        }
        //방 개수 Count
        int room = 0;

        for (int i = 0; i < arr.length; i++) {
            System.out.println("I:" + i);
            for (int j = 1; j < arr[i].length; j++) {
                System.out.println("J:" + j);
                if (arr[i][j] % k == 0) {
                    room += arr[i][j] / k;
                } else if (arr[i][j] % k == 1) {
                    room += (arr[i][j] / k) + 1;
                }
            }
        }

//        //출력
        bw.write(room);
//        //남은 값 출력, 버퍼 초기화
//        bw.flush();
        //입력버퍼 close
        br.close();
        //출력버퍼 close
        bw.close();
    }
}
```

---
