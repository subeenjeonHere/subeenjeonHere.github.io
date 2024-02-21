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

```java
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

```java
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

# ☻ 평균 구하기

2024년 2월 20일

### **문제 설명**

정수를 담고 있는 배열 arr의 평균값을 return하는 함수, solution을 완성해보세요.

### **제한사항**

- arr은 길이 1 이상, 100 이하인 배열입니다.
- arr의 원소는 -10,000 이상 10,000 이하인 정수입니다.

### **입출력 예**

| arr | return |
| --- | --- |
| [1,2,3,4] | 2.5 |
| [5,5] | 5 |

---

# ☺︎ Snippets

```java
class Solution {
    public double solution(int[] arr) {
        double answer = 0;
        int start = 0;
        while(start<arr.length){
            answer += arr[start];
        }
        answer = answer / arr.length;
        return answer;
    }
}
```

```java
실행 시간이 10.0초를 초과하여 실행이 중단되었습니다. 실행 시간이 더 짧은 다른 방법을 찾아보세요.
```

# ☠️

```java
import java.util.*;
class Solution {
    public double solution(int[] arr) {
        double answer = 0;
        
        List<Integer> list = new ArrayList<>();
        for(int i=0; i<arr.length; i++){
            list.add(arr[i]);
        }
        answer = list.stream()
            .mapToDouble(Integer::doubleValue)
            .average()
            .orElse(0.0);
        return answer;
    }
}
```

```java
정확성  테스트
테스트 1 〉	통과 (6.96ms, 75.5MB)
테스트 2 〉	통과 (4.29ms, 84.9MB)
테스트 3 〉	통과 (3.51ms, 77.2MB)
테스트 4 〉	통과 (6.66ms, 75.4MB)
테스트 5 〉	통과 (5.08ms, 78.4MB)
테스트 6 〉	통과 (4.48ms, 77.8MB)
테스트 7 〉	통과 (5.45ms, 77.5MB)
테스트 8 〉	통과 (5.07ms, 72.4MB)
테스트 9 〉	통과 (4.60ms, 83.7MB)
테스트 10 〉	통과 (3.61ms, 74.1MB)
테스트 11 〉	통과 (4.99ms, 74MB)
테스트 12 〉	통과 (3.03ms, 75.8MB)
테스트 13 〉	통과 (3.66ms, 75.1MB)
테스트 14 〉	통과 (3.61ms, 77.6MB)
테스트 15 〉	통과 (3.27ms, 74.8MB)
테스트 16 〉	통과 (3.01ms, 75.2MB)
```


### Stream API

데이터의 흐름을 처리하는데 특화된 인터페이스임. 특히 데이터 병렬 처리를 쉽게 구현할 수 있도록 돕는다. 스트림 API는 데이터를 '스트림'으로 처리한다는 개념에서 이름이 유래되었다고 함.

‘스트림'은 **데이터의 연속적인 흐름**을 의미하며, 이 흐름을 여러 가지 방법으로 변환하거나 추출하는 것이 가능하다. 예를 들어, 리스트의 모든 요소를 특정 조건에 따라 필터링하거나, 각 요소에 함수를 적용하는 것이 가능하다.

'API'로 부르는 이유는, **스트림 API가 데이터 처리를 위한 메서드와 연산**을 제공하는 애플리케이션 프로그래밍 인터페이스(API)이기 때문이라고 한다. **API**는 특정 기능을 사용하기 위한 메서드나 도구를 제공하는 것.

### 그럼 다르게 풀 수 있겠다는 생각이

```java
import java.util.*;
class Solution {
    public double solution(int[] arr) {

        double answer = 0;
        answer = Arrays.stream(arr)
            .average()
            .getAsDouble();
        return answer;
    }
}
```

이렇게도 풀 수 있다.

```java
테스트 1 〉	통과 (2.90ms, 75.1MB)
테스트 2 〉	통과 (2.79ms, 74MB)
테스트 3 〉	통과 (2.69ms, 74.9MB)
테스트 4 〉	통과 (3.40ms, 73.2MB)
테스트 5 〉	통과 (3.45ms, 84.1MB)
테스트 6 〉	통과 (3.42ms, 80.6MB)
테스트 7 〉	통과 (2.23ms, 74.4MB)
테스트 8 〉	통과 (1.96ms, 78.8MB)
테스트 9 〉	통과 (2.12ms, 75.9MB)
테스트 10 〉	통과 (1.93ms, 77.6MB)
테스트 11 〉	통과 (2.73ms, 76.4MB)
테스트 12 〉	통과 (1.84ms, 76.7MB)
테스트 13 〉	통과 (2.29ms, 71.3MB)
테스트 14 〉	통과 (1.87ms, 68.5MB)
테스트 15 〉	통과 (2.01ms, 74.8MB)
테스트 16 〉
```
---

