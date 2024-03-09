---
title: Programmers Lv.1
date: 2024-02-14
categories: PS
tags:
  [
    PS
    Programmers
  ]
---


---

# [Programmers](https://school.programmers.co.kr/)

# ☻ 같은 숫자는 싫어

### **문제 설명**

배열 arr가 주어집니다. 배열 arr의 각 원소는 숫자 0부터 9까지로 이루어져 있습니다. 이때, 배열 arr에서 연속적으로 나타나는 숫자는 하나만 남기고 전부 제거하려고 합니다. 단, 제거된 후 남은 수들을
반환할 때는 배열 arr의 원소들의 순서를 유지해야 합니다. 예를 들면,

- arr = [1, 1, 3, 3, 0, 1, 1] 이면 [1, 3, 0, 1] 을 return 합니다.
- arr = [4, 4, 4, 3, 3] 이면 [4, 3] 을 return 합니다.

배열 arr에서 연속적으로 나타나는 숫자는 제거하고 남은 수들을 return 하는 solution 함수를 완성해 주세요.

### 제한사항

- 배열 arr의 크기 : 1,000,000 이하의 자연수
- 배열 arr의 원소의 크기 : 0보다 크거나 같고 9보다 작거나 같은 정수

---

### 입출력 예

| arr             | answer    |
|-----------------|-----------|
| [1,1,3,3,0,1,1] | [1,3,0,1] |
| [4,4,4,3,3]     | [4,3]     |

# ☺︎ Snippets

> 성공
>

```java
import java.util.*;

public class Solution {
    public int[] solution(int[] arr) {


        Stack<Integer> stk = new Stack<>();
        //스택에 첫 번째 값 삽입
        stk.push(arr[0]);

        for (int i = 1; i < arr.length; i++) {
            if (!stk.isEmpty() && stk.peek().equals(arr[i])) {
                continue;
            } else if (!stk.isEmpty() && !stk.peek().equals(arr[i])) {
                stk.push(arr[i]);
            }
        }

        int[] answer = new int[stk.size()];

        for (int i = answer.length - 1; i >= 0; i--) {
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

| arr       | return |
|-----------|--------|
| [1,2,3,4] | 2.5    |
| [5,5]     | 5      |

---

# ☺︎ Snippets

```java
class Solution {
    public double solution(int[] arr) {
        double answer = 0;
        int start = 0;
        while (start < arr.length) {
            answer += arr[start];
        }
        answer = answer / arr.length;
        return answer;
    }
}
```

```java
실행 시간이 10.0
초를 초과하여
실행이 중단되었습니다.
실행 시간이
더 짧은
다른 방법을
찾아보세요 .
```

# ☠️

```java
import java.util.*;

