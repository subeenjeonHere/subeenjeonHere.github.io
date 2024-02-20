---
title: Programmers Lv.0
date: 2024-02-13 
categories: Algorithms
tags:
  [
    Algorithms
    Programmers
  ]
---

# [Programmers](https://school.programmers.co.kr/)


# ☻ 짝수의 합

### **문제 설명**

정수 `n`이 주어질 때, `n`이하의 짝수를 모두 더한 값을 return 하도록 solution 함수를 작성해주세요.

---

### 제한사항

0 < `n` ≤ 1000

---

### 입출력 예

| n | result |
| --- | --- |
| 10 | 30 |
| 4 | 6 |

---

### 입출력 예 설명

입출력 예 #1

- `n`이 10이므로 2 + 4 + 6 + 8 + 10 = 30을 return 합니다.

입출력 예 #2

- `n`이 4이므로 2 + 4 = 6을 return 합니다.

# ☺︎ 코드

```jsx
class Solution {
    public int solution(int n) {

        int sum = 0;        
        for(int i=0; i<=n;i++){
            if(i%2==0){
                sum += i;
            }
        }
        return sum;
    }
}
```

# ☺︎ Solution

1. sum = 0으로 초기화
2. for문으로 i는 0부터 n이하까지 증가하도록 함
    1. 짝수의 합이라는 것은, 2로 나누었을 때 나머지가 0이라는 뜻임. 따라서, 2로 나누었을 때 0인 i만 (0부터 n까지의 원소들) sum에 저장해주면 됨.

---

# ☻ 배열의 평균값

### **문제 설명**

정수 배열 `numbers`가 매개변수로 주어집니다. `numbers`의 원소의 평균값을 return하도록 solution 함수를 완성해주세요.

---

### 제한사항

- 0 ≤ `numbers`의 원소 ≤ 1,000
- 1 ≤ `numbers`의 길이 ≤ 100
- 정답의 소수 부분이 .0 또는 .5인 경우만 입력으로 주어집니다.

---

### 입출력 예

| numbers | result |
| --- | --- |
| [1, 2, 3, 4, 5, 6, 7, 8, 9, 10] | 5.5 |
| [89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99] | 94.0 |

---

### 입출력 예 설명

입출력 예 #1

- `numbers`의 원소들의 평균 값은 5.5입니다.

입출력 예 #2

- `numbers`의 원소들의 평균 값은 94.0입니다.

# ☺︎ 코드

```jsx
class Solution {
    public double solution(int[] numbers) {

        double answer = 0;
        for(int i=0; i<numbers.length; i++){
            answer += numbers[i];
        }
        answer = answer / numbers.length;
        return answer;
    }
}
```

---

# ☻ 피자 나눠먹기 (3)

### **문제 설명**

머쓱이네 피자가게는 피자를 두 조각에서 열 조각까지 원하는 조각 수로 잘라줍니다. 피자 조각 수 `slice`와 피자를 먹는 사람의 수 `n`이 매개변수로 주어질 때, `n`명의 사람이 최소 한 조각 이상 피자를 먹으려면 최소 몇 판의 피자를 시켜야 하는지를 return 하도록 solution 함수를 완성해보세요.

---

### 제한사항

- 2 ≤ `slice` ≤ 10
- 1 ≤ `n` ≤ 100

---

### 입출력 예

| slice | n | result |
| --- | --- | --- |
| 7 | 10 | 2 |
| 4 | 12 | 3 |

---

### 입출력 예 설명

입출력 예 #1

- 10명이 7조각으로 자른 피자를 한 조각 이상씩 먹으려면 최소 2판을 시켜야 합니다.

입출력 예 #2

- 12명이 4조각으로 자른 피자를 한 조각 이상씩 먹으려면 최소 3판을 시켜야 합니다.

# ☺︎ 코드

> 1차 : 실패
>

