---
layout: post
title: "Spring FrameWork란?"
date: 2022-01-12
excerpt: "Spring FrameWork에 대해 알아보자"
tags: [study, Spring FrameWork, spring]
comments: true
---


## Spring FrameWork란?
 - 자바 플랩폼을 위한 오픈소스 애플리케이션 프레임워크로 Spring이라고도 불림
 - 동적 웹사이트 개발을 위한 여러 서비스를 제공하며 전자정부 표준프레임워크의 기반 기술

### 특징
 
 |:---:|:---:|
 |IOC<br>(Inversion of Control)<br>제어반전|컨트롤의 제어권이 프레임워크에 있음. 개발자는 만들어둔 자원을 호출해서 사용|
 |DI<br>(Dependency Injection)<br>의존성주입|설정파일이나 어노테이션으로 객체간 의존관계를 설정해 개발자가 객체 생성 X|
 |POJO<br>(Plain Old Java Object)|특정 라이브러리를 사용할 필요가 없어 개발이 쉽고 기존 라이브러리의 지원이 용이|
 |Spring AOP<br>(Aspect Oriented Programming)<br>관점지향 프로그래밍|트랜잭션, 로깅, 보안 등 공통으로 필요로 하는 기능을 분리하여 관리|
 |Spring JDBC|mybatis 같은 DB처리를 위한 영속성 프레임워크와 연결할 수 있는 인터페이스 제공|
 |Spring MVC|MVC패턴을 통해 Model, View, Controller 사이의 의존관계를 DI 컨테이너에서 관리하여 개발자가 아닌서버가 객체를 관리함|
 |PSA<br>(Portable Service Abstraction)|추상화 계층을 사용하여 개발자에게 편의성을 제공해주는 Service Abstraction을 다른 기술 스택으로 바꿀 수 있는 확장성을 제공|

### 스프링 설정 파일
 1. web.xml
  - 서버 구동시 web.xml이 제일 먼저 읽히는 파일로 여러 xml 파일을 인식하도록 각종 설정 정의
  - web.xml에서 사용자의 모든 요청을 appServlet이 받아서 DispatcherServlet으로 처리하도록 설정 => 즉, servlet을 일일히 만들 필요 없이 하나의 servlet으로 모든 요청을 처리하게 해줌

 2. root-context.xml
  - 서버 구동과 동시에 바로 셋팅할 내용들을 작성하면 됨(bean으로 등록)
	주로 DB연결설정, 트랜잭션 처리, 내외부 모듈 연동 등 비즈니스 로직 관련 설정
  - sqlSessionFactory, sqlSession 클래스 등록 등등
 
 3. pom.xml
  - Maven의 빌드 정보를 담고 있는 파일로 project, properties, repositories, dependenties, build 등의 태그로 이루어져 있음

 4. mybatis-config.xml
  - Dto 객체의 정보를 설정하는 파일로 typeAlias와 mapper 태그로 이루어져있음
  => 순수 mybatis일 때는 environment와 dataSource로 DB접속정보를 작성해야 했지만 spring-mybatis는 가장 먼저 읽히는 root-context에 db접속정보 등록함

 5. servlet-context.xml
 - 


#### 출처

  - 수업자료
  - 생활코딩