class Solution {
    public double solution(int[] arr) {
        double answer = 0;

        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < arr.length; i++) {
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
정확성 테스트
테스트 1 〉

통과(6.96ms, 75.5MB)

테스트 2 〉

통과(4.29ms, 84.9MB)

테스트 3 〉

통과(3.51ms, 77.2MB)

테스트 4 〉

통과(6.66ms, 75.4MB)

테스트 5 〉

통과(5.08ms, 78.4MB)

테스트 6 〉

통과(4.48ms, 77.8MB)

테스트 7 〉

통과(5.45ms, 77.5MB)

테스트 8 〉

통과(5.07ms, 72.4MB)

테스트 9 〉

통과(4.60ms, 83.7MB)

테스트 10 〉

통과(3.61ms, 74.1MB)

테스트 11 〉

통과(4.99ms, 74MB)

테스트 12 〉

통과(3.03ms, 75.8MB)

테스트 13 〉

통과(3.66ms, 75.1MB)

테스트 14 〉

통과(3.61ms, 77.6MB)

테스트 15 〉

통과(3.27ms, 74.8MB)

테스트 16 〉

통과(3.01ms, 75.2MB)
```

### Stream API

데이터의 흐름을 처리하는데 특화된 인터페이스임. 특히 데이터 병렬 처리를 쉽게 구현할 수 있도록 돕는다. 스트림 API는 데이터를 '스트림'으로 처리한다는 개념에서 이름이 유래되었다고 함.

‘스트림'은 **데이터의 연속적인 흐름**을 의미하며, 이 흐름을 여러 가지 방법으로 변환하거나 추출하는 것이 가능하다. 예를 들어, 리스트의 모든 요소를 특정 조건에 따라 필터링하거나, 각 요소에 함수를 적용하는
것이 가능하다.

'API'로 부르는 이유는, **스트림 API가 데이터 처리를 위한 메서드와 연산**을 제공하는 애플리케이션 프로그래밍 인터페이스(API)이기 때문이라고 한다. **API**는 특정 기능을 사용하기 위한 메서드나
도구를 제공하는 것.

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
테스트 1 〉

통과(2.90ms, 75.1MB)

테스트 2 〉

통과(2.79ms, 74MB)

테스트 3 〉

통과(2.69ms, 74.9MB)

테스트 4 〉

통과(3.40ms, 73.2MB)

테스트 5 〉

통과(3.45ms, 84.1MB)

테스트 6 〉

통과(3.42ms, 80.6MB)

테스트 7 〉

통과(2.23ms, 74.4MB)

테스트 8 〉

통과(1.96ms, 78.8MB)

테스트 9 〉

통과(2.12ms, 75.9MB)

테스트 10 〉

통과(1.93ms, 77.6MB)

테스트 11 〉

통과(2.73ms, 76.4MB)

테스트 12 〉

통과(1.84ms, 76.7MB)

테스트 13 〉

통과(2.29ms, 71.3MB)

테스트 14 〉

통과(1.87ms, 68.5MB)

테스트 15 〉

통과(2.01ms, 74.8MB)

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
|-----|--------|
| 3   | "Odd"  |
| 4   | "Even" |

---

# ☺︎ Snippets

```java
class Solution {
    public String solution(int num) {

        String answer = "";
        answer = (num % 2 == 0) ? "Even" : "Odd";
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

| x  | n | answer       |
|----|---|--------------|
| 2  | 5 | [2,4,6,8,10] |
| 4  | 3 | [4,8,12]     |
| -4 | 2 | [-4, -8]     |

---

# ☺︎ Snippets

```java
import java.util.*;

class Solution {
    public long[] solution(int x, int n) {

        long[] answer = new long[n];

        long sum = 0L;

        for (int i = 0; i < answer.length; i++) {
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

| N   | answer |
|-----|--------|
| 123 | 6      |
| 987 | 24     |

### ☻ Snippets

```java
import java.util.*;

public class Solution {
    public int solution(int n) {
        int answer = 0;

        String str = String.valueOf(n);
        String[] arr = new String[str.length()];
        int sum = 0;
        for (int i = 0; i < arr.length; i++) {
            arr[i] = String.valueOf(str.charAt(i));
            sum += Integer.parseInt(arr[i]);
        }
        answer = sum;

        return answer;
    }
}
```

---

### ☻ 나머지가 1이 되는 수

2024년 2월 21일

### **문제 설명**

자연수`n`이 매개변수로 주어집니다.`n`을`x`로 나눈 나머지가 1이 되도록 하는 가장 작은 자연수`x`를 return 하도록 solution 함수를 완성해주세요. 답이 항상 존재함은 증명될 수 있습니다.

---

### 제한사항

- 3 ≤`n`≤ 1,000,000

---

### 입출력 예

| n  | result |
|----|--------|
| 10 | 3      |
| 12 | 11     |

# ☺︎ Snippets

```java
class Solution {
    public int solution(int n) {
        int answer = 0;
        int x = 1;

        for (int i = 0; i <= n; i++) {
            if (n % x == 1) {
                answer = x;
                return answer;
            } else {
                x++;
            }
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

| N   | answer |
|-----|--------|
| 123 | 6      |
| 987 | 24     |

### ☻ Snippets

```java
import java.util.*;

public class Solution {
    public int solution(int n) {
        int answer = 0;

        String str = String.valueOf(n);
        String[] arr = new String[str.length()];
        int sum = 0;
        for (int i = 0; i < arr.length; i++) {
            arr[i] = String.valueOf(str.charAt(i));
            sum += Integer.parseInt(arr[i]);
        }
        answer = sum;

        return answer;
    }
}
```

---

### ☻ 자릿수 뒤집어 배열로 만들기

2024년 2월 24일

### **문제 설명**

자연수 n을 뒤집어 각 자리 숫자를 원소로 가지는 배열 형태로 리턴해주세요. 예를들어 n이 12345이면 [5,4,3,2,1]을 리턴합니다.

### 제한 조건

- n은 10,000,000,000이하인 자연수입니다.

### 입출력 예

| n     | return      |
|-------|-------------|
| 12345 | [5,4,3,2,1] |

---

# ☺︎ a/t

- long n 의 각 자릿수를 answer 배열에 넣고, reverse
    - long을 int로 Casting girl
    - (int)

    ```jsx
    long x = 11L;
    int xInt = x.intValue();
    ```

    - 각 자릿수를 charAt처럼 추출하기
    - 이를 배열에 넣고 역순으로 정렬하기
    - 혹은 Stream 사용

# ☻ Snippets

```java
import java.util.*;

class Solution {
    public int[] solution(long n) {

        String strings = String.valueOf(n);

        int[] answer = new int[strings.length()];

        for (int i = 0; i < answer.length; i++) {
            answer[i] = strings.charAt(i) - '0';
        }

        // 배열 뒤집기, 콜렉션 말고 for문으로 뒤집기
        for (int i = 0; i < answer.length / 2; i++) {
            int temp;
            temp = answer[i];
            // 0 4, 1 3

            answer[i] = answer[answer.length - 1 - i];
            answer[answer.length - 1 - i] = temp;
        }

        return answer;
    }
}
```
```jsx
        for(int i = answer.length-1; i >=0 ; i--){
            answer[i] = nums.charAt(nums.length()-i-1)-'0';
        }
```

Collection.reverse(answer);로 풀 수도 있었으나 일단 기본적인 것부터 짚고 넘어가려고 for문 돌렸다. 🥲

---

### ☻ 문자열 내 p와 y의 개수

2024년 2월 24일

대문자와 소문자가 섞여있는 문자열 s가 주어집니다. s에 'p'의 개수와 'y'의 개수를 비교해 같으면 True, 다르면 False를 return 하는 solution를 완성하세요. 'p', 'y' 모두 하나도 없는 경우는 항상 True를 리턴합니다. 단, 개수를 비교할 때 대문자와 소문자는 구별하지 않습니다.

예를 들어 s가 "pPoooyY"면 true를 return하고 "Pyy"라면 false를 return합니다.

### 제한사항

- 문자열 s의 길이 : 50 이하의 자연수
- 문자열 s는 알파벳으로만 이루어져 있습니다.

---

### 입출력 예

| s | answer |
| --- | --- |
| "pPoooyY" | true |
| "Pyy" | false |

---

### ☺︎ Snippets

```jsx
class Solution {
    boolean solution(String s) {
        boolean answer = true;
        
        //s chartAt으로 p, y 있는지 검사
        //p,y 없다면 true 리턴
        //p,y 대문자이거나 소문자이면 count
        //for문 내에서 charAt으로 비교하며 p == y ==일 경우 countp, county 증가
        //countp == county라면 true 리턴 다르면 false 리턴
        //대문자 소문자 관계없으니 toUpperCase
        String strings = s.toUpperCase();
        
        // System.out.println(strings);
        int countp = 0;
        int county = 0;
        
        for(int i = 0; i<strings.length(); i++){
            if(strings.charAt(i) == 'P'){
                countp ++;
            }
            
            if(strings.charAt(i)=='Y'){
                county ++;
            }
        }
        if(countp == county){
            answer = true;
        }else{
            answer = false;
        }
        
        return answer;
    }
}
```
---
### ☻ 정수 제곱근 판별

2024년 2월 24일

### **문제 설명**

임의의 양의 정수 n에 대해, n이 어떤 양의 정수 x의 제곱인지 아닌지 판단하려 합니다.

n이 양의 정수 x의 제곱이라면 x+1의 제곱을 리턴하고, n이 양의 정수 x의 제곱이 아니라면 -1을 리턴하는 함수를 완성하세요.

### 제한 사항

- n은 1이상, 50000000000000 이하인 양의 정수입니다.

### 입출력 예

| n | return |
| --- | --- |
| 121 | 144 |
| 3 | -1 |

### 입출력 예 설명

**입출력 예#1**

121은 양의 정수 11의 제곱이므로, (11+1)를 제곱한 144를 리턴합니다.

**입출력 예#2**

3은 양의 정수의 제곱이 아니므로, -1을 리턴합니다.

---

### ☺︎ a/t

양의 정수의 제곱이 아니라는 것은, 1부터 N까지의 각 정수들을 x라고 가정했을 때, x * x 가 되는 값이 없음을 말한다. 4라고 가정했을 때 1*1, 2*2, 3*3 이 4와 동일한 것이 있는가? 를 묻는 것이다.

그리고 해당 값의 제곱을 구해야 하므로, Math.pow 메소드를 사용하면 된다.

> 시간초과
>

```jsx
class Solution {
    public long solution(long n) {
        long answer = 0;
        
        for(int i=0; i<=n; i++  ){
            int res = i*i;
            if(res == n){
                answer = (int) Math.pow(i+1,2);
                break;
            }
            else{
                answer = -1;
            }
        }        
        return answer;
    }
}
```

> 실패
>

```jsx
public class Main2 {
    public static void main(String[] args) {

        int n = 121;
        // i*i를 곱해서 121이 되는 수라면 121의 약수일 것임
        //소수일 경우, 이 값이 없으므로 -1을 return
        //소수가 아닐 경우에, 해당 값의 약수를 구해 i*i == 121이 되는 값을 찾아 Math.pow
        //혹은 약수 중에**서, Math.pow(i,2)가 n일 경우**
        for (int i = 1; i <= Math.sqrt(n); i++) {
            if (n % i == 0) {
                if (i * i == n) {
                    System.out.println((int) Math.pow(i + 1, 2));
                    return;
                }
            }
        }
        System.out.println("-1");
    }
}
```

```jsx
테스트 1 〉	통과 (0.13ms, 77.4MB)
테스트 2 〉	실패 (63.29ms, 87.8MB)
테스트 3 〉	통과 (0.14ms, 73.9MB)
테스트 4 〉	실패 (14.55ms, 77.2MB)
테스트 5 〉	통과 (0.76ms, 77.7MB)
테스트 6 〉	통과 (0.07ms, 76.3MB)
테스트 7 〉	실패 (1.65ms, 72.8MB)
테스트 8 〉	통과 (0.36ms, 75.9MB)
테스트 9 〉	통과 (0.60ms, 72.5MB)
테스트 10 〉	실패 (1.73ms, 72.8MB)
테스트 11 〉	실패 (2.87ms, 71.8MB)
테스트 12 〉	실패 (2.45ms, 73.2MB)
테스트 13 〉	통과 (0.08ms, 75.4MB)
테스트 14 〉	실패 (2.70ms, 72.2MB)
테스트 15 〉	통과 (1.03ms, 78.5MB)
테스트 16 〉	통과 (0.37ms, 86.4MB)
테스트 17 〉	통과 (0.03ms, 75MB)
테스트 18 〉	실패 (0.03ms, 76.2MB)
```

### 이렇게 하니 통과

왜지?

```jsx
class Solution {
    public long solution(long n) {    
        
    for (int i = 1; i <= Math.sqrt(n); i++) {
            if (n % i == 0) {
                if (Math.pow(i,2)==n) {
                    return (long) Math.pow(i+1  ,2);                   
                }
            }
        }
        return -1;
    }
}  


```

반환 타입을 long으로, 왜냐면 매개변수가 long이니 반환도 long 타입으로 해주어야 했다.

i가 n의 약수일 때는 같다.

- i*i가 n 이라면
- i의 제곱이 n과 같다면

이 둘은 사실상 같은 거 아닌가?

### 타입

```jsx
 if ((long)i * (long)i == n) {
```

이게 문제였다. i가 int였으니, 당연히 long 값이 들어왔을 때 실패한다.

아무튼 타입의 문제였고 솔..

---

### ☻ 정수 내림차순 배치하기

2024년 2월 25일

### **문제 설명**

함수 solution은 정수 n을 매개변수로 입력받습니다. n의 각 자릿수를 큰것부터 작은 순으로 정렬한 새로운 정수를 리턴해주세요. 예를들어 n이 118372면 873211을 리턴하면 됩니다.

### 제한 조건

- `n`은 1이상 8000000000 이하인 자연수입니다.

### 입출력 예

| n | return |
| --- | --- |
| 118372 | 873211 |

---

### ☺︎ Snippets

```jsx
import java.util.*;

class Solution {
    public long solution(long n) {
        long answer = 0;

        String num = String.valueOf(n);
        int[] arr = new int[num.length()];
        StringBuilder sb = new StringBuilder();
        
        for(int i=0; i<arr.length; i++){
            arr[i] = Integer.parseInt(String.valueOf(num.charAt(i)));
        }

        Arrays.sort(arr);
        for(int i=0; i < arr.length/2; i++){
            int temp = 0;
            temp = arr[i];
            arr[i]=arr[arr.length-i-1];
            arr[arr.length-i-1] = temp;
        }
        for(int i=0; i<arr.length; i++){
            // sb.append(arr[i]);
            sb.append(arr[i]);
        }
        return Long.valueOf(sb.toString());

    }
}
```

---

### ☻ 제일 작은 수 제거하기

2024년 2월 25일

### **문제 설명**

정수를 저장한 배열, arr 에서 가장 작은 수를 제거한 배열을 리턴하는 함수, solution을 완성해주세요. 단, 리턴하려는 배열이 빈 배열인 경우엔 배열에 -1을 채워 리턴하세요. 예를들어 arr이 [4,3,2,1]인 경우는 [4,3,2]를 리턴 하고, [10]면 [-1]을 리턴 합니다.

### 제한 조건

- arr은 길이 1 이상인 배열입니다.
- 인덱스 i, j에 대해 i ≠ j이면 arr[i] ≠ arr[j] 입니다.

### 입출력 예

| arr | return |
| --- | --- |
| [4,3,2,1] | [4,3,2] |
| [10] | [-1] |

---

### ☺︎ a/t

배열에서 최솟값을 찾고  스택에 Push하며 최솟값을 만나면 조건문으로 탈출

```jsx
import java.util.*;
class Solution {
    public Stack<Integer> solution(int[] arr) {
        int[] answer = {};
        int min= arr[0];
        
        for(int i=0; i<arr.length; i++){
            if(min>arr[i]){
                min = arr[i];
            }
        }
        Stack<Integer> stack = new Stack<>();
        if(arr.length==1){
            stack.push(-1);
            return stack;
        }
        for(int i=0; i<arr.length; i++){
            if(min == arr[i]){
                continue;
            }
            stack.push(arr[i]);
        }
        return stack;
    }
}
```
---

### ☻ 직사각형 별찍기

2024년 2월 25일

### **문제 설명**

이 문제에는 표준 입력으로 두 개의 정수 n과 m이 주어집니다.

별(*) 문자를 이용해 가로의 길이가 n, 세로의 길이가 m인 직사각형 형태를 출력해보세요.

---

### 제한 조건

- n과 m은 각각 1000 이하인 자연수입니다.

---

### 예시

입력

`5 3`

출력

- `****
  *****`

---

### ☺︎Snippets

```jsx
import java.util.Scanner;

class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int a = sc.nextInt();
        int b = sc.nextInt();
        StringBuilder sb = new StringBuilder();
        for(int i=0; i<b; i++){
            for(int j=0; j<a; j++){
                sb.append("*");
            }
            sb.append("\n");
        }

        System.out.println(sb.toString());
    }
}
```
---
### ☻ 행렬의 덧셈

2024년 2월 25일

### **문제 설명**

행렬의 덧셈은 행과 열의 크기가 같은 두 행렬의 같은 행, 같은 열의 값을 서로 더한 결과가 됩니다. 2개의 행렬 arr1과 arr2를 입력받아, 행렬 덧셈의 결과를 반환하는 함수, solution을 완성해주세요.

### 제한 조건

- 행렬 arr1, arr2의 행과 열의 길이는 500을 넘지 않습니다.

### 입출력 예

| arr1 | arr2 | return |
| --- | --- | --- |
| [[1,2],[2,3]] | [[3,4],[5,6]] | [[4,6],[7,9]] |
| [[1],[2]] | [[3],[4]] | [[4],[6]] |

---

### ☺︎ a/t

이중 for문

### ☺︎ Snippets

```jsx
import java.util.*;
class Solution {
    public int[][] solution(int[][] arr1, int[][] arr2) {
        int[][] answer = new int[arr1.length][arr1[0].length]; 
        
        for(int i=0; i<arr1.length; i++){
            for(int j=0; j<arr1[i].length; j++){
                answer[i][j] = arr1[i][j]+arr2[i][j];
            }
        }
        return answer;
    }
}
```
---
### ☻ 두 정수 사이의 합

2024년 2월 26일

### **문제 설명**

두 정수 a, b가 주어졌을 때 a와 b 사이에 속한 모든 정수의 합을 리턴하는 함수, solution을 완성하세요.

예를 들어 a = 3, b = 5인 경우, 3 + 4 + 5 = 12이므로 12를 리턴합니다.

### 제한 조건

- a와 b가 같은 경우는 둘 중 아무 수나 리턴하세요.
- a와 b는 -10,000,000 이상 10,000,000 이하인 정수입니다.
- a와 b의 대소관계는 정해져있지 않습니다.

### 입출력 예

| a | b | return |
| --- | --- | --- |
| 3 | 5 | 12 |
| 3 | 3 | 3 |
| 5 | 3 | 12 |

### ☺︎ at

- while 문으로 a ≤ b 까지 범위 지정 후, a ++ 1씩 증가
- sum += 누적 합

### ☺︎ Snippets

```jsx
class Solution {
    public long solution(int a, int b) {
        long answer = 0;
        
        if(a<b){
            while(a<=b){
            answer += a;
            a++;
            }
        } else{
            while(b<=a){
                answer +=b;
                b++;
            }
        }
        
        return answer;
    }
}
```
---

### ☻ 두 정수 사이의 합

2024년 2월 26일

### **문제 설명**

두 정수 a, b가 주어졌을 때 a와 b 사이에 속한 모든 정수의 합을 리턴하는 함수, solution을 완성하세요.

예를 들어 a = 3, b = 5인 경우, 3 + 4 + 5 = 12이므로 12를 리턴합니다.

### 제한 조건

- a와 b가 같은 경우는 둘 중 아무 수나 리턴하세요.
- a와 b는 -10,000,000 이상 10,000,000 이하인 정수입니다.
- a와 b의 대소관계는 정해져있지 않습니다.

### 입출력 예

| a | b | return |
| --- | --- | --- |
| 3 | 5 | 12 |
| 3 | 3 | 3 |
| 5 | 3 | 12 |

### ☺︎ at

- while 문으로 a ≤ b 까지 범위 지정 후, a ++ 1씩 증가
- sum += 누적 합

### ☺︎ Snippets

```jsx
class Solution {
    public long solution(int a, int b) {
        long answer = 0;
        
        if(a<b){
            while(a<=b){
            answer += a;
            a++;
            }
        } else{
            while(b<=a){
                answer +=b;
                b++;
            }
        }
        
        return answer;
    }
}
```

---

### ☻ 콜라츠 추측

2024년 2월 26일

### **문제 설명**

1937년 Collatz란 사람에 의해 제기된 이 추측은, 주어진 수가 1이 될 때까지 다음 작업을 반복하면, 모든 수를 1로 만들 수 있다는 추측입니다. 작업은 다음과 같습니다.

```jsx
1-1. 입력된 수가 짝수라면 2로 나눕니다. 
1-2. 입력된 수가 홀수라면 3을 곱하고 1을 더합니다. 
2. 결과로 나온 수에 같은 작업을 1이 될 때까지 반복합니다.
```

예를 들어, 주어진 수가 6이라면 6 → 3 → 10 → 5 → 16 → 8 → 4 → 2 → 1 이 되어 총 8번 만에 1이 됩니다. 위 작업을 몇 번이나 반복해야 하는지 반환하는 함수, solution을 완성해 주세요. 단, 주어진 수가 1인 경우에는 0을, 작업을 500번 반복할 때까지 1이 되지 않는다면 –1을 반환해 주세요.

### 제한 사항

- 입력된 수, `num`은 1 이상 8,000,000 미만인 정수입니다.

### 입출력 예

| n | result |
| --- | --- |
| 6 | 8 |
| 16 | 4 |
| 626331 | -1 |

### 입출력 예 설명

입출력 예 #1

문제의 설명과 같습니다.

입출력 예 #2

16 → 8 → 4 → 2 → 1 이 되어 총 4번 만에 1이 됩니다.

입출력 예 #3

626331은 500번을 시도해도 1이 되지 못하므로 -1을 리턴해야 합니다.

### ☺︎ Snippets

```jsx
class Solution {
    public int solution(int num) {
        int answer = 0;
        long longNum = (long) num;
        int sum = 0;
        
        while(longNum !=1){
            if(longNum%2==0){
                longNum = isEven(longNum);
                sum++;
            }else{
                longNum = isOdd(longNum);
                sum++;
            }
            if(sum > 500){
                return -1;
            }            
    }
    
         answer = sum;
        return answer;
    }    
    static long isEven(long n ){
        long i = n/2;
        return i;
    }
    static long isOdd(long n ){
        long i = n*3+1;
        return i;
    }
}
```