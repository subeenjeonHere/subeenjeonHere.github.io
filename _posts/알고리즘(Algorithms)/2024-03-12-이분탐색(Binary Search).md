---
title: 이분 탐색(Binary Search)
date: 2024-03-12
categories: Algorithms
tags:
  [
    Algorithms
      BaekJoon
  ]
---

# ☻ 이분탐색 (Binary Search)

---

이분 탐색(Binary Search)은 정렬된 배열에서 특정한 원소를 찾아내는 알고리즘이다.

이 알고리즘은 찾으려는 원소와 중앙 원소를 비교하여 찾는 원소가 중앙 원소보다 작으면 왼쪽을, 크면 오른쪽을 탐색하는 방식을 반복한다. 이렇게 탐색 범위를 절반씩 줄여나가면서 원하는 원소를 찾아낸다.

---

예를 들어, 1부터 10까지의 정렬된 숫자 배열에서 숫자 7을 찾는다고 가정해보자.

| 배열 인덱스 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 배열 값 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |

이분 탐색 알고리즘을 시작할 때, 먼저 **중앙값인 5(인덱스 4)를 확인**한다. 찾고자 하는 숫자 7이 5보다 크므로, **6부터 10 사이(인덱스 5부터 9까지)에서 검색**을 계속한다.

| 배열 인덱스 | 5 | 6 | 7 | 8 | 9 |
| --- | --- | --- | --- | --- | --- |
| 배열 값 | 6 | 7 | 8 | 9 | 10 |

다음으로 **중앙값인 8(인덱스 7)를 확인한다**. 찾고자 하는 숫자 7이 8보다 작으므로, 6부터 7 사이(인덱스 5부터 6까지)에서 검색을 계속한다.

| 배열 인덱스 | 5 | 6 |
| --- | --- | --- |
| 배열 값 | 6 | 7 |

마지막으로, 중앙값인 7(인덱스 6)을 확인하면 찾고자 하는 숫자 7을 찾게 된다. 이렇게 매번 검색 범위가 절반으로 줄어든다.

---

## Java로 구현하면

이진 탐색 알고리즘은 반복 형태(iterative)와 재귀 형태(recursive) 구현할 수 있다.

### 왜

이진 탐색 알고리즘은 문제를 반복적으로 또는 재귀적으로 분할하여 해결하는 방식을 채택하기 때문이다.

### 반복 형태의 구현

**while** 루프를 사용하여 배열의 한 부분을 취하여 해당 부분에서 탐색을 계속 실시한다. 탐색 범위를 절반씩 줄여나가는 것이 이분 탐색의 핵심 원리이며, 이 원리는 반복문을 통해 구현할 수 있다.

### 재귀 형태의 구현

이분 탐색 함수를 자신 안에서 다시 호출하여 문제를 더 작은 부분 문제로 분할하고, 이를 해결하는 방식을 채택한다. 이 역시 이분 탐색의 원리를 따르며, 이를 재귀 호출을 통해 구현할 수 있다.

---

### 1. 반복 형태의 구현

```java
 // 이진 탐색의 Java 구현

import java.io.*;

class BinarySearch {

    // 만약 x가 arr[] 안에 있다면 x의 인덱스를 반환한다.
    int binarySearch(int arr[], int x)
    {
        int l = 0, r = arr.length - 1;
        while (l <= r) {
            int m = l + (r - l) / 2;

            // x가 중앙에 있는지 확인
            if (arr[m] == x)
                return m;

            // x가 더 크면, 왼쪽 반을 무시
            if (arr[m] < x)
                l = m + 1;

            // x가 더 작으면, 오른쪽 반을 무시
            else
                r = m - 1;
        }

        // 여기에 도달하면, 원소가 존재하지 않음
        return -1;
    }

    // 메인 코드
    public static void main(String args[])
    {
        BinarySearch ob = new BinarySearch();
        int arr[] = { 2, 3, 4, 10, 40 };
        int n = arr.length;
        int x = 10;
        int result = ob.binarySearch(arr, x);
        if (result == -1)
            System.out.println(
                "원소가 배열에 존재하지 않습니다");
        else
            System.out.println("원소는 "
                               + "인덱스 " + result + "에 존재합니다");
    }
}
```

---

## 2. 재귀 형태의 구현

```java
// 재귀적 이진 탐색의 Java 구현
class BinarySearch {

    // 만약 x가 arr[l..r]에 있다면 x의 인덱스를 반환, 그렇지 않으면 -1 반환
    int binarySearch(int arr[], int l, int r, int x)
    {
        if (r >= l) {
            int mid = l + (r - l) / 2;

            // 원소가 중앙에 존재하는 경우
            if (arr[mid] == x)
                return mid;

            // 원소가 mid보다 작으면, 왼쪽 하위 배열에만 존재할 수 있음
            if (arr[mid] > x)
                return binarySearch(arr, l, mid - 1, x);

            // 그렇지 않으면 원소는 오른쪽 하위 배열에만 존재할 수 있음
            return binarySearch(arr, mid + 1, r, x);
        }

        // 배열에 원소가 없는 경우 여기에 도달
        return -1;
    }

    // 메인 코드
    public static void main(String args[])
    {
        BinarySearch ob = new BinarySearch();
        int arr[] = { 2, 3, 4, 10, 40 };
        int n = arr.length;
        int x = 10;
        int result = ob.binarySearch(arr, 0, n - 1, x);
        if (result == -1)
            System.out.println(
                "원소가 배열에 존재하지 않습니다");
        else
            System.out.println(
                "원소는 인덱스 " + result + "에 존재합니다");
    }
}
```

---