```jsx
class Solution {
    public int solution(int slice, int n) {
        int answer = 0;
        
        if(n % slice == 0){
            answer = n / slice;
            
        } else if (slice % n >0){
            answer = (int) Math.ceil(n/slice)+1;
        }
        return answer;
    }
```

```jsx
만약, 사람 % 피자 == 0이라면, 사람/피자조각 만큼만 있으면 됨
만약 피자%사람 > 0 이상이면 나머지 만큼의 사람이 피자를 먹지 못 함 -> 사람/피자(몫) 보다 더 많은 피자가 필요함
```

> 2차
>

```jsx
class Solution {
    public int solution(int slice, int n) {
        int answer = 0;
        
        if(n % slice == 0){
            answer = n / slice;
            
        } else{
            answer =n/slice+1;
        }
        return answer;
    }
}
```

나는 왜 저렇게 풀었던 것일까?

1. 단순히 나머지가 > 0이면 남은 사람이 있다는 것이므로 피자가 한 판 더 필요함 그래서 몫에다 +1 해주면 된다.
2. 그리고 그 외에는 남은 사람이 없다는 것이므로 사람/피자조각 수 만큼의 피자만 있으면 되는 것이다.
3. else에선 사람 수와 피자 수가 정확히 나누어 떨어지지 않는 모든 경우를 처리하였다. 이 경우 항상 몫에 1을 더하여 처리하게 된다.
    1. 피자 조각이 사람 수보다 적은 경우 → n/slice + 1, 피자 조각이 사람 수보다 많은 경우 → 최소 1판은 필요함, 피자 조각이 3조각이고 사람이 5명일 때는 정답은 +1 이 됨.

---

# ☻ 문자열 뒤집기

### **문제 설명**

문자열 `my_string`이 매개변수로 주어집니다. `my_string`을 거꾸로 뒤집은 문자열을 return하도록 solution 함수를 완성해주세요.

---

### 제한사항

- 1 ≤ `my_string`의 길이 ≤ 1,000

---

### 입출력 예

| my_string | return |
| --- | --- |
| "jaron" | "noraj" |
| "bread" | "daerb" |

---

### 입출력 예 설명

입출력 예 #1

- `my_string`이 "jaron"이므로 거꾸로 뒤집은 "noraj"를 return합니다.

입출력 예 #2

- `my_string`이 "bread"이므로 거꾸로 뒤집은 "daerb"를 return합니다.

# ☺︎ 코드

```jsx
import java.util.*;

class Solution {
    public String solution(String my_string) {
        
        String str = my_string;
        String[] arr = new String[str.length()];
        
        for(int i=0; i < str.length(); i++){
            arr[i] = String.valueOf(str.charAt(str.length()-i-1));
        }
    
        StringBuffer sb = new StringBuffer();
        for(int i=0; i<arr.length; i++){
            sb.append(arr[i]);
        }
        return sb.toString();
    }
}
```

아까 풀었던 뒤집기 문제 생각해서 풀었다. 다만 더 효율적인 알고리즘이 있을 것이라 예상한다..

```jsx
import java.util.*;

class Solution {
    public String solution(String my_string) {
        
        String str = my_string;
        String answer ="";
        
        for(int i=0; i < str.length(); i++){
            answer += String.valueOf(str.charAt(str.length()-i-1));
        }        
        return answer;
    }
}
```

배열을 선언해주고 또 다시 StringBuffer를 써서 append해줄 필요 없이, String 문자열에 문자열 끝에서부터 시작하는 값을 더해주면 됐다.

---

# ☻ 배열 자르기

### **문제 설명**

정수 배열 `numbers`와 정수 `num1`, `num2`가 매개변수로 주어질 때, `numbers`의 `num1`번 째 인덱스부터 `num2`번째 인덱스까지 자른 정수 배열을 return 하도록 solution 함수를 완성해보세요.

---

### 제한사항

