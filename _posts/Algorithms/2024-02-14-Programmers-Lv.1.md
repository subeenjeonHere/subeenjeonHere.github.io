---
title: Programmers Lv.1
date: 2024-02-14
categories: Algorithms
tags:
  [
    Algorithms
    Programmers
  ]
---

# [Programmers](https://school.programmers.co.kr/)

# ☻ 같은 숫자는 싫어

### **문제 설명**

배열 arr가 주어집니다. 배열 arr의 각 원소는 숫자 0부터 9까지로 이루어져 있습니다. 이때, 배열 arr에서 연속적으로 나타나는 숫자는 하나만 남기고 전부 제거하려고 합니다. 단, 제거된 후 남은 수들을 반환할 때는 배열 arr의 원소들의 순서를 유지해야 합니다. 예를 들면,

- arr = [1, 1, 3, 3, 0, 1, 1] 이면 [1, 3, 0, 1] 을 return 합니다.
- arr = [4, 4, 4, 3, 3] 이면 [4, 3] 을 return 합니다.

배열 arr에서 연속적으로 나타나는 숫자는 제거하고 남은 수들을 return 하는 solution 함수를 완성해 주세요.

### 제한사항

- 배열 arr의 크기 : 1,000,000 이하의 자연수
- 배열 arr의 원소의 크기 : 0보다 크거나 같고 9보다 작거나 같은 정수

---

### 입출력 예

| arr | answer |
| --- | --- |
| [1,1,3,3,0,1,1] | [1,3,0,1] |
| [4,4,4,3,3] | [4,3] |

# ☺︎ MDS

```jsx
arr = [1, 1, 3, 3, 0, 1, 1] 이면 [1, 3, 0, 1] 을 return 합니다.
arr = [4, 4, 4, 3, 3] 이면 [4, 3] 을 return 합니다.

1. Stack에 값을 push하고, stack 내의 원소들을 탐색 후 중복된 값이 있다면 push하지 말아야 한다.
2. 중복된 값이 있어서 push되지 않은 element들을 배열에 저장한다.

1. 스택에 첫 번째로 값을 push
2. 이후 peek하여 중복된 값이면 push하지 않는다
3. peek 값과 push할 값이 같지 않으면 stk에 push
```

# ☺︎ Snippets

> 성공
>

```jsx
import java.util.*;

public class Solution {
    public int[] solution(int []arr) {
    
        
        Stack<Integer> stk = new Stack<>(  );
        //스택에 첫 번째 값 삽입
        stk.push(arr[0]);
        
        for(int i=1; i<arr.length; i++){
            if(!stk.isEmpty() && stk.peek().equals(arr[i])){
                continue;
            } else if(!stk.isEmpty()&&!stk.peek().equals(arr[i])){
                stk.push(arr[i]);
            }
        }    
        
        int[] answer = new int[stk.size()];

        for(int i=answer.length-1; i>=0; i--){
            answer[i] = stk.pop();
        }
        return answer;
    }
}
```

### ► 정리

1. 이후 삽입될 원소들 중복을 체크(peek)하기 위해, 스택에 첫 번째 값을 push
2. 스택이 비어있지 않고
    1. 스택의 top 원소를 peek() 했을 때 arr[i] 과 동일하다면 push하지 않음
    2. 스택의 top 원소를 peek() 했을 때 arr[i]와 동일하지 않다면 push함
3. 이러한 과정을 거치면 stack에는 중복된 원소가 존재하지 않을 것임.
    1. 왜냐하면, 값을 삽입하기 이전 중복된 요소가 없을 때만 push 하였기 때문임.
4. 이후 stack에 있는 원소값을 [1,3,0,1] 로 리턴해야 하기 때문에 1차원 배열이 필요함.
5. 주의할 것은 stack에서 pop되는 순서는 맨 마지막에 push된 원소부터 pop됨
    1. Stack은 LIFO 형식의 자료구조이기 때문에 한 쪽에서만 push, pop 연산을 반복하기 때문임. 따라서 pop된 순서대로 결과 배열에 저장하면 역순으로 저장될 것임.
6. 따라서, 결과 배열에 저장할 때 역순으로 (결과배열의 맨 마지막 idx에서 0번째 idx까지) 저장을 해주어야 함.
7. 여기서 사용된 stack 연산은 push, pop, peek, isEmpty 연산임. 스택의 Operation과 스택에 자료가 어떻게 삽입되고 저장 되는지를 구현하는 기본적인 문제였음.

---