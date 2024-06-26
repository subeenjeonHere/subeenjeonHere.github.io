---
title: 델타 4방 탐색 (Delta 4-way Search)
description: Delta 4-directional search refers to searching adjacent cells in four directions (up, down, left, and right) from a given position in a grid or matrix.
author: [ subeenjeon ]
date: 2024-03-12
categories: [ Algorithms ]
tags: [ Algorithms, 알고리즘 ]
pin: true
math: true
mermaid: true
toc: true
image:
#  path: /commons/devices-mockup.png
#  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#  alt: Responsive rendering of Chirpy theme on multiple devices.
---

# ◼︎ 델타 4방 탐색 (Delta 4-way Search)

2차원 배열에서 상하좌우 방향으로 이동하면서 데이터를 탐색하는 알고리즘

---

## Snippets

```java
public class Main {
    // 상하
    static int[] dx = {-1, 1, 0, 0};
    // 좌우
    static int[] dy = {0, 0, -1, 1};

    public static void main(String[] args) {
        int N = 4;
        for (int x = 0; x < 4; x++) {
            for (int y = 0; y < 4; y++) {
                if (map[x][y] == -1) {

                    // target이 -1이라면 4방 탐색을 수행함
                    for (int d = 0; d < 4; d++) {
                        //현재 위치에서 상하좌우로 이동한 좌표 계산
                        int nx = x + dx[d];
                        int ny = y + dy[d];

                        if (ny < 0 || ny >= N || nx < 0 || nx >= N) {
                            continue; // 범위 벗어나면 다음 방향 진행
                        }
                        System.out.println("탐색결과: " + map[nx][ny] + " ");
                        System.out.println();
                    }
                }
            }
        }
    }
}
```

---

## 이동할 x와 y좌표를 계산하는 부분

```java
int[] dx = {-1, 1, 0, 0};
int[] dy = {0, 0, -1, 1};
```

- dx는 좌우 이동
- dy는 상하이동

이 배열을 통해 4방향으로의 이동을 처리할 수 있다.

---

## 4방 탐색의 핵심

```java
static int[] dx = {0, 0, -1, 1}; // 좌, 우 이동
static int[] dy = {-1, 1, 0, 0}; // 상, 하 이동
```

이 코드는 델타 탐색의 핵심이며, 상, 하, 좌, 우로 이동하기 위해 현재 위치에 델타값(dx, dy)을 더하는 연산을 수행한다.

---

## Example

아래와 같은 2차원 배열이 있다고 가정한다.

```java
        int[][] map = [{1, 2, 3, 4},
                {5, -1, 7, 8},
                {9, 10, 11, 12},
                {13, 14, -1, 16}];
```

위의 dx, dy 배열을 사용하여 4방향으로 이동할 수 있다. dx, dy의 각 원소는 상, 하, 좌, 우 이동을 의미하며, 이는 각각 북, 남, 서, 동 방향을 나타낸다.

```java
// 상(북), 하(남), 좌(서), 우(동)
for(int d = 0; d < 4; d++) {
    int nx = x + dx[d];
    int ny = y + dy[d];
}
```

---

## (1,1)에서 상방향 이동

```java
x + dx[d] : 1 , -1
y + dy[d] : 1 , 0
nx: 0 / ny: 1
탐색결과: 2 
```

여기서 dx[0]과 dy[0]을 사용하면 상(북) 방향으로 이동하게 되며, 이는 y 좌표를 1 감소시키는 것을 의미한다.  따라서 (1,1) 위치에서 상(북) 방향으로 이동하면 **(0,1)** 위치가 된다. 이 때 x 좌표는 변화가 없으므로 dx[0]은 0이다.

`x + dx[d]` 와 `y + dy[d]`는 현재 위치 `(x, y)`로부터 상, 하, 좌, 우 방향으로 이동한 위치를 나타낸다. 이렇게 계산한 nx, ny는 이동한 위치의 좌표이며, 이를 기준으로 2차원 배열에서 값을 조회한다.

---

### 상방향 이동하려면 x 좌표 변화량은 0이어야 한다.

`for(int d = 0; d < 4; d++)` 구문에서 `d`는 0부터 3까지 증가하며, 각각 상, 하, 좌, 우를 나타낸다. 이 때, dx와 dy 배열의 인덱스로 `d`를 사용하여 방향에 따른 x와 y의 변화량을 구한다.

그렇다면 핵심은 **dx, dy 배열의 인덱스를 적절하게 설정 해주는** 것이겠다. 상,하,좌,우로 이동할 때는 y 좌표만 1 감소 시키고 x 좌표는 변화시키지 않아야 한다.

그래서 상방향으로 이동하고자 할 땐, dy 배열 -1에 해당하는 인덱스의 대응하는 dx 배열엔 0이 오도록 설정해야 한다.

---

## (1,1)에서 우측 방향 이동

```java
x + dx[d] : 1 , 0
y + dy[d] : 1 , 1
nx: 1 / ny: 2
탐색결과: 7 
```

코드에서 (1,1)에서 우측 이동을 시도할 때, `dx[d]` 배열에서 상응하는 값이 0이고, `dy[d]` 배열에서 상응하는 값이 1이다.

이것은 x 좌표를 변경하지 않고 y 좌표를 1 증가시키는 연산을 의미한다. 따라서, (1,1)에서 `dy[d]` 즉, 1을 더해 y 좌표를 1 증가시키면, 새로운 좌표는 (1,2)가 된다.

이 때 해당 위치의 배열 값이 7이므로, 우측 이동을 한 탐색 결과가 7이 나온다.

---

### 우측 이동하려면 y 좌표 변화량은 1이어야 한다.

우측으로 이동하고자 할 땐, dx 배열 1에 해당하는 인덱스의 대응하는 dy 배열엔 1이 오도록 설정해야 한다. 이렇게 설정하면, x 좌표가 변화하지 않고 y 좌표만 1 증가하여 우측으로 이동할 수 있다.

---