- 2 ≤ `numbers`의 길이 ≤ 30
- 0 ≤ `numbers`의 원소 ≤ 1,000
- 0 ≤`num1` < `num2` < `numbers`의 길이

---

### 입출력 예

| numbers | num1 | num2 | result |
| --- | --- | --- | --- |
| [1, 2, 3, 4, 5] | 1 | 3 | [2, 3, 4] |
| [1, 3, 5] | 1 | 2 | [3, 5] |

---

### 입출력 예 설명

입출력 예 #1

- [1, 2, 3, 4, 5]의 1번째 인덱스 2부터 3번째 인덱스 4 까지 자른 [2, 3, 4]를 return 합니다.

입출력 예 #2

- [1, 3, 5]의 1번째 인덱스 3부터 2번째 인덱스 5까지 자른 [3, 5]를 return 합니다.

# ☺︎ Snippets

> 1차: 시간초과
>

```jsx
class Solution {
    public int[] solution(int[] numbers, int num1, int num2) {
        
        int[] answer = new int[numbers.length - num2 + num1];
        
        int i = 0;
        while(i < num2){
            if(i >= num1 && i <= num2){
                answer[i] = numbers[i-1];
                i++;
            }
        }        
        return answer;
    }
}
```

> 2차: 실패
>

```jsx
class Solution {
    public int[] solution(int[] numbers, int num1, int num2) {
        int[] answer = new int[numbers.length - num2 + num1];
        int m = 0;
        for (int i=num1 ; i<= num2; i++){
            answer[m] = numbers[i];
            m++;
        }
        return answer;
    }
}
```

> 3차
>

```jsx
생각정리
1. for 0~배열 순회
if(i >= num1 && i<=num22)
배열[0]번지부터 저장
```

> 4차 성공
>

```jsx
class Solution {
    public int[] solution(int[] numbers, int num1, int num2) {
        int[] answer = new int[num2 - num1+1];
        int idx = 0;
        for(int i=0; i<numbers.length; i++){
            if(i>=num1 && num2>=i){
                answer[idx] = numbers[i];
                idx++;
            }
        }
        return answer;
    }
}
```

### b/c

> 실패했던 이유
>

예를 들어, numbers 배열이 [1, 2, 3, **4, 5, 6, 7,** 8, 9]이고, num1이 3이며 num2가 6인 경우

numbers 배열에서 인덱스 3부터 6까지의 요소를 자르려고 하면, answer 배열은 [4, 5, 6, 7]이 될 . 이는 4개의 요소를 가지므로 answer 배열의 크기는 4가 되어야 한다.

그러나 현재 코드에서 answer 배열의 크기는 **numbers.length - num2 + num1**로 설정되어 있다. 이 경우, numbers.length는 9이고, num2는 6이며, num1는 3이므로, answer 배열의 크기는 9 - 6 + 3으로 계산되어 6이 된다. 원하는 배열 크기는 4인데, 배열의 크기는 6이 되므로 실패.

> 5차 성공
>

```jsx
class Solution {
    public int[] solution(int[] numbers, int num1, int num2) {
        int[] answer = new int[num2 - num1 + 1];
        int m = 0;
        for (int i=num1 ; i<= num2; i++){
            answer[m] = numbers[i];
            m++;
        }
        return answer;
    }
}
```

굳이 if 조건문을 안 걸어주어도, num1 → num2까지만 원본 배열을 탐색므로 더 효율적일 듯 하다.

---

# ☻ 피자 나눠먹기 (1)

### **문제 설명**

머쓱이네 피자가게는 피자를 일곱 조각으로 잘라 줍니다. 피자를 나눠먹을 사람의 수 `n`이 주어질 때, 모든 사람이 피자를 한 조각 이상 먹기 위해 필요한 피자의 수를 return 하는 solution 함수를 완성해보세요.

---

### 제한사항

- 1 ≤ `n` ≤ 100

---

### 입출력 예

