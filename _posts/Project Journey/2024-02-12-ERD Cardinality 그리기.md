---
title: ERD Cardinality 그리기 (4)
date: 2024-02-12 09:28:00 +09:00
categories: ProjectJourney
tags:
  [
    ProjectJourney
  ]
---

# ☻ 테이블 N:M 관계 나타내기
본격적으로 ERD를 생성해야 한다. 

### ERD , 개체, 속성의 개념

- ERD(Entity Relationship Diagram): 데이터의 논리적인 구조와 그 관계를 도식화한 것이다.  테이블 간의 관계를 명확하게 표현하여 데이터베이스 설계에 필요한 정보를 제공한다.
- 개체(Entity): 데이터베이스에서 정보를 저장하려는 실세계의 객체나 개념이다. 예를 들어, '학생', '교수', '과목' 등이 될 수 있다.
- 속성(Attribute): 개체를 설명하는 데 사용되는 특성이나 성질이다. 예를 들어, '학생' 개체의 속성은 '학번', '이름', '전공' 등이 될 수 있다.

즉, 테이블로 개체를 표현할 수 있으며, 개체의 인스턴스나 이벤트를 저장한다. 이후 컬럼으로 그 개체의 속성을 표현할 수 있다.
<br>

---


### ERD 관계의 종류

### 1. 관계 선

- 실선 : 식별 관계
    - 부모 자식 관계에서 자식이 부모의 키를 외래키로 참조한다.
- 점선 : 비식별 관계
    - 부모 자식 관계에서 자식이 부모의 키를 일반 속성으로 참조한다.

### 2. 카디널리티 (Cardinality)

카디널리티(Cardinality)는 ERD에서 개체(Entity)간 관계의 수를 나타내는 개념이다. 예를 들어, '1:1', '1:N', 'N:1', 'N:M' 등과 같은 형태로 표현되며, 이는 각 개체가 가질 수 있는 관계의 수를 나타낸다. 예를 들어, '1:1'은 한 개체가 다른 한 개체와만 관계를 가질 수 있다는 것을 의미하며, '1:N'은 한 개체가 다른 여러 개체와 관계를 가질 수 있다는 것을 의미한다.

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/61d5abc0-6cb0-409f-9b68-3442e4e8c10d)

### 1. One -to One

'학생 ↔ 강좌' 관계를 예로 들 수 있다. 한 학생이 하나의 특정 강좌에만 등록할 수 있고, 각 강좌도 한 학생에게만 속할 수 있다. 이러한 관계를 1:1 관계라고 한다. 또한, 사람 ↔ 여권 관계를 생각해봐도 좋다.
![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/f1553413-1a6c-47a9-8346-b2d5c2790e25)

### 2. One-to Many

학생 ↔ 수업 관계를 예로 들어보자. 하나의 클래스를 기준으로, 하나의 클래스는 여러 학생에게 등록될 수 있다. 이러한 관계를 1:N이라고 한다. 하나의 개체가 여러 개체를 관리하거나 지원하는 구조이다.
![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/831204af-e683-4462-aa30-2079daae2136)

### 3. Many-to-One

여러 학생이 하나의 클래스에 등록될 수 있다. 여러 개체가 하나의 개체에 종속되는 구조이다.
![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/9f4e78e4-6b1b-4997-a807-6d5d5f0caf55)

### 4. Many-to-Many

학생과 교수진의 관계를 예시로 들 수 있다.여러 학생이 여러 교수진들과 관련이 있으며, 동시에 하나의 학생도 여러 교수진과 관련이 있을 수 있다. 간단히 설명하면, 학생은 여러 교수에게 수업을 듣고 교수는 여러 학생에게 강의한다.
![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/5fb789f8-319d-4ca7-8767-f46c8f339036)

---

## ☺︎ ERD를 그려보자

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/d601dac4-378b-46b3-bc30-71734297340a)

---

## ◼︎ 게시판 - 댓글 - 알림 테이블

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/3037df05-3bee-4771-bea2-c1d6bd902472)

1. **게시판(board) 테이블과 댓글(board_comment) 테이블 간의 0:N 관계:**
    - 게시글은 하나 무조건 존재하고, 댓글은 게시글에 포함된다.
    - 하나의 게시글에 0 ~ N개 달릴 수 있다.
    - 따라서, 두 테이블의 관계를 Zero to Many로 설정한다.
2. **댓글(board_comment) 테이블과 알림(board_notification) 테이블 간의 0:N 관계:**
    - 하나의 댓글이 여러 개의 알림을 가질 수 있지만, 하나의 알림은 특정 댓글에 종속된다. 즉, 하나의 댓글은 0~N개의 알림을 가질 수 있다.
    - 따라서, 두 테이블 관의 관계를 Zero to Many로 설정한다.

---

## ◼︎ 회원 - 회원이용약관중개 - 이용약관 테이블
![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/e34f699f-490d-46c4-a380-d5ffc61dade6)

1. 회원 ↔ 회원이용약관 중개 테이블간의 One-to-Many 관계:
    1. 하나의 회원이 여러 이용 약관에 동의할 수 있다.
2. 이용약관 ↔ 회원이용약관중개 테이블간의 Many-to-One 관계:
    1. 여러 약관 동의가 하나의 약관 조건과 연결될 수 있다.

---

## ◼︎ 티켓오픈공지사항 - 회원테이블

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/bf885a41-17d1-4c90-abe1-5004ce32d650)
한 명의 관리자 회원이 여러 개의 티켓 오픈 공지사항을 작성할 수 있지만, 작성하지 않을 수도 있다. 공지사항의 관점에서 보면, 각 공지사항은 한 명의 관리자 회원에 의해 작성된다.

티켓 오픈 공지사항과 회원 테이블 간의 Zero-to-Many 관계가 형성된다.

---

## ◼︎ 회원 - 구단 테이블

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/5fbf9ab5-9992-4452-9494-1671c081fa72)
회원 한 명은 하나의 응원 구단을 선택하거나, 선택하지 않을 수 있다. 또한, 구단은 여러 회원을 가질 수 있다. 

따라서 Zero to Many 관계를 가진다.


---

## ◼︎ 회원 - 예약 테이블

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/39b6653c-5ec9-48f6-9fc7-a10a04d786d1)

회원 한 명은 한 장의 티켓을 예약 할 수 있다. 또한, 특정 예약은 한 명의 회원에게 소유된다.

예약 테이블에 데이터가 생성되는 경우는 회원이 예약을 진행했을 때이므로, 회원과 예약 테이블간의 관계는 One-to-One 관계를 갖는다.

---
```
중략..
```
---

## ERD

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/a257ed36-1a27-4b94-ba57-7a684cd5a136)

