---
title: 🚀 Planet Project 트러블 슈팅 및 에러 정리
date: 2024-03-14
categories: ProjectJourney
tags:
  [
    ProjectJourney
  ]
---

# TOC

<!-- TOC -->
* [1. JPA 테이블 연관관계 매핑 에러](#1-jpa-테이블-연관관계-매핑-에러)
* [2. 외래 키 제약조건](#2-외래-키-제약조건)
* [3. Spring이 내 Entity를 완전히 무시할 때](#3-spring이-내-entity를-완전히-무시할-때)
* [4. 올바른 정보로 로그인 해도 에러가 발생한다.](#4-올바른-정보로-로그인-해도-에러가-발생한다)
* [5. JPA Find 메소드의 쿼리 실행 결과가 1개 이상일 때](#5-jpa-find-메소드의-쿼리-실행-결과가-1개-이상일-때)
* [6. Spring Security 403 에러](#6-spring-security-403-에러)
* [7. OAuth2 로그인](#7-oauth2-로그인)
<!-- TOC -->

---

> 비교적 기본적인 부분을 놓친 것들이기에 부끄럽지만, 발생했던 에러들 중 몇개를 정리해 두었다.

---

### 로켓 예약 기능 구현 시

# 1. JPA 테이블 연관관계 매핑 에러

기존엔 단일 테이블로만 JPA를 사용했다. 이 예약 기능을 구현하며 3~4개의 테이블을 매핑해야 했는데, 초기에 테이블 간 연관관계 매핑이 제대로 설정되지 않았던 것이 원인으로 NPE 에러를 많이 겪었다.

## 문제

예약 정보를 생성하는 과정에서 'User' 엔티티와 'Reservation' 엔티티 간의 매핑 문제로 인해 'Reservation' 엔티티에서 'User' 엔티티의 정보를 제대로 조회하지 못해 발생했던 에러이다.

‘User’가 null이기 때문에 사용자의 예약을 진행할 수 없었다.

---

## 상황

### User

```jsx
/**
*User : Reservation = 1:N
*/
@OneToMany(mappedBy = "user", cascade = CascadeType.ALL)
private List<Reservation> reservationList;
```

### Reservation

```jsx
@ManyToOne(fetch = FetchType.EAGER)
@JoinColumn(name = "user_id",referencedColumnName = "user_id")
public User user;
```

### ReservationRepository

> 예약 진행을 위해, 이메일로 User 테이블에서 사용자를 조회하는 메소드
>

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/3402e3c9-31a9-44c4-95d7-95543c390437)

### ERD

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/2f32636a-660c-490d-8877-4f5bb914129a)


---

## 해결 과정

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/4bac4555-1c4d-4ecb-91ed-d308a9c4ff53)
### 발생했던 에러 로그

```java
Cannot invoke "org.demo.study.entity.User.getEmail()" because "this.user" is null
```

```java
Unknown column 'r1_0.email' in 'field list’
```

```java
Unknown column 'email' in 'field list’
```

해당 에러들은 공통적으로 Reservation 엔티티에서 User 엔티티가 Unknown, Null임을 말해주고 있었다.

1. 'User' 엔티티가 null이기 때문에 'getEmail()' 메소드를 호출할 수 없다.
2. 'email'이라는 컬럼을 찾을 수 없다.

각 엔티티에 연관관계 매핑을 완료했다고 생각했기에, DB에 문제가 있는 건 아닐까 했고, 스택 오버 플로우에 검색을 해봤다.

다시 처음부터 생각해봤다.

그냥, Resrvation 테이블과 User 테이블이 조인이 잘못되었다는 거다. 그럼 JPA에서는 테이블 간의 연관관계 매핑이 안 되어있다는 것이다.

그리고 테이블 연관관계 매핑 강의를 다시 봤고, 뭐가 문제였는지 알 수 있었다.

User 테이블과 Reservation 테이블은 **User**에게 여러 **Reservation**이 있는 1:N 관계를 가지고 있다. 그럼 예약을 위해 Reservation 객체를 만들 때, 해당 객체가 **어떤 User에게 속하는지를 별도로 지정**해주어야 했다.

즉, 테이블 연관관계를 매핑함에 있어 기본적인 절차를 놓쳤던 것이다.

```java
// 연관관계 set
public void setUser(User users) {
    this.users = users;
}
```

setUser() 메소드를 사용해 어떤 User에게 해당 Reservation이 속하는지를 지정해주어야 했다.

이를 통해 생성된 Reservation 객체는 User의 이메일을 조회할 때 하나의 객체로 작동한다.

---

## 2. 문제

```java
"Not-null 속성이 null이거나 일시적인 값을 참조합니다 : Reservation.user"
```

### 상황

위와 동일하게, 테이블 간의 연관관계 매핑이 제대로 되지 않아 발생했던 `NullPointerException` 에러이다.

예약 정보를 Reservation 파라미터로 전달하면서, 해당 예약 정보에는 출발 행성(departurePlanet), 도착 행성(arrivalPlanet), 사용자 정보(user), 그리고 기타 예약과 관련된 필드들이 매핑되어야 했다.

이것도 setUser(User user) 로 연관관계 매핑을 다시 해주고 해결하였다.

---

### 게시판 CRUD 구현 시

# 2. 외래 키 제약조건

![My Gif](https://subeenjeonHere.github.io/assets/e1.gif)

```jsx
Cannot delete or update a parent row: a foreign key constraint fails (`board`.`comment`, CONSTRAINT `comment_FK` FOREIGN KEY (`board_id`) REFERENCES `board` (`board_id`))
```

### 상황

board 테이블의 1454번 게시글을 삭제하려고 하였다.

### 원인

구글에 Cannot delete or update a parent row: 를 검색해보았고, 많은 도움을 받는 스택 오버플로우에서 같은 에러들과 그 내용들을 봤다. 이 에러는 외래 키 제약조건 때문에 발생한다.

내 상황에선, 'board' 테이블의 특정 레코드(게시글)를 삭제하려 했을 때, 이를 참조하는 'comment' 테이블의 외래 키 제약조건을 위반해서 발생했다. 즉, 'comment' 테이블에서 해당 게시글을 참조하는 레코드가 존재하는데, 그 게시글을 삭제하려 해서 에러가 발생한 것이다.

### 왜 이 에러를 이제 발견했는가

1456번 게시글은 해당 게시글을 참조하는 레코드가 없어서 삭제가 되었는데, 1454번 게시글은 댓글이 있어서 (해당 게시글Id를 참조하는 레코드) 삭제가 불가능했던 것이다.

### 해결법

`CASECADE` 옵션을 추가한다. CRUD 게시판에서 해당 게시글이 삭제되면, 댓글도 자동으로 삭제되는 것이 일반적이다. 따라서 CASECADE 옵션을 추가한다. 데이터 베이스 설계 이후 바로 개발을 진행하면서 이런 세세한 부분들을 놓쳤다.

```sql
ALTER TABLE comment 
ADD CONSTRAINT board_id FOREIGN KEY (board_id)
REFERENCES board(board_id) ON DELETE CASCADE;
```

### 결과

![My Gif](https://subeenjeonHere.github.io/assets/e2.gif)

위의 CASECADE 제약 조건을 추가하는 SQL문을 실행한 결과, 게시글이 삭제될 때 해당 게시글을 참조하고 있는 댓글들도 함께 자동으로 삭제되는 것을 확인할 수 있었다. 이로써 외래 키 제약조건으로 인한 문제는 해결되었다.


> 막상 해결하고 보니 기술적인 부분은 아니었고, 정말 간단한 에러였지만 시간을 많이 소요해서 기록해두었다..


---

# 3. Spring이 내 Entity를 완전히 무시할 때

### 상황

| 요약 |
| --- |
| 브라우저 상의 구현한 CRUD기능은 정상적으로 동작하나, 정작 가장 중요한 데이터 베이스에 값이 C,R,U,D 되지 않는다. |
| 회원가입도 정상적으로 처리되나, 막상 user 테이블엔 값이 들어오지 않는다. |

### 데이터 베이스 내용과

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/986e09d1-fa89-446c-aeda-1f97291f6e4b)

### 데이터를 불러올 화면

```java
<tr th:each="board: ${boardlist}">
                <td th:text="${board.boardId}"></td>
                <td>
                    <a th:text="${board.title}" th:href="@{/board/view/{id}(id=${board.boardId})}"></a>
                </td>
                <td th:text="${board.hit}"></td>
            </tr>
            <tr th:if="${#lists.isEmpty(boardlist)}">
                <td colspan="3">게시글이 없습니다.</td>
            </tr>
            </tbody>
```

### 게시글 목록을 저장할 List가 isEmpty 라고 한다. “게시글이 없습니다.”

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/8c285b12-78fa-41bb-926c-ff5cd2eaa9fa)

### 게시글 목록 데이터를 불러올 코드 내용

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/44cd13d9-9396-49f6-80c0-78384341e4f1)

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/99c09f18-faf1-47ab-b87c-c35baf53809d)

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/136d4be9-abce-4d4d-8a86-8da315a2936e)

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/7a07c5fc-42f0-437d-b3d9-ebde031a8166)