| n | result |
| --- | --- |
| 7 | 1 |
| 1 | 1 |
| 15 | 3 |

# ☺︎ 코드

```jsx
class Solution {
    public int solution(int n) {
        int answer = 1;
        if(n/7 >1){
            answer += (n/7);
        }else{
            answer = answer;
        }
        return answer;
```

```jsx
테스트 1 〉	통과 (0.03ms, 73.9MB)
테스트 2 〉	통과 (0.01ms, 75MB)
테스트 3 〉	통과 (0.01ms, 71.5MB)
테스트 4 〉	통과 (0.01ms, 74.5MB)
테스트 5 〉	실패 (0.02ms, 76.2MB)
테스트 6 〉	실패 (0.02ms, 76.6MB)
테스트 7 〉	통과 (0.02ms, 75.5MB)
테스트 8 〉	통과 (0.02ms, 78.6MB)
테스트 9 〉	통과 (0.02ms, 76.1MB)
테스트 10 〉	통과 (0.02ms, 73.8MB)
테스트 11 〉	통과 (0.02ms, 75MB)
```

자꾸 테스트 5,6 에서 실패한다.

> 2차
>

```jsx
class Solution {
    public int solution(int n) {
        int answer = 0;
        if(n%7==0){
            answer = (n/7);
        }else{
            answer = (n/7)+1;
        }
        return answer;
    }
}
```

2차 시도에 성공했다. 핵심은

나머지가 0으로 나누어 떨어지는지의 여부만 판단하면 됐다.

1. 나머지가 0으로 나눠 떨어지면 7의 배수인 것이고, 피자는 몫 만큼만 필요하면 될 것이다. (1,2,3 → N)
2. 나머지가 0으로 나눠 떨어지지 않는다면, 1판 더 필요할 것이다. 1~6명이라고 쳐도 몫은 0에 +1판, 8~13명이라고 쳐도 몫은 1에 + 1판이다.

간단하게 생각하면 되는 문제다. 1차 코드에서는 몫이 1을 초과할 때 피자 한 판을 추가함. 8~13명일 땐 몫이 1인데, 이 경우 피자를 한 판 더 추가하는 범위에서 벗어남.

```jsx
class Solution {
    public int solution(int n) {
        int answer = 0;
        answer = (n%7==0) ? (n/7) : (n/7)+1;
        return answer;
    }
}
```

삼항 연산자로도 풀이할 수 있다.

---

# ☻ 최댓값 만들기(1)

### **문제 설명**

정수 배열 `numbers`가 매개변수로 주어집니다. `numbers`의 원소 중 두 개를 곱해 만들 수 있는 최댓값을 return하도록 solution 함수를 완성해주세요.

---

### 제한사항

- 0 ≤ `numbers`의 원소 ≤ 10,000
- 2 ≤ `numbers`의 길이 ≤ 100

---

### 입출력 예

| numbers | result |
| --- | --- |
| [1, 2, 3, 4, 5] | 20 |
| [0, 31, 24, 10, 1, 9] | 744 |

## ☺︎ 생각 정리

### 1. 배열 최댓값 메소드

```jsx
class Main {
  public static void main(String[] args) {

    // create an array of int type
    int[] arr = {4, 2, 5, 3, 6};

    // assign first element of array as maximum value
    int max = arr[0];

    for (int i = 1; i < arr.length; i++) {

      // compare all elements with max
      // assign maximum value to max
      max = Math.max(max, arr[i]);

    }

    System.out.println("Maximum Value: " + max);
  }
```

max와 배열의 전체 요소를 비교해서 큰 값에 max를 할당

내가 구해야 할 것은 최댓값 두개이다. 첫 번째 최댓값과 두 번째 최댓값

# ☺︎ Snippets

### 1. for문

