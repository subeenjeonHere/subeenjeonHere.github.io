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
| 기본 문제✔ | 1475  |      [방 번호]()      |
| 기본 문제✔ | 3273  |     [두 수의 합]()     |
| 기본 문제  | 10807 |     [개수 세기]()      |
| 기본 문제  | 13300 |      [방 배정]()      |
| 기본 문제  | 11328 |     [Strfry]()     |
| 기본 문제  | 1919  |    [애너그램 만들기]()    |

---

# **☻ 알파벳 개수**

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

```jsx
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

```java
int[] cnt = new int[10];
```

수정하였고, 통과 완료.

---