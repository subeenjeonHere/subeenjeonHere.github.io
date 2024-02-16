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

| 문제 분류  |  문제   |                      문제 제목                       |
|:------:|:-----:|:------------------------------------------------:|
| 연습 문제  | 10808 | [알파벳 개수](https://www.acmicpc.net/problem/10808)  |
| 기본 문제✔ | 2577  |  [숫자의 개수](https://www.acmicpc.net/problem/2577)  |
| 기본 문제✔ | 1475  |   [방 번호](https://www.acmicpc.net/problem/1475)   |
| 기본 문제✔ | 3273  |  [두 수의 합](https://www.acmicpc.net/problem/3273)  |
| 기본 문제  | 10807 |  [개수 세기](https://www.acmicpc.net/problem/10807)  |
| 기본 문제  | 13300 |  [방 배정](https://www.acmicpc.net/problem/13300)   |
| 기본 문제  | 11328 | [Strfry](https://www.acmicpc.net/problem/11328)  |
| 기본 문제  | 1919  | [애너그램 만들기](https://www.acmicpc.net/problem/1919) |

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