```jsx
import java.util.*;
class Solution {
    public int solution(int[] numbers) {
        int answer = 0;
      
        int nmax = numbers[0];
        int smax = numbers[0];
        for(int i=0; i<numbers.length; i++){
            nmax = Math.max(nmax,numbers[i]);
        }
        for(int j=0; j<numbers.length; j++){
            if(numbers[j]!=nmax){
                smax = Math.max(smax,numbers[j]);
            }
        }
        answer = nmax * smax;
        return answer;
    }
}
```

테케 5번 하나 실패 고려해야할 것

- **원소 중복될 경우 ≠ 에서 커트되므로 이걸 수정해야 함**
- 음수 원소로 들어갈 수 없음, 원소의 개수는 두 개 이상

### 2. Arrays.sort

```jsx
Arrays.sort(arr);
answer = arr[arr.length - 1] * arr[arr.length - 2];
System.out.println(answer);
```

1. 배열을 오름차순으로 정렬
2. 배열 마지막 원소와, 그 직전 원소를 곱함 (정렬 이후 오름차순으로 정렬되었을 것이므로)

### 3. Math.max

```jsx
for (int i = 0; i < arr.length; i++) {
            for (int j = 0; j < arr.length; j++) {
                if (i != j) {
                    answer = Math.max(answer, arr[i] * arr[j]);
                }
            }
        }
```

이렇게도 풀 수 있다.

1. (i번째, 0~배열.length) : 배열을 전체 탐색하고 곱한값을 max 에 저장 → i는 배열.length까지 증가 → arr[0~length] * arr[0~length] 값을 전체 다 곱해서 Max를 판별할 수 있다.

---

# ☻ 특정문자제거하기

```jsx
class Solution {
    public String solution(String my_string, String letter) {
        String answer = "";
        answer = my_string.replace(letter,"");
        return answer;
    }
}
```

### 1. String.replace

`string.replace(oldChar, newChar)`

oldChar → newChar 변환

---

# ☻ 아이스 아메리카노

### **문제 설명**

머쓱이는 추운 날에도 아이스 아메리카노만 마십니다. 아이스 아메리카노는 한잔에 5,500원입니다. 머쓱이가 가지고 있는 돈 `money`가 매개변수로 주어질 때, 머쓱이가 최대로 마실 수 있는 아메리카노의 잔 수와 남는 돈을 순서대로 담은 배열을 return 하도록 solution 함수를 완성해보세요.

---

### 제한사항

- 0 < `money` ≤ 1,000,000

---

### 입출력 예

| money | result |
| --- | --- |
| 5,500 | [1, 0] |
| 15,000 | [2, 4000] |

---

# MS

1. 커피는 5500, 머쓱이가 가진 돈 Money → Money/5500
2. 몫은 머쓱이가 마실 수 있는 최대 커피 잔 수, 나머지는 남는 돈

# ☺︎ Snippets

```jsx
class Solution {
    public int[] solution(int money) {
        int[] answer;
        answer = new int[]{money/5500,money%5500};
        
        return answer;
    }
}
```

---

# ☻ 문자반복출력

### **문제 설명**

문자열 `my_string`과 정수 `n`이 매개변수로 주어질 때, `my_string`에 들어있는 각 문자를 `n`만큼 반복한 문자열을 return 하도록 solution 함수를 완성해보세요.

---

### 제한사항

- 2 ≤ `my_string` 길이 ≤ 5
- 2 ≤ `n` ≤ 10
- "my_string"은 영어 대소문자로 이루어져 있습니다.

---

### 입출력 예

| my_string | n | result |
| --- | --- | --- |
| "hello" | 3 | "hhheeellllllooo" |

# ☺︎ Snippets

```jsx
import java.util.*;

class Solution {
    public String solution(String my_string, int n) {
        String answer = "";
        
        StringBuilder sb = new StringBuilder(  );
        for(int i=0; i<my_string.length(); i++){
            int cnt = 0;           
            while(cnt < n ){
                sb.append(my_string.charAt(i));
                cnt++;
            }
        }
        answer = sb.toString();
        return answer;
    }
}
```