# ☻ 짝수와 홀수

2024년 2월 20일

### **문제 설명**

정수 num이 짝수일 경우 "Even"을 반환하고 홀수인 경우 "Odd"를 반환하는 함수, solution을 완성해주세요.

### 제한 조건

- num은 int 범위의 정수입니다.
- 0은 짝수입니다.

### 입출력 예

| num | return |
| --- | --- |
| 3 | "Odd" |
| 4 | "Even" |

---

# ☺︎ Snippets

```java
class Solution {
    public String solution(int num) {

        String answer = "";
        answer = (num%2==0) ? "Even" : "Odd";
        return answer;
    }
}
```

삼항연산자로 풀었다.

# Java 삼항 연산자

삼항 연산자는 Java에서 사용하는 조건 연산자. 이 연산자는 세 개의 피연산자를 가지며, "조건 ? 값1 : 값2"와 같은 형태로 사용된다.

조건 부분은 boolean 표현식이어야 하며,

이 조건이 참(true)일 경우에는 값1이 반환되고, 거짓(false)일 경우에는 값2가 반환된다.

---

# ☻ x만큼 간격이 있는 n개의 숫자

2024년 2월 20일

### **문제 설명**

함수 solution은 정수 x와 자연수 n을 입력 받아, x부터 시작해 x씩 증가하는 숫자를 n개 지니는 리스트를 리턴해야 합니다. 다음 제한 조건을 보고, 조건을 만족하는 함수, solution을 완성해주세요.

### **제한 조건**

- x는 -10000000 이상, 10000000 이하인 정수입니다.
- n은 1000 이하인 자연수입니다.

### **입출력 예**

| x | n | answer |
| --- | --- | --- |
| 2 | 5 | [2,4,6,8,10] |
| 4 | 3 | [4,8,12] |
| -4 | 2 | [-4, -8] |

---

# ☺︎ Snippets

```java
import java.util.*;
class Solution {
    public long[] solution(int x, int n) {
        
        long[] answer = new long[n];
        
        long sum = 0L;
        
        for(int i=0; i<answer.length; i++){
            sum += x;
            answer[i] = sum;
        }
        return answer;
    }
}
```
---

### ☻ 자릿수 더하기

2024년 2월 21일

### **문제 설명**

자연수 N이 주어지면, N의 각 자릿수의 합을 구해서 return 하는 solution 함수를 만들어 주세요.

예를들어 N = 123이면 1 + 2 + 3 = 6을 return 하면 됩니다.

### 제한사항

- N의 범위 : 100,000,000 이하의 자연수

---

### 입출력 예

| N | answer |
| --- | --- |
| 123 | 6 |
| 987 | 24 |

### ☻ Snippets

```java
import java.util.*;

public class Solution {
    public int solution(int n) {
        int answer = 0;

        String str = String.valueOf(n);
        String[] arr = new String[str.length()];
        int sum = 0;
        for(int i=0; i<arr.length; i++){
            arr[i] = String.valueOf(str.charAt(i));
            sum += Integer.parseInt(arr[i]);
        }
        answer = sum;

        return answer;
    }
}
```

---

### ☻  나머지가 1이 되는 수

2024년 2월 21일

### **문제 설명**

자연수 `n`이 매개변수로 주어집니다. `n`을 `x`로 나눈 나머지가 1이 되도록 하는 가장 작은 자연수 `x`를 return 하도록 solution 함수를 완성해주세요. 답이 항상 존재함은 증명될 수 있습니다.

---

### 제한사항

- 3 ≤ `n` ≤ 1,000,000

---

### 입출력 예

| n | result |
| --- | --- |
| 10 | 3 |
| 12 | 11 |

# ☺︎ Snippets

```java
class Solution {
    public int solution(int n) {
        int answer = 0;
        int x =1;

        for(int i=0; i<= n; i++){
            if(n%x ==1){
                answer = x;
                return answer;
            }else{
                x++;
            }
        }
        return answer;
    }
}
```