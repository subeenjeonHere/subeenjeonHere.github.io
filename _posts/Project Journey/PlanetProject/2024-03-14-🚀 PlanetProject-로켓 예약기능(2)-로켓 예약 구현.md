---
title: 🚀PlanetProject-로켓 예약기능(2) 로켓 예약 구현
date: 2024-03-14
categories: ProjectJourney
tags:
  [
    ProjectJourney
  ]
---

<!-- TOC -->
* [☻ 로켓 예약하기](#-로켓-예약하기)
  * [☺︎ 목표 구현 기능](#-목표-구현-기능)
  * [☺︎ 로켓 예약 하기 코드와 동작 방식](#-로켓-예약-하기-코드와-동작-방식)
  * [☺︎ 로켓 스케쥴 조회와 동작 방식](#-로켓-스케쥴-조회와-동작-방식)
* [☻ 로켓 예약 내역 조회](#-로켓-예약-내역-조회)
  * [☺︎ 로켓 예약 내역 조회와 동작 방식](#-로켓-예약-내역-조회와-동작-방식)
<!-- TOC -->

---

# ☻ 로켓 예약하기

## ☺︎ 목표 구현 기능

사용자는 출발 행성, 도착 행성 등을 입력하여 로켓을 예약할 수 있다.

![My Gif](https://subeenjeonHere.github.io/assets/14m1.gif)

---

## ☺︎ 로켓 예약 하기 코드와 동작 방식

사용자가 예약을 요청하면, 시스템은 행성 테이블에서 출발 행성과 도착 행성을 찾는다.

그 다음, 사용자 테이블에서 사용자를 찾는다. 이 정보를 바탕으로 예약 정보가 생성되어 예약 테이블에 저장된다.

이 과정이 완료되면, 시스템은 사용자에게 예약 완료를 반환하도록 했다.

### 예약 하기 (예약 정보 생성)

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/f15893ab-2a3d-41b0-9b0a-1a683fd4f8bf)

### Controller

```java
    /**
     * @param rocketId
     * @param model
     * @return
     * @Feature: Flow-Step2 로켓 스케쥴 Showing
     */
    @GetMapping("/showschedule/{rocketId}")
    public String showScheduleListByRocket(@PathVariable("rocketId") Long rocketId, Model model) {
        List<Schedule> schedulelistByRocket = scheduleService.findByRocket(rocketId);
        model.addAttribute("schedulelistByRocket", schedulelistByRocket);
        return "/planetHub/reservationForm";
    }

    /**
     * @param reservationDto
     * @param bindingResult
     * @param model
     * @return
     * @throws NotFoundException
     * @Feature: Flow-Step3 예약 진행
     */
    @PostMapping("/reservation")
    public String saveReservation(@ModelAttribute("reservationDtos") @Valid ReservationDto reservationDto, BindingResult bindingResult, Model model) throws NotFoundException {
        log.info("Processing reservation");
        if (bindingResult.hasErrors()) {
            return "/planetHub/reservationForm";
        }
        reservationService.saveReservation(reservationDto);
        return "redirect:/planet";
    }

```

### Service

```java
    /**
     * @param reservationDto
     * @Author: Subeen Jeon
     * @Date: 8 Feb
     * @Feature: Reservation rockets
     */
    @Transactional
    @Override
    public void saveReservation(ReservationDto reservationDto) throws NotFoundException {
        try {
            User user = userRepository.findByEmail(reservationDto.getEmail());
            if (user != null) {
                Planet departure = planetRepository.findById(reservationDto.getDeparturePlanetId()).orElseThrow(() ->
                        new NotFoundException("Departure planet not found"));
                Planet arrival = planetRepository.findById(reservationDto.getArrivalPlanetId()).orElseThrow(() ->
                        new NotFoundException("Arrival planet not found"));

                Reservation reservation = Reservation.builder()
                        .departurePlanet(departure)
                        .arrivalPlanet(arrival)
                        .travleDate(reservationDto.getDepartureDate())
                        .passengerCount(reservationDto.getPassengerCount())
                        .users(user)
                        .build();
                reservationRepository.save(reservation);
            }
        } catch (Exception e) {
            throw new NotFoundException("User Not Found.");
        }
    }

    /**
     * @param email
     * @return resrvationList
     * @throws NotFoundException
     * @Feature: Find reservation by user
     */
    @Override
    public List<Reservation> findReservationByUser(String email) throws NotFoundException {
        List<Reservation> reservationList = reservationRepository.findReservationByUsersEmail(email);
        return reservationList;
    }
```

---

## ☺︎ 로켓 스케쥴 조회와 동작 방식

사용자가 로켓 예약 폼에 접속하면, 사용자에게 표시할 모든 행성 정보, 스케줄 정보, 그리고 로켓 정보를 조회하는 로직이다.

이를 위해 `PlanetRepository`, `ScheduleService`, 그리고 `RocketRepository`를 사용하여 각각의 정보를 가져왔다. 이후 컨트롤러에서 Model에 추가하여 사용자에게 표시하도록 했다.

### 로켓 스케쥴 조회

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/5e33b403-9f9c-47fd-a7f7-211482baf890)

### Controller

```java
    /**
     * @param model
     * @return
     * @throws NotFoundException
     * @Feature: Flow-Step1 예약 폼 Showing
     */
    @GetMapping("/reservationForm")
    public String showReservationForm(Model model) throws NotFoundException {
        List<Planet> planetlist = planetRepository.findAll();
        List<Schedule> schedulelist = scheduleService.findAll();
        List<Rocket> rocketList = rocketRepository.findAll();

        log.info("Schedule={}", schedulelist.size());
        model.addAttribute("planetlist", planetlist);
        model.addAttribute("schedulelist", schedulelist);
        model.addAttribute("rocketList", rocketList);
        model.addAttribute("reservationDto", new ReservationDto());
        log.info("List ={}", planetlist.size());
        return "/planetHub/reservationForm";
    }

```

### Service

```java
@Repository
public interface PlanetRepository extends JpaRepository<Planet, Long> {

    @Override
    @Transactional(readOnly = false)
    List<Planet> findAll();

@Service
public class ScheduleServiceImpl implements ScheduleService {

    @Override
    public List<Schedule> findAll() {
        return scheduleRepository.findAll();
    }

    @Override
    public List<Schedule> findByRocket(Long rocketId) {
        List<Schedule> scheduleListByRocket =
                scheduleRepository.findByScheduleByRocketId(rocketId);
        return scheduleListByRocket;
    }

@Repository
public interface RocketRepository extends JpaRepository<Rocket, Long> {
}
```

---

# ☻ 로켓 예약 내역 조회

### ☺︎ 목표 구현 기능

구현해야 할 기능은 로켓을 예약한 사용자가 자신의 예약 내역을 조회할 수 있도록 한다.

그리고, 예약 상세 정보에서 예약 내역을 보여줄 것이다.

### 1차 구현

![My Gif](https://subeenjeonHere.github.io/assets/14m2.gif)

### ERD

테이블은 아래와 같다. 예약(Reservation)과 사용자(User) 사이의 관계에서 주인은 예약(Reservation)이다.

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/521e315a-aecf-40c3-9b78-139468343a68)


## ☺︎ 로켓 예약 내역 조회와 동작 방식

![image](https://github.com/subeenjeonHere/subeenjeonHere.github.io/assets/145312273/aaf08a95-3fa4-4d56-adb3-b122e4a03ef1)


### Controller

showConfirmationByUser 메소드에서 사용자의 이메일을 받아와, 해당 이메일로 예약 내역을 조회하도록 했다.

```java
    /**
     * @param user
     * @param model
     * @return
     * @Feature: Flow-Step1 예약 내역 조회폼 Showing
     */
    @GetMapping("/reservation/confirmation")
    public String showConformationForm(@ModelAttribute User user, Model model) {
        log.info("Flow 1");
        model.addAttribute("user", new User());
        return "/reservation/reservation-flow1";
    }

    /**
     * @param user
     * @param result
     * @param model
     * @return
     * @throws NotFoundException
     * @Feature: Flow-Step2 예약 내역 Showing
     */
    @GetMapping("/reservation/user-confirm")
    public String showConfirmationByUser(@Valid @ModelAttribute("user") User user,
                                         BindingResult result, Model model) throws NotFoundException {
        //예약 내역 검색
        try {
            log.info("Params email={}, phoneNumber={}", user.getEmail(), user.getPhoneNumber());
            String email = user.getEmail();
            List<Reservation> reservationList = reservationService.findReservationByUser(email);
            model.addAttribute("reservationList", reservationList);
            return "/reservation/reservation-flow2";
        } catch (Exception e) {
            e.printStackTrace();
            return "error/404";
        }
```

### Service

```java
    /**
     * @param user
     * @param result
     * @param model
     * @return
     * @throws NotFoundException
     * @Feature: Flow-Step2 예약 내역 Showing
     */
    @GetMapping("/reservation/user-confirm")
    public String showConfirmationByUser(@Valid @ModelAttribute("user") User user,
                                         BindingResult result, Model model) throws NotFoundException {
        //예약 내역 검색
        try {
            log.info("Params email={}, phoneNumber={}", user.getEmail(), user.getPhoneNumber());
            String email = user.getEmail();
            List<Reservation> reservationList = reservationService.findReservationByUser(email);
            model.addAttribute("reservationList", reservationList);
            return "/reservation/reservation-flow2";
        } catch (Exception e) {
            e.printStackTrace();
            return "error/404";
        }
```

```java
    /**
     * @param email
     * @return resrvationList
     * @throws NotFoundException
     * @Feature: Find reservation by user
     */
    @Override
    public List<Reservation> findReservationByUser(String email) throws NotFoundException {
        List<Reservation> reservationList = reservationRepository.findReservationByUsersEmail(email);
        return reservationList;
    }
```

---