1. StringBuilder 쓸 필요 없었다.

---

# ☻ 옷가게 할인받기

### **문제 설명**

머쓱이네 옷가게는 10만 원 이상 사면 5%, 30만 원 이상 사면 10%, 50만 원 이상 사면 20%를 할인해줍니다.

구매한 옷의 가격 `price`가 주어질 때, 지불해야 할 금액을 return 하도록 solution 함수를 완성해보세요.

---

### 제한사항

- 10 ≤ `price` ≤ 1,000,000
    - `price`는 10원 단위로(1의 자리가 0) 주어집니다.
- 소수점 이하를 버린 정수를 return합니다.

---

### 입출력 예

| price | result |
| --- | --- |
| 150,000 | 142,500 |
| 580,000 | 464,000 |

# ☺︎ Snippets

> 1차 실패
>

```jsx
class Solution {
    public int solution(int price) {
        int answer = 0;
        if(price>=500000){
            answer = price-(int)(price*0.2);
        }else if(price>=300000){
            answer =  price-(int)(price*0.1);
        }else if(price>=100000){
            answer =  price-(int)(price*0.05);
        }else{
            answer = price;
        }
        return answer;
    }
}
```

> 반례
>

```jsx
price가 100010일 때 결과값 95010이 출력된다.
```

> 왜?
>
1. 문제에는 100,000 이상일 때 5% 할인되도록 명시되어 있다.
2. 소수점 반환 전 95,009.5 원이 출력되고, 소수점 이하를 버린 최종 반환된 실제 할인된 가격은 **95,009**원이 되어야 한다.
3. `(int)(price*0.05)` 식은 가격*0.05를 부동 소수점 숫자로 계산한 후 반올림한다. 따라서, 최종 금액에 원래 가격에서 뺄셈되기 전 반올림되어 출력이 잘못된다.

> 2차 성공
>

```jsx
class Solution {
    public int solution(int price) {
        int answer = 0;
        if(price>=500000){
            answer = (int)(price*0.2);
        }else if(price>=300000){
            answer = (int)(price*0.1);
        }else if(price>=100000){
            answer =  (int)(price*0.05);
        }else{
            answer = price;
        }
        return answer;
    }
}
```
---

# ☻ 세균 증식

### **문제 설명**

어떤 세균은 1시간에 두배만큼 증식한다고 합니다. 처음 세균의 마리수 `n`과 경과한 시간 `t`가 매개변수로 주어질 때 `t`시간 후 세균의 수를 return하도록 solution 함수를 완성해주세요.

---

### 제한사항

- 1 ≤ `n` ≤ 10
- 1 ≤ `t` ≤ 15

---

### 입출력 예

| n | t | result |
| --- | --- | --- |
| 2 | 10 | 2048 |
| 7 | 15 | 229,376 |

# ☺︎ Snippets

제곱을 사용해서 풀 수 있는 문제다.

```java
class Solution {
    public int solution(int n, int t) {
        int answer = 0;
        answer = n*(int) Math.pow(2,t);
        return answer;
    }
}
```

```jsx
n마리의 세균이 있다. 이 n마리의 세균은 10시간 동안 증식한다고 가정하자.
세균은 1시간에 두 배만큼 증식한다. 여기서 두 마리의 세균은 10시간 동안 증식한다.
따라서, n마리의 세균은 2*t만큼 증식하게 된다.
```
---

# ☻ 모음 제거

2024년 2월 19일

### **문제 설명**

영어에선 a, e, i, o, u 다섯 가지 알파벳을 모음으로 분류합니다. 문자열 `my_string`이 매개변수로 주어질 때 모음을 제거한 문자열을 return하도록 solution 함수를 완성해주세요.

---

### 제한사항

- `my_string`은 소문자와 공백으로 이루어져 있습니다.
- 1 ≤ `my_string`의 길이 ≤ 1,000