### 에러 로그는

```java
Exception evaluating SpringEL expression: "board.title" (template: "/boardview" - line 33, col 44)] with root cause
```

"board.title"이 존재하지 않아 발생하는 문제인데, 즉 board 객체가 null 이라는 것이다.

### 원인

정상적으로 게시글 데이터를 불러오다가, 갑자기 데이터를 읽어오지 못 하기에 수만가지 생각은 다 해본 것 같다. IntelliJ 버그인가? DBeaver가 어디가 이상한가?

JPA가 데이터를 불러오고, 여기에 `Entity`나 `Repository`가 데이터 베이스랑 직접적으로 상호작용 하기에 이 둘을 중점적으로 봤다.


### 시도한 것

```java
jpa:
    show-sql: true
    properties:
      hibernate:
        format_sql: true

List<Board> findAllWithUsers; //조인관계 고려하여

CRUDrepository<Board,Long>
```

### 이 어노테이션을 추가하고 갑자기 데이터가 불러와진다.

```java
@SpringBootApplication
@ComponentScan(basePackages = {"org.demo.study.controller", "org.demo.study.service"})
@EnableJpaRepositories(basePackages = {"org.demo.study.repository"})
@EntityScan(basePackages = {"org.demo.study.entity"})
public class StudyApplication {
```

