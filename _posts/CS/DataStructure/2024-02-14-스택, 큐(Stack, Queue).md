---
title: 스택, 큐(Stack, Queue)
date: 2024-02-14
categories:
  - DataStructure
tags:
  [
    DataStructure
  ]
---



# ☻ Stack (스택)

1. 들어온 시간 순으로 데이터를 쌓아갈 때, 가장 위 (가장 최근에 삽입)에 있는 데이터를 삭제하거나, 거기에 이어서 새로운 데이터를 삽입할 수 있도록 하는 선형 자료구조.
2. 한 쪽 끝에서만 자료를 넣고 뺄 수 있는 LIFO 형식의 자료구조이다.

## 스택 구성도

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/34d98fbf-2319-405d-a23f-851d914953c2)

- 한 방향으로만 `PUSH`와 `POP`을 이용하여 자료를 넣고 꺼낸다.
- `TOP` 은 스택에서 가장 위에있는 데이터로, 스택 포인터(Stack Pointer)라고도 불린다.

## 스택 연산

| 연산        | 설명                        |
|-----------|---------------------------|
| push()    | item 하나를 스택의 가장 윗 부분에 추가  |
| pop()     | 스택의 가장 위에있는 데이터를 제거하고, 꺼냄 |
| peek()    | 스택의 가장 위에 있는 항목을 반환       |
| isEmpty() | 스택이 비어있을 때 true 반환        |
| isFull()  | 스택이 가득 찼다면, true 반환       |

## 스택 구현

```java
public class Stack {
    public static <arr> void main(String[] args) {
        int[] arr = {1, 1, 3, 3, 0, 1, 1};

        // Pushing element on the top of the stack
        java.util.Stack<Integer> stk = new java.util.Stack<>();
        for (int i = 0; i < 5; i++) {
            stk.push(i);
        }

        // Peek (On the top of the stack
        Integer element = (Integer) stk.peek();
        System.out.println("Peek" + element);

        //Searching element in the stack
        Integer searching = (Integer) stk.search(1);
        System.out.println("Search" + searching);

        // Popping element from the top of the stack
        for (int i = 0; i < 5; i++) {
            int ele = stk.pop();
            System.out.println(ele);
        }
    }
}
```

---

# ☻ Queue (큐)

스택과는 달리 한 쪽에서는 데이터 삽입, 다른 한 쪽에서는 데이터의 삭제만 가능한 **FIFO(First-In First-Out)**의 구조를 가지는 선형 데이터 구조. 즉, 먼저 들어간 것이 먼저 나온다.

## 큐 구성도

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/4046bba2-a83a-415d-9a3f-e0cd6996e6e8)

- 삭제 연산이 수행되는 곳을 프론트(Front), 삽입 연산이 수행되는 곳을 리어(Rear)라고 칭한다.
- 프론트에서 이뤄지는 삭제 연산을 디큐(Dequeue), 리어에서 이뤄지는 삽입 연산을 인큐(Enqueue)라고 칭한다.

## 큐의 연산

| 연산 | 설명 |
| --- | --- |
| Enqueue(), Add(), Offer() | 항목을 큐의 뒤쪽에 추가 |
| Dequeue() | 큐의 앞에서 항목을 제거하고 반환 |
| Front(), Front() | 제거하지 않고, 큐의 앞에 있는 요소를 확인 |
| IsEmpty() | 큐가 비어있는 경우 true를 반환 |
| IsFull() | 큐가 가득 찬 경우 true를 반환 |
| Rear() | 제거하지 않고, 큐의 뒤쪽 끝에 있는 요소를 반환 |

## 큐 구현

```java
import java.util.LinkedList;
import java.util.Queue;

public class Main {
  public static void main(String[] args) {
    Queue<Integer> queue = new LinkedList<>();

    // 큐에 데이터 추가
    for(int i=0; i<5; i++) {
      queue.add(i);
    }

    // 큐에 있는 요소 확인
    System.out.println("Elements in Queue: " + queue);

    // 큐에서 데이터 제거
    int removed = queue.remove();
    System.out.println("removed element: " + removed);

    System.out.println(queue);

    // 큐에 있는 요소 확인
    int head = queue.peek();
    System.out.println("head of queue: " + head);

    // 큐 크기 확인
    int size = queue.size();
    System.out.println("Size of queue: " + size);
  }
}

```