---

### 입출력 예

| my_string | result |
| --- | --- |
| "bus" | "bs" |
| "nice to meet you" | "nc t mt y" |

---

### ☺︎ a/t

1. 아스키코드 활용하기
    1. 아스키코드 String으로 변환해서 contains라면 제거
    2. charAt과 같다면, replace

# ☻ Snippets

> 1차
>

```java
class Solution {
    public String solution(String my_string) {
        String answer = "";
        // ASCII 코드
        // a 97, e 101, i 105, o 111, u 117
        int[] arr = {97,101,105,111,117};

        char[] charArr = new char[arr.length];
        for(int j=0; j<arr.length; j++){
            charArr[j] = (char) arr[j];
        }

        for(int i=0; i<charArr.length; i++){
            if(my_string.contains(String.valueOf(charArr[i]))){
                answer = my_string.replace(String.valueOf(charArr[i]),"");
            }
        }
        return answer;
    }
}
```

```java
테스트 1
입력값 〉	"bus"
기댓값 〉	"bs"
실행 결과 〉	테스트를 통과하였습니다.

테스트 2
입력값 〉	"nice to meet you"
기댓값 〉	"nc t mt y"
실행 결과 〉	실행한 결괏값 "nice to meet yo"이 기댓값 "nc t mt y"과 다릅니다.
```

> 실패 원인
>
1. char 배열 길이는 5인 반면, my_string 길이는 이를 초과한다.
2. 그럼 i랑 e는 바뀌었어야 하는데 왜 그대로인지?

> 2차
>

```java
public class Main {
    public static void main(String[] args) {
        int[] arr = {97, 101, 105, 111, 117};
        String str = "nice to meet you";
        char[] chars = new char[arr.length];
        String answer = "";

        for (int i = 0; i < chars.length; i++) {
            int start = 0;

            chars[i] = (char) arr[i];

            while (start < str.length()) {
                if (str.charAt(start) == chars[i]) {
                    char oldChar = str.charAt(start);
                    //aeiou를 삭제하기 위해, 공백을 char 타입으로 만들기
                    char newChar = '\u0000';
                    answer = str.replace(oldChar, newChar);
                    System.out.println("Chat At: " + str.charAt(start));
                    str = answer;
                }
                start++;
            }
            System.out.println(answer);
        }
    }
}
```

> 3차
>

```java
public class Main {
    public static void main(String[] args) {
        String str = "nice to meet you";

        String answer = "";
        answer = str.replaceAll("[aeiou]", "");
        System.out.println(answer);
```

아스키 코드에 꽂혀서 이렇게 풀면 되는거를 1시간 동안 삽질했다.

그냥 `replaceAll()`함수를 사용하여 모음 'a', 'e', 'i', 'o', 'u'를 한 번에 제거하면 된다. **정규 표현식에 매칭되는 문자열을 두 번째 인자로 주어진 “”로 대체하면 된다.**

하지만, `String`과 `Char`의 차이에 대해 다시 짚고 넘어갈 수 있었다.

---

### ⚒️ Methods

### String replace()

**public** String **replace**(**char** oldCharacter, **char** newCharacter)

| 메소드 | 설명 |
| --- | --- |
| public String replace(char oldCharacter, char newCharacter) | oldCharacter가 이 문자열에서 모두 newCharacter로 바뀐 새로운 문자열을 반환한다. |

### String replaceAll()

| 메소드 | 정의 |
| --- | --- |
| replaceAll(String regex, String replacement) | 주어진 정규 표현식과 일치하는 이 문자열의 각 부분 문자열을 주어진 대체 문자열로 교체한다. |
| regex | 이 문자열과 일치시킬 정규 표현식이다. |
| replacement | 각 일치 항목에 대체할 문자열이다. |

### String charAt()

**public** char **charAt(int** index)