즉, `@EntityScan` 어노테이션은 JPA가 Entity 클래스를 찾기 위한 경로를 지정해주는 역할을 한다. 따라서, 메인 어플리케이션에 `@EntityScan`을 추가하고, 검색 경로(`basePackages`)를 Entity 클래스가 위치한 패키지로 지정해주었다. 그리고 `@EnableJpaRepositories` 어노테이션은 JPA가 Repository 인터페이스를 인식하기 위한 어노테이션이라고 한다. 이 어노테이션도 메인 어플리케이션에 추가했다.

---

### Spring Security 회원가입/로그인 구현 시

# 4. 올바른 정보로 로그인 해도 에러가 발생한다.

![My Gif](https://subeenjeonHere.github.io/assets/e3.gif)

### 원인

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/3c4b8aaf-ee0a-46f0-bffb-66534b9a5453)

스프링 공식 문서를 한참 들여다 보고 나서야 알았다. 스프링 시큐리티는 로그인 시에 기본적으로 'username'과 'password'라는 파라미터 이름을 사용한다. 나는 email로 로그인을 처리를 했기에 에러가
발생했던 것이다.

### 해결법

만약 유저 네임이 아닌, 다른 필드로 로그인을 처리할 것이라면, Security 설정에서 usernameParameter를 사용하여 파라미터 이름을 email로 커스터 마이징 해줘야 한다. 이렇게 설정하지 않을 경우
username 파라미터를 찾지 못해 에러가 발생한다.

---


# 5. JPA Find 메소드의 쿼리 실행 결과가 1개 이상일 때

### 상황

