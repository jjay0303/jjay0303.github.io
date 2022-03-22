---
layout: post
title: "[Spring] 설정파일 1. web.xml"
date: 2022-02-02
excerpt: "Spring"
categories: Spring
tags: [study, Spring]
comments: true
---

## web.xml에 대해 알아보자

### 1. web.xml이란?
 - DD(Deployment Descriptor:배포설명자)로 불리는 Web Application의 설정파일
 - 즉, 웹페이지의 환경설정파일로 WAS가 구동 시 함께 실행되야할 설정들을 정의한 문서

### 2. web.xml 구조
 1. <context-param> : STS의 기본 설정파일 이외에 사용자가 직접 컨트롤하는 XML파일을 지정 
 2. <servlet> : DispatcherServlet 관련 설정
 3. <listner> : 스프링 설정 정보 읽음
 4. <filter> : encoding 처리

### 2_1) context-param
 - 모든 서블릿과 필터에서 사용하는 root-context.xml을 설정
 ```xml
    <context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/root-context.xml</param-value>
	</context-param>
 ```

### 2_2) DispatcherServlet
 - 클라이언트의 요청 처리를 위한 DispatcherServlet 
 - Spring 콘테이너를 생성하여 클라이언트의 요청을 받아 컨트롤러로 보냄
 👉 즉, 하나의 서블렛으로 모든 요청을 받는 **스프링의 핵심요소**
 
### 2_3) ContextLoaderListner
 - 웹어플리케이션이 서블릿 컨테이너에 로딩될 때 실행되는 리스너로 웹어플리케이션이 로딩딜 때 Web ApplicationContext를 만들고, 생성된 WebApplicationContext는 contextConfigLocation에 설정한 빈 설정파일을 사용해 웹애플리케이션에서 사용할 객체를 관리
 👉 즉, 애플리케이션당 한 개의 WAC를 만들고 이 WAC를 애플리케이션 전체에서 사용

 - DispatcherServlet과 ContextLoadListner의 차이
 | DS | CLL |
 |:---:|:---:|
 | 서블릿 당 WAC 한 개 생성 | 애플리케이션 당 WAC 한 개 생성 |
 | CLL이 생성한 WAC를 상속받음(즉 자식 WAC) | CLL이 생성한 WAC는 부모 WAC |

 - 자식이 빈을 참조할 때 먼저 내부 빈을 참조 -> 없으면 부모 참조 -> 부모에도 없으면 예외 발생 
 => 때문에 빈을 나눠서 관리
   - WAC(CLL) : 웹에 종속적이지 않은 빈 ex)서비스, DAO
   - WAC(DS) : 웹에 종속적인 빈 ex) 컨트롤러, MVC 관련 빈

### 2_4) servlet-mapping
 - 이름 으로 처리 할 HTTP 요청을 URL 패턴으로 변경

## 참고자료
 - <a href="https://ssseung.tistory.com/m/72"> Servlet & 스프링 web.xml 설정 </a>
 - <a href="https://www.baeldung.com/spring-xml-vs-java-config"> web.xml vs Initializer with Spring </a>