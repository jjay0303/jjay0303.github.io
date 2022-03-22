---
layout: post
title: "[Spring] Annotation이란?"
date: 2022-01-13
excerpt: "Annotation에 대해 알아보자"
categories: Spring
tags: [study, Spring, Annotation]
comments: true
---


### 1. Annotation이란?
 - JAVA에서 자바 소스코드에 추가하여 사용가능한 메타데이터의 일종
 - 소스코드에 추가시 특별한 기능들을 사용할 수 있음
 - 클래스와 메서드에 추가하여 다양한 기능을 부여함
    - bean주입, getter/setter 생성, 클래스 역할 부여 등등..

#### 👉코드량 감소, 생산성 증가, 유지보수 쉬움


### 2. Annotaion의 종류와 역할

 #### @Configuration 
   - 빈 설정을 담당하는 클래스에 사용
   - 나의 경우 xml로 config를 해서 사용해보지 않음

 #### @ComponentScan
   - @Configuration과 함께쓰면 해당 클래스가 자바 빈 설정 클래스임을 알려주고 스프링 빈 범위를 정의할 수 있다

 #### @Component
   - 개발자가 생성한 Class를 Spring의 Bean으로 등록할 때 사용
   - @ComponentScan의 주요 대상

 #### @Bean
   - 외부라이브러리 등을 Bean으로 등록할 때 사용
   - name 속성을 사용하여 id 지정 가능

 #### @Controller
   - Spring MVC의 Controller 클래스를 bean에 자동 등록

 #### @Service
   - Service 클래스를 bean에 자동 등록

 #### @Repository
   - DB와 접근하는 클래스(Dao)에 사용
 
 #### @Autowired
   - field(속성), setter, constructor(생성지)에 사용
   - Type에 따라서 알아서 해당 객체 생성하고 bean을 강제 주입(스프링이 자동으로 값을 할당) 👉 DI(의존성 주입) 특징
   - Type 먼저 확인 후 못 찾으면 Name에 따라 주입
   - 해당 bean을 가져오지 못하면 `NoUniqueBeanDefinitionException` 예외 발생

 #### @RequestMapping(method=, value="", consumes="", produces="")
   - HandlerMapping 등록으로 요청된 url과 일치하는 value값을 가지는 클래스나 메소드를 실행
   - Class 단위면 하위 메소드 모두 적용 / 메소드 단위면 해당 메소드만 응답값을 받음
   - 속성들
      - method : GET/POST
      - value : 요청 받기위한 경로 또는 mapping값
      - consumes : 요청값의 유형 지정
      - produces : 요청에 대한 응답 유형 지정, json객체 응답 등에 사용

 #### @RequestParam(value="", defaultValue="")
   - view에서 요청값을 받을 때 `url`의 ?뒤에 오는 값, `POST` 전송 시 요청받을 키값, value값 등을 받을 때 사용
   - 속성들
      - value : 요청받는 경로나 mapping값
      - defaultValue : 요청이 없을 경우 대체할 기본값
   <br>
   - 만약 특정 게시판을 가기위해 해당 태그를 클릭하면 클릭 시 page값(첫 페이지 값)을 같이 넘겨줘야 한다<br> 
   👉VIEW에서 `<a href="?cpage=1">`로 url뒤의 값을 controller로 직접 넘겨줌

   - @RequestParam으로 기본값을 지정 시<br> 
   👉VIEW에서 `<a href="boardMain.bo">`로 매핑만 해주고 controller에서 지정한 기본값으로 알아서 처리

   ```java
   @RequestMapping(value="boardMain.bo")
	public String eventMain(@RequestParam(value="cpage", defaultValue="1") int currentPage) {
		// cpage의 값을 1로 지정하여 currentPage 즉, 현재페이지=1페이지로 설정됨
	}
   ```
   <br><br>

#### 출처

  - 수업자료
  - <a href="https://dev.to/composite/-40c0">스프링 주요 어노테이션 정리</a>