![My Gif](https://subeenjeonHere.github.io/assets/14m6.gif)

## 문제

```jsx
Query did not return a unique result: 7 results were returned
```

테스트를 위해 생성한 `test@01` 이라는 email 속성을 가진 데이터가 7개 있다고 한다.

![My Gif](https://subeenjeonHere.github.io/assets/14m7.gif)

물론 데이터 베이스에 없는 값으로 가입을 시도하면 가입은 완료되는 상황이다.

## 문제 발생 코드

```jsx
@Override
    public boolean save(User user) {
        logger.info("회원가입 호출");
        d
        //이미 등록된 이메일인지 확인
        if (existingUser.isEmpty()) {
            userRepository.save(user);
            return true;
        } else {
            return false;
        }
    }
```

## 원인

바로 스택 오버플로우에 검색을 해봤고, 원인은 코드에 있었다. **JPA**에서 **Find**메소드를 사용했을 때, 반환값이 여러 개인데 그 반환값을 받아주는 **객체가 하나일 때** 발생하는 오류라고 한다.

### JPA Find 메소드

실은 이해가 안돼서 검색을 더 해보니, **기본적으로 JPA Find 메소드는 하나의 결과를 반환하도록 설계**되어 있다고 한다.

### 왜 하나만 반환하도록 설계 되어있는 건가?

JPA 객체 = 관계 매핑에 의해 JPA는 데이터 베이스의 행을 자바 객체에 매핑하기 때문에, 하나의 행은 하나의 객체를 나타낸다.

F**ind 메소드를 사용해서 조회를 할 때는 하나의 행, 즉 하나의 객체만을 반환** 받는 것이 원칙이다. 따라서 하나 이상의 결과가 반환되면 `NonUniqueResultException`이 발생한다.

이전에 ‘test@01’ 이메일로 테스트를 위해 여러 번 가입한 상황이었기에, `userRepository.findByEmail(user.getEmail())`해당 쿼리가 이미 여러 개의 결과를 반환하여 에러가 발생했던 것이다.

## 해결 방법

### UserServiceImpl

처음에는 이상한 걸 알면서도 마음이 급해서 이런 식으로 코드를 수정 했었다.

```jsx
@Override
    public boolean save(User user) {
        logger.info("회원가입 호출");
        String newemail = user.getEmail();
        //이미 가입된 이메일인지 확인
        long existinguser = userRepository.countByEmail(newemail);
        if (existinguser == 0) {
            return true;
        } else {
            return false;
        }
    }
```

이렇게 하니까 해결은 됐다.

![My Gif](https://subeenjeonHere.github.io/assets/14m8.gif)

### 다시 수정하기

그리고 다시 구글링을 더 해봤다. 이 경우 List 형태로 반환받으면, 같은 행을 여러 개 반환 할지라도 모두 리스트에 담겨서 반환되어 `NonUniqueResultException` 에러를 피할 수 있다고 한다. 이후에 해당 리스트의 크기를 체크해주는 방식으로 해결할 수 있다.

> 일단 코드를 짜봤는데 어떻게 처리해야할지 잘 모르겠다. 삼촌이나 선생님께 여쭤 봐야할 것 같아서 일단 기록하고 수정
>

```java
    /**
     * @param registrationRequest
     * @return
     * @Feature: Processing registration
     */
    @Override
    public List<RegistrationRequest> register(RegistrationRequest registrationRequest) {

//        User user = new User();
        User user = userRepository.findByEmail(registrationRequest.getEmail());
        List<RegistrationRequest> registrationRequestList = new ArrayList<>();

        if (user == null) {
            BeanUtils.copyProperties(registrationRequest, user);
            user.setPassword(bCryptPasswordEncoder.encode(registrationRequest.getPassword()));
            registrationRequestList = userRepository.saveUser(user);

        }
        return registrationRequestList;
    }
```

```java
    @Override
    public List<RegistrationRequest> register(RegistrationRequest registrationRequest) {

        Optional<User> user = userRepository.findUserByEmail(registrationRequest.getEmail());
        User findUser = userRepository.findByEmail(registrationRequest.getEmail());

        List<RegistrationRequest> registrationRequestList = new ArrayList<>();

        if (user.isEmpty()) {
            findUser.setPassword(bCryptPasswordEncoder.encode(registrationRequest.getPassword()));
            findUser.setEmail(registrationRequest.getEmail());
            findUser.setPhoneNumber(registrationRequest.getPhoneNumber());
            findUser.setPassportNumber(registrationRequest.getPassportNumber());
            findUser.setBirtDate(registrationRequest.getBirthdate());
//            BeanUtils.copyProperties(registrationRequest, findUser);
            userRepository.save(findUser);
        }
        return registrationRequestList;
    }
```

---


# 6. Spring Security 403 에러

![My Gif](https://subeenjeonHere.github.io/assets/14m9.gif)

## 원인

스프링 시큐리티의 403 에러는 주로 **접근 권한이 없는 사용자가** 특정 리소스에 접근하려 할 때 발생한다.

### 예를 들면

| 원인 | 설명 |
| --- | --- |
| 부적절한 권한 설정 | 스프링 시큐리티는 권한 기반의 접근 제어를 제공한다. 특정 URL이나 메소드에 대한 접근 권한이 잘못 설정되었을 때 403 에러가 발생한다. |
| 인증 실패 | 사용자가 올바르게 인증되지 않았을 경우에도 403 에러가 발생할 수 있다. 이는 사용자가 로그인하지 않았거나, 세션이 만료되었거나, 인증 토큰이 유효하지 않을 때 발생한다. |
| CSRF 토큰 문제 | 스프링 시큐리티는 CSRF 공격을 방어하기 위해 CSRF 토큰을 사용한다. 요청에 CSRF 토큰이 포함되지 않았거나, 토큰이 유효하지 않으면 403 에러가 발생한다. |

> Security는 공부하고 써야할 것 같다. 왜냐면 접근 자체를 못 하니까.
>

## 해결 방법

### 1. FilterChain

FilterChain에서`authorizeHttpRequests` 메소드를 사용하여 보안이 필요한 경로를 설정하고, `permitAll` 메소드를 사용하여 모든 사용자가 접근할 수 있는 경로를 설정해야 한다.

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/30d1b768-5872-4064-a505-f9c0859b6a6a)

```java
//어떤 경로가 보안되어야하고, 아닌지 결정
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
                /**
                 * 커스텀 필터 체인
                 */
                .authorizeHttpRequests(authrizerequests -> authrizerequests
                        .requestMatchers("/user/register", "/user/registerpro", "/user/login", "/user/registerpro")
                        .permitAll()
                )
                .httpBasic(Customizer.withDefaults())
                .formLogin((form) -> form
                        .loginPage("/user/login")//로그인 페이지가 있고, permitAll로 모두 접근 가능
                )
                .logout((logout) -> logout.permitAll())
                .csrf(AbstractHttpConfigurer::disable)
                .cors(AbstractHttpConfigurer::disable);

        return http.build();
    }
```

### 2. HTML

```sql
<head>
    <meta name="_csrf" th:content="${_csrf.token}"/>
    <meta name="_csrf_header" th:content="${_csrf.headerName}"/>
</head>
```

HTML에 CSRF 토큰 추가: HTML head 태그 내에 meta 태그를 추가하여 CSRF 토큰과 헤더 이름을 설정한다.

```sql
.csrf(AbstractHttpConfigurer::disable)
```

나는 Security FilterChain에 CSRF를 비활성화 해주는 방법으로 해결했다.

## 해결

![My Gif](https://subeenjeonHere.github.io/assets/14m11.gif)


### 근데 왜 CSRF 비활성화 처리를 별도로 해주어야 하는건가

스프링 시큐리티의 CSRF (Cross-Site Request Forgery) 보호 기능은 애플리케이션을 특정 유형의 공격으로부터 보호한다.

이 기능이 활성화되어 있으면, 스프링 시큐리티는 요청에 CSRF 토큰이 포함되어 있는지 확인하고, 토큰이 없거나 유효하지 않으면 403 에러를 반환하게 되어있다.

현재 프로젝트에선 CSRF 공격이 필요하지 않아, 비활성화했다.

### 보안 목적으로 이 기능이 있는건데 이걸 비활성화 해도 되는건가

배포하거나 실제 운영 환경에서는 CSRF 보호 기능을 활성화해야 하는 게 아닌가 궁금해서 검색을 해봤다. 보호 기능을 활성화 해야하고 클라이언트 측 요청에선 CSRF 토큰을 포함시키는 로직을 구현해야 한다.

---

### 구글 OAuth2 로그인 구현 시

# 7. OAuth2 로그인

## 문제

구글 로그인 이후, 리디렉션 실패

### 1. OAuth2 로그인 창으로 리디렉션

```java
Redirecting to https://accounts.google.com/o/oauth2/v2/auth?response_type=code&client_id=*****.apps.googleusercontent.com&scope=email%20profile&state=*****&redirect_uri=http://localhost:8000/login/oauth2/code/google
```

### 2. 사용자가 Google에서 돌아왔을 때 로그

```java
Securing GET /login/oauth2/code/google?state=*****&code=*****&scope=email+profile+openid+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.profile+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email&authuser=0&prompt=consent
```

| 키워드 | 설명 |
| --- | --- |
| 'Securing' | Spring Security가 경로 및 요청에 대한 보안 처리를 수행하고 있다는 것을 의미 |
| 'state' | OAuth2 흐름에서 사용자 상태를 나타내는 매개변수로, 유효성 검사에 사용 |
| 'code' | Google에서 반환한 OAuth2 인증 코드 |
| 'scope' | 사용자에 대한 요청된 권한 범위를 나타냄 |
| 'authuser' | 인증된 사용자의 ID |
| 'prompt' | 사용자에게 동의를 요청할 지 여부를 나타내는 매개변수 |

### 3. 인증 예외 발생 이후 로그

OAuth2 인증 예외가 발생하고, 사용자를 로그인 페이지로 리디렉션하는 부분

```java
No event was found for the exception org.springframework.security.oauth2.core.OAuth2AuthenticationException
Redirecting to /user/login?error
```

### 4. 로그인 페이지로 리디렉션 및 로그

로그인 페이지로의 리디렉션 및 해당 페이지에서 로그인 폼을 표시하는 부분

```java
Securing GET /user/login?error
Set SecurityContextHolder to anonymous SecurityContext
Secured GET /user/login?error
Login form
```

## 원인

Google OAuth2 인증 과정에서 발생하는 `OAuth2AuthenticationException`라는 예외

```java
OAuth2AuthenticationException
```

## 해결

### 1. 승인된 자바스크립트 원본 경로

Google 콘솔에 승인된 자바스크립트 원본에 필요한 경로가 누락되어 있었다.

```java
http://localhost:8000
```

---

### 2. 유저 정보를 가져오는 데 성공 이후, 데이터 베이스 주입 시 에러

유저 정보를 성공적으로 가져온 후 데이터베이스에 주입하려 할 때 발생하는 에러. 특히 'phonenumber' 컬럼이 null일 수 없다는 에러

Google OAuth2 로그인 시, 번호 정보를 제공하지 않는다. 따라서 데이터 베이스 phonenumber 컬럼을 Null 허용으로 변경했다.

```java
Column 'phonenumber' cannot be null
[insert into users (birth_date,email,enabled,locked,passportnumber,password,phonenumber,provider,provider_id,role,username)
```

### 2.2 OAuth2 로그인으로 가져올 수 있는 프로필 정보

Google은 기본적인 사용자 정보(이메일 주소, 프로필 이미지, 사용자 식별자 등)를 제공함. 그러나 Google은 휴대전화 번호를 기본적인 사용자 정보로 제공하지 않음.

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/21a5b5a4-de47-48c3-8f3b-cc4eef13026f)
### 휴대폰 번호가 Not Null이어야 한다면 어떻게 해야하는가

개인 프로젝트라서 휴대폰 번호를 일단 Null값 허용으로 변경했다.

근데 휴대폰 번호가 Not Null이어야 한다면?

사용자 정보를 담는 별도의 테이블을 만들어야 한다. 이 테이블에는 OAuth2 로그인으로 받아올 수 없는 필수 정보(예: 휴대전화번호)를 저장하고, OAuth2에서 제공받는 사용자 정보와 연결될 수 있도록 설계해야 한다.

이렇게 하면 OAuth2 로그인으로 받아올 수 없는 정보를 사용자에게 따로 요청하여 저장하고, 필요할 때 해당 정보를 활용할 수 있다.

### 3. ProviderId 에러

데이터 베이스에 저장할 때, 'provider_id' 컬럼에 대입되는 값이 해당 컬럼의 데이터 타입 범위를 벗어나는 문제다.

INT → VARCHAR 타입으로 수정 후 해결했다.

```java
Data truncation: Out of range value for column 'provider_id' at row 1
```

### 왜 INT 타입은 안 되는건가

또 검색을 했다.

구글 OAuth2에서 제공하는 'provider_id'는 사용자를 고유하게 식별하는 문자열이다. 따라서 데이터베이스에서 이 'provider_id'를 저장할 때는 문자열을 저장할 수 있는 VARCHAR 또는 TEXT와 같은 데이터 타입을 사용해야 한다.

왜냐면, 'provider_id' 값이 숫자만으로 구성되지 않고 길이가 변할 수 있기 때문이라고 한다.