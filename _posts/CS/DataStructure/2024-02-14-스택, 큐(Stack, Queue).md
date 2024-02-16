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

1. 들어온 시간 순으로 데이터를 쌓아갈 때, 가장 위 (가장 최근에 삽입)에 있는 데이터를 삭제하거나, 거기에 이어서 새로운 데이터를 삽입할 수 있도록 하는 추상 자료형이다.
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

