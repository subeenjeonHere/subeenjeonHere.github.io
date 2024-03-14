---
title: 🚀 Planet Project (1) 게시판 CRUD
date: 2024-02-28
categories: ProjectJourney
tags:
  [
    ProjectJourney
  ]
---

# TOC

<!-- TOC -->
* [TOC](#toc)
* [☻ 게시판 CRUD 기능](#-게시판-crud-기능)
    * [☺︎ 목표 구현 기능](#-목표-구현-기능)
* [☻ 게시글 리스트 + JPA 페이지네이션](#-게시글-리스트--jpa-페이지네이션)
    * [아쉬운 점이 있다면](#아쉬운-점이-있다면)
* [☻ 게시글 조회 + 조회수 증가](#-게시글-조회--조회수-증가)
  * [JPA를 사용해 사용자가 작성한 특정 게시글 가져오기](#jpa를-사용해-사용자가-작성한-특정-게시글-가져오기)
    * [하지만 주의해야 할 점은](#하지만-주의해야-할-점은)
    * [그럼 ‘TransactionRequiredException’ 은 무엇인가](#그럼-transactionrequiredexception-은-무엇인가)
* [☻ 게시글 작성](#-게시글-작성)
* [☻ 게시글 수정](#-게시글-수정)
* [☻ 게시글 삭제](#-게시글-삭제)
* [📌 발생했던 에러 정리](#발생했던-에러-정리)

<!-- TOC -->

---

# ☻ 게시판 CRUD 기능

### ☺︎ 목표 구현 기능

- 사용자는 글을 작성, 조회, 수정, 삭제할 수 있다.
- 사용자는 게시된 글의 댓글을 조회, 작성, 수정, 삭제할 수 있다.
- 사용자는 게시글을 페이지 단위로 조회할 수 있다.
- 사용자는 각 게시글의 조회수를 확인할 수 있다.

---

주로 CRUD (생성, 읽기, 업데이트, 삭제) 기능이다. 사용자가 정보를 생성하거나 저장한 후에 정보를 검색하고, 필요에 따라 수정하거나 삭제할 수 있도록 해주는 것이 이 기능의 핵심이다.

또한, JPA를 사용하였다. 다만, 이게 끝은 아니다. JPA를 사용해서 CRUD 작업에서 발생할 수 있는 여러 이슈들이 있다.

| 문제                       | 설명                                                                                        | 해결법                                     |
|--------------------------|-------------------------------------------------------------------------------------------|-----------------------------------------|
| N+1 문제                   | JPA가 쿼리를 수행할 때 연관된 엔티티를 로딩하기 위해 추가적인 쿼리를 실행하는 현상                                          | Fetch Join이나 EntityGraph를 통한 적절한 쿼리 최적화 |
| 영속성 컨텍스트와 데이터베이스 간의 불일치: | JPA는 영속성 컨텍스트라는 1차 캐시를 사용해 엔티티를 관리하는데, 이 컨텍스트가 항상 DB와 동기화되지 않을 수 있다. 이로써 상태 차이가 발생할 수 있다. |                                         |

완벽하게 이 성능 이슈까지 고려하여 기능을 구현 했다고는 못 하겠으나, 4월 이후 리팩토링하면서 JPA를 더 깊게 공부해볼 것이다.

---

# ☻ 게시글 리스트 + JPA 페이지네이션

![My Gif](https://subeenjeonHere.github.io/assets/t1.gif)

## ☺︎ 게시글 리스트, 페이지 네이션 동작 방식

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/1efa19b3-556e-4ce9-9ab0-209db93b1ff6)


### Controller

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/bd02db02-3b79-47c7-8d53-1f2032b3eda4)


```java
@GetMapping("/list")
    public String getAllBoard(Model model
            , @RequestParam(defaultValue = "0") int pageNo
            , @RequestParam(defaultValue = "10") int pageSize) {
        Page<Board> boardPage = boardService.findAll(pageNo, pageSize);
        model.addAttribute("currentPage", boardPage.getNumber());
        model.addAttribute("totalBoards", boardPage.getTotalElements());
        model.addAttribute("totalPages", boardPage.getTotalPages());
        model.addAttribute("pageSize", boardPage.getSize());
        model.addAttribute("boardPage", boardPage);
        return "/boardlist";
    }
```

### Service

```java
Page<Board> findAll(int pageNo, int pageSize);
```
```java
@Override
public Page<Board> findAll(int pageNo, int pageSize) {
    Pageable pageable = PageRequest.of(pageNo, pageSize);
    return boardRepository.findAll(pageable);
}
```

### 게시글 리스트 출력

게시글 리스트를 출력하는 부분에서는 JPA의 `findAll` 메소드를 사용하여 데이터 베이스에서 모든 게시글들을 가져왔다.

### 페이지 네이션

이 때, `Pageable` 인터페이스를 파라미터로 전달하여 특정 페이지의 게시글만 가져오도록 했다. 컨트롤러의 파라미터에서 페이지 번호(pageNo), 페이지 크기(pageSize)로 한 페이지에 표시될 게시글의
수와 현재 페이지를 설정하였다.

이후, Service 구현체(Impl)에서 PageRequest.of() 메소드를 사용하여 페이지 번호와 페이지 크기를 인자로 전달해 Pageable 객체를 생성했다. 최종적으로, 게시글 리스트를 전체 가져오는
findAll 메소드에 pageble 객체를 전달하여 페이지 네이션 된 결과를 반환하도록 했다.

### 아쉬운 점이 있다면

PageWrapper 클래스를 사용해도 되었을 것이다. 그렇다면 컨트롤러가 더 간결해지고, 가독성이 향상됐을 것이다. 이전에 사용했던 적이 있는데 이번에는 별도로 생성하지 않고, 컨트롤러 내에서 페이징 정보를
처리했다. 아래는 이전에 사용했던 PageWrapper 코드이다.

### PageWrapper

```java
public class PageWrapper<T> {
    public static final int MAX_PAGE_ITEM_DISPLAY = 5;
    private Page<T> page;
    private List<PageItem> items;
    private int currentNumber;
    private String url;

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public PageWrapper(Page<T> page, String url) {
        this.page = page;
        this.url = url;
        items = new ArrayList<PageItem>();

        currentNumber = page.getNumber() + 1;
        int start, size;
        if (page.getTotalPages() <= MAX_PAGE_ITEM_DISPLAY) {
            start = 1;
            size = page.getTotalPages();
        } else {
            if (currentNumber <= MAX_PAGE_ITEM_DISPLAY - MAX_PAGE_ITEM_DISPLAY / 2) {
                start = 1;
                size = MAX_PAGE_ITEM_DISPLAY;
            } else if (currentNumber >= page.getTotalPages() - MAX_PAGE_ITEM_DISPLAY / 2) {
                start = page.getTotalPages() - MAX_PAGE_ITEM_DISPLAY + 1;
                size = MAX_PAGE_ITEM_DISPLAY;
            } else {
                start = currentNumber - MAX_PAGE_ITEM_DISPLAY / 2;
                size = MAX_PAGE_ITEM_DISPLAY;
            }
        }

        for (int i = 0; i < size; i++) {
            items.add(new PageItem(start + i, (start + i) == currentNumber));
        }
    }
// 생략
}
```

### Controller

```java

@RequestMapping(value = "/list", method = RequestMethod.GET)
public String boardList2(Model model, Pageable pageable) {
    //페이징 된 객체 출력
    Page<Board> boardPage = boardService.boardList(pageable);
    //페이징된 정보 래핑
    PageWrapper<Board> page = new PageWrapper<Board>(boardPage, "/boardlist");
    model.addAttribute("boardlist", page.getContent());
    model.addAttribute("page", page);
    return "boardlist";
}
```

---

# ☻ 게시글 조회 + 조회수 증가

## ☺︎ 특정 게시글 조회 동작 방식

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/d22070be-e221-461b-8346-915444efcd4d)

### Controller

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/7091039a-4756-4049-9eaa-cfe9b96a1c37)

```java
@GetMapping("/view/{boardNo}")
public String getBoard(@PathVariable("boardNo") Long boardNo, Model model) {
    Optional<Board> newBoard = boardService.findBoardWithBoardNo(boardNo);
    newBoard.ifPresent(board -> {
        List<Comment> commentList = commentService.findCommentList(boardNo);
        model.addAttribute("commentList", commentList);
        model.addAttribute("board", board);
        model.addAttribute("username", board.getUser().getUsername());
    });
    return "/boardview";
}
```

`getBoard()` 메소드를 통해 게시글 상세 조회가 시작된다. 게시글의 번호(`boardNo`)를 매개변수로 받아와서 `Model` 객체에 추가한다. 그리고 `@PathVariable` 어노테이션을 사용하여
URL 경로에서 게시글 번호를 추출하는데, 이는 게시글 조회를 위한 key 값으로 사용된다.

이후, Service를 호출한다. `findBoardWithBoardNo()` 메소드를 호출하여 게시글 번호에 해당하는 게시글 정보를 가져온다.

### ServiceImpl

```jsx
@Override
public Optional<Board> findBoardWithBoardNo(Long boardNo) {
    boardRepository.updateHit(boardNo);
    return boardRepository.findBoardWithBoardNo(boardNo);
}
```

`findBoardWithUser()` 메소드 내부에서는 먼저 `boardRepository.updateHit(boardNo);`를 호출하여 게시글의 조회수를 1 증가시킨다. 그
후 `boardRepository.findBoardWithUser(boardNo);`를 호출하여 게시글 정보와 게시글 작성자의 정보를 함께 가져온다.

## JPA를 사용해 사용자가 작성한 특정 게시글 가져오기

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/5840e900-9048-4267-82c2-2e81714007cd)

### Board Entity

```jsx
@ManyToOne(fetch = FetchType.LAZY)
@JoinColumn(name = "user_id")
private User user;
```

Board 엔티티엔 사용자와 게시글 사이의 관계를 설정하기 위해 User 필드가 ManyToOne으로 매핑되어 있다. 이는 하나의 사용자가 여러 게시글을 작성할 수 있음을 나타낸다.
또한, `fetch = FetchType.LAZY` 로 게시글을 조회할 때 사용자 정보는 바로 로드하지 않고, 필요할 때 로드하도록 설정한다.

### Repository

```jsx
@Query("SELECT b FROM Board b JOIN FETCH b.user WHERE b.boardId = :boardNo")
Optional<Board> findBoardWithBoardNo(@Param("boardNo") Long boardNo);
```

게시글 번호를 매개 변수로 받아 해당 번호의 게시글과 그 게시글을 작성한 사용자의 정보를 함께 조회하는 쿼리를 실행한다.

```java
@Modifying
@Transactional
@Query("UPDATE Board b set b.hit = b.hit +1 WHERE b.boardId = :boardNo")
int updateHit(Long boardNo);
```

Service에서 호출한 조회수 증가 메소드로, 특정 게시글의 조회수를 증가시키는 쿼리를 실행한다. `@Modifying` 어노테이션은 이 쿼리가 데이터를 변경한다는 것을 나타낸다.

---

`@Modifying` 어노테이션은 처음 사용해봐서, 다시 공부를 해보자면

### `@Modifying`

Spring Data JPA에서 제공하는 어노테이션으로, 이 어노테이션을 사용하면 쿼리 메소드가 데이터베이스의 데이터를 수정할 수 있게 한다. 일반적으로 `@Query` 어노테이션과 함께 사용되며, JPA 쿼리를
사용하여 데이터를 업데이트, 삭제 등의 작업을 수행할 때 필요하다.

예를 들어, 게시글의 조회수를 증가시키는 메소드에서 `@Modifying` 어노테이션을 사용하면, 해당 메소드가 데이터베이스의 게시글 조회수를 직접 수정할 수 있게된다.

### 하지만 주의해야 할 점은

`Modifying` 어노테이션을 사용하는 메소드는 데이터의 변경이 발생하므로, 이 변경 작업이 트랜잭션 내에서 실행되어야 한다. 그렇지 않으면 `TransactionRequiredException`이 발생할 수
있다고 한다. 따라서 `@Modifying` 어노테이션을 사용하는 메소드는 `@Transactional` 어노테이션을 반드시 추가해야 한다.

### 그럼 ‘TransactionRequiredException’ 은 무엇인가

`TransactionRequiredException`은 트랜잭션이 필요한 상황에서 트랜잭션이 없을 때 발생하는 예외이다. 트랜잭션은 여러 개의 데이터베이스 작업이 한 번에 수행되어야 할 때 사용되며, 데이터 베이스
일관성 유지를 위해 이러한 작업들은 모두 성공하거나 모두 실패해야 한다.

---

# ☻ 게시글 작성

![My Gif](https://subeenjeonHere.github.io/assets/t22.gif)

## ☺︎ 게시글 작성 동작 방식

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/75131d2b-4e0b-4dff-8a2e-65cb5b3f99c2)
### Controller

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/73aa3e21-4af2-421f-81d6-3b02a4554805)

```java

@PostMapping("/writepro")
public String saveBoard(@Valid Board board, BindingResult bindingResult, Model model) {
    logger.info("Saved title={}, content={}", board.getTitle(), board.getContent());
    model.addAttribute("board", board);
    if (bindingResult.hasErrors()) {
        model.addAttribute("messgae", "글 작성 중 에러가 발생했습니다.");
        return "redirect:/board/list";
    }
    boardService.save(board);
    return "redirect:/board/list";
}
```

게시글 작성 기능에서는 `@Valid` 어노테이션과 `BindingResult`를 이용하여 유효성 검사를 수행하고, 에러 처리를 하였다.

### `@Valid`

Valid 어노테이션은 게시글 객체의 유효성을 검사하며, 객체의 필드에 선언된 제약 조건을 검사한다. 만약 제약 조건을 위반하면,  `BindingResult`에 에러 정보가 저장되며, 이를 이용하여 에러 처리를
수행할 수 있다.

예를 들어, 객체의 필드에 `@NotNull`, `@Size`, `@Min`, `@Max` 등의 제약 조건 어노테이션을 선언하면, `@Valid` 어노테이션은 이러한 제약 조건들이 지켜지는지 검사한다.

만약 해당 필드가 `@NotNull`로 선언되었는데 `null` 값이 들어가거나, `@Size(max=10)`으로 선언되었는데 그 길이가 10을 초과하는 등의 경우, `@Valid` 어노테이션은 이를 에러로
간주하고 `BindingResult` 객체에 에러 정보를 저장한다.

### `BindingResult`

스프링에서 제공하는 데이터 바인딩 결과를 담는 객체이다. 이것은 컨트롤러 메서드의 매개변수로 선언되며, 특히 `@Valid` 어노테이션으로 유효성 검사를 수행한 결과를 담는다. 예를
들어, `bindingResult.hasErrors()` 메소드를 사용하여 에러의 유무를 확인하고, 에러가 있을 경우 에러 메시지를 사용자에게 반환하거나 에러 처리를 수행할 수 있다.

### Service

```java
@Override
public Board save(Board board) {
    return boardRepository.save(board);
}
```

그리고 Service 계층에서는 save 메소드를 호출해 게시글을 저장한다.

### 수정 전

처음 공부할 때 이런 코드로 게시글 유효성 검사(?)를 작성했다..

단순히 "제목"과 "내용" 필드가 비어있지 않은지 확인하는 간단한 조건문일 뿐, 실제 유효성 검사가 아니다.

```java
if (!board.getTitle().isEmpty() && !board.getContent().isEmpty()) {
```

### 유효성 검사란?

사용자가 입력한 데이터가 애플리케이션에 의해 예상된 형식, 길이, 또는 범위를 충족하는지 확인하는 과정이다. 요구사항과 일치하도록 보장하며, 올바르지 않은 데이터가 애플리케이션 내부로 들어가는 것을 막아준다.

### 언제 시행하는가?

1. 사용자가 데이터를 입력하거나 수정할 때
2. 사용자가 데이터를 서버로 전송하기 전에
3. 데이터를 데이터베이스에 저장하기 전에

### 스프링에서 유효성 검사를 수행하는 법

| 종류                              | 설명                                                                                                                                         |
|---------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Java Bean Validation            | Java Bean Validation API는 클래스에 제약조건을 선언할 수 있게 해 주는 어노테이션 기반의 프레임 워크이다. . 예를 들어 @NotNull, @Size, @Min, @Max 등의 어노테이션을 사용하여 제약조건을 선언할 수  있다. |
| @Valid                          | Spring에서 제공하는 Bean Validation을 위한 어노테이션이다. 객체의 유효성을 검사하기 위해 사용되며, 주로 객체의 필드 위에 선언된 제약 조건을 검사한다.                                            |
| BindingResult 객체 사용:            | Spring은 데이터 바인딩 시점에 발생할 수 있는 문제를 해결하고, 사용자 입력을 검증하기 위한 Validator 인터페이스를 제공한다. 이를 구현하여 사용자 정의 검증 로직을 추가할 수 있다.                              |
| ControllerAdvice를 이용한 전역 예외 처리: | @ControllerAdvice 어노테이션을 사용하여 컨트롤러에서 발생하는 예외를 전역에서 처리할 수 있다. 이를 통해 유효성 검사에서 발생한 예외를 효과적으로 처리할 수 있다.                                        |

---

# ☻ 게시글 수정

## ☺︎ 게시글 수정 동작 방식

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/0dccde55-7d48-4851-94a4-744064539dac)
### Controller

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/20700512-b0d5-40cd-85bf-03ed1c7bb01a)

```jsx
@PostMapping("/update/{boardNo}")
public String boardUpdate(@PathVariable("boardNo") Long boardNo, @Valid Board board, BindingResult bindingResult) {
    logger.info("Updated title={} , content={}", board.getTitle(), board.getContent());
    Optional<Board> existingBoardOptional = boardService.findBoardById(boardNo);
    existingBoardOptional.ifPresent(existingBoard -> {
        existingBoard.setTitle(board.getTitle());
        existingBoard.setContent(board.getContent());
        boardService.save(existingBoard);
    });
    return "redirect:/boardlist";
}
```

### Service

```jsx
@Override
public Optional<Board> findBoardById(Long boardNo) {
    return boardRepository.findById(boardNo);
}
```

게시글 수정 기능은 `@PostMapping` 어노테이션을 사용하여 URL 경로에 게시글 번호를 전달한다. 이를 통해 특정 게시글을 수정할 수 있다. 이 후 게시글 번호를 매개변수로
받아, `findBoardById` 메소드를 호출하여 DB에서 해당 번호의 게시글을 찾는다. 이 때 반환값은 `Optional<Board>` 타입이다.

`Optional` 객체의 `ifPresent()` 메소드를 호출하여 객체가 존재하는지 확인하고, 존재할 경우 `existingBoard`로 변환하여 게시글의 제목과 내용을 수정하고, 수정된 게시글은 save
메소드를 호출하여 저장된다.

---

# ☻ 게시글 삭제

## ☺︎ 게시글 삭제와 동작 방식

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/3a4ec15b-9a37-461d-844f-9991fd4ad742)

### Controller

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/f8aadf49-e554-4503-be5e-006d042d21c1)

```jsx
// 게시글삭제
@GetMapping("/delete/{boardNo}")
public String boardDelete(@PathVariable("boardNo") Long boardNo, Board board) {
    logger.info("Board delete Id={}", board.getBoardId());
    Optional<Board> boardOptional = boardService.findBoardById(boardNo);
    boardOptional.orElseThrow(() -> new NoSuchElementException("Not Found"));
    boardService.delete(boardOptional.get());
    return "redirect:/boardlist";
}
```

게시글 번호를 매개변수로 받아, `findBoardById` 메소드를 호출하여 DB에서 해당 번호의 게시글을 찾는다. 이 때 반환값은 `Optional<Board>` 타입이다. 이후, Optional 객체의
oeElseThorw()를 호출하여 객체가 존재하지 않을 경우 `NoSuchElementException` 을 발생시키고, 게시글이 존재할 경우 delete 메소드를 호출하여 게시글을 삭제한다.

### Service-Repository

```jsx
@Override
public void delete(Board board) {
    boardRepository.delete(board);
}
```
---

# 📌 발생했던 에러 정리


비교적 간단하고, 기본적인 부분들을 놓친 것이기에 부끄럽지만, 발생했던 에러들 정리하기

### 1. 외래 키 제약조건

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

---

### 2. Spring이 내 Entity를 완전히 무시할 때

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

> 막상 해결하고 보니 기술적인 부분은 아니었고, 정말 간단한 에러였지만 시간을 많이 소요해서 기록해두었다..🥲
>