| 메소드 | 설명 |
| --- | --- |
| public char charAt(int index) | 이 문자열에서 지정된 위치의 char 값을 반환한다. 인덱싱은 0부터 시작한다. index 인자가 음수이거나 이 문자열의 길이보다 크거나 같을 경우 IndexOutOfBoundsException을 발생 |

---

# ☻ 짝수는 싫어요

2024년 2월 19일

### **문제 설명**

정수 `n`이 매개변수로 주어질 때, `n` 이하의 홀수가 오름차순으로 담긴 배열을 return하도록 solution 함수를 완성해주세요.

---

### 제한사항

- 1 ≤ `n` ≤ 100

---

### 입출력 예

| n | result |
| --- | --- |
| 10 | [1, 3, 5, 7, 9] |
| 15 | [1, 3, 5, 7, 9, 11, 13, 15] |

---

# ☺︎ a/t

1. start = 1; 로 설정하고 %2 != 0일 때를 고려하여 배열에 추가하면, 1, 0, 3, 0 이런 식으로 배열에 추가되는 문제점
2. ArrayList를 활용하기

# ☺︎ Snippets

```java
import java.util.*;
class Solution {
    public ArrayList<Integer> solution(int n) {
        
        ArrayList<Integer> arrst = new ArrayList<>();
        
        int start = 1;
        while(start<=n){
            if( start %2 == 1){
                arrst.add(start);
            }
            start ++;
        }
        return arrst;
    }
}
```

### 해결

1차원 배열을 사용하고, i 값을 하나씩 카운팅하여 일치하면 배열에 저장할 경우 1, 0, 3 이런 식으로 저장되는 문제점이 있었다.

ArrayList는 필요에 따라 배열 크기를 동적으로 늘리거나 줄일 수 있으므로, 얼만큼의 요소를 저장할 지 모를 때 특히 유용하다.

특히 표준 배열을 사용했다면 정확한 크기를 알아야 했고, 요소들 사이에 빈 인덱스가 생길 수도 있었는데 ArrayLIst를 사용해서 해결하였다.

---

# ☻ 순서쌍의 개수

2024년 2월 20일

### **문제 설명**

순서쌍이란 두 개의 숫자를 순서를 정하여 짝지어 나타낸 쌍으로 (a, b)로 표기합니다. 자연수 `n`이 매개변수로 주어질 때 두 숫자의 곱이 `n`인 자연수 순서쌍의 개수를 return하도록 solution 함수를 완성해주세요.

---

### 제한사항

- 1 ≤ n ≤ 1,000,000

---

### 입출력 예

| n | result |
| --- | --- |
| 20 | 6 |
| 100 | 9 |

---

# ☺︎ Snippets

```java
class Solution {
    public int solution(int n) {
        int answer = 0;
        
        for(int i=1; i<=n; i++){
            if(n % i == 0){
                answer++;
            }
        }
        return answer;
    }
}
```

1. 처음에 ArrayList를 생성하고, 저장한 이후 중첩 For문을 생성했었다. 그리고 각 원소들을 전체 비교한 후 n과 일치한다면 카운트를 증가시켰다.
2. 전혀 그럴 필요가 없었다. 애초에 **두 숫 자의 곱이 n이 된다**는 것은 n의 약수임을 의미한다. 따라서 약수의 개수만 구해주면 되었다. 왜 굳이 복잡하게 생각했는지..

### 약수란

어떤 수를 나누어 떨어지게 하는 수를 의미한다. 예를 들어, 숫자 6의 약수는 1, 2, 3, 6인 것 처럼..

### 다른

이렇게 삼항 연산자로도 풀 수 있다.

```java
class Solution {
    public int solution(int n) {
        int answer = 0;
        int start = 1;
        int count = 0;
        while(start <=n){
            answer = (n%start ==0) ? count++ : 1;
            start++;
        }
        answer = count;
        return answer;
    }
}
```

---

