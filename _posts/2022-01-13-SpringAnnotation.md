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
 1. @Component
    - 개발자가 생성한 Class를 Spring의 Bean으로 등록할 때 사용

 2. @Bean
    - 외부라이브러리 등을 Bean으로 등록

 3. @Controller
    - Controller 클래스를 bean에 자동 등록

 4. @Service
    - Service 클래스를 bean에 자동 등록
 
 5. @Autowired
    - 해당 객체 생성하고 알아서 주입해줌
    - 즉, bean으로 등록한 클래스를 불러와서 주입받게 해줌
    👉 DI(의존성 주입) 특징 

 6. @RequestMapping(value="")
    - HandlerMapping 등록으로 요청된 url과 일치하는 value값을 가지는 클래스나 메소드를 실행
    - Class 단위면 하위 메소드 모두 적용 / 메소드 단위면 해당 메소드만

 7. @Repository
    - DB와 접근하는 클래스에 사용

