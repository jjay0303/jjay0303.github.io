---
layout: post
title: "[Spring] spring-security Library"
date: 2022-01-14
excerpt: "spring-security에 대해 알아보자"
categories: Spring
tags: [study, Spring, Spring Library]
comments: true
---

## spring-security 모듈 라이브러리(core, web, config)에 대해 알아보자

### 1. spring-security란?
 - 스프링 기반의 애플이케이션의 보안(인증, 권한, 인가 등)을 담당하는 스프링 하위 프레임워크
 - 서블릿 필터(filter)와 필터체인으로 구성된 위임모델 사용<br>
	👉 MVC와 분리하여 관리 및 동작
 -  기본적으로 세션&쿠키방식 인증

### 2. 기본 설정
### 1) 스프링 시큐리티 모듈 라이브러리 추가(core, web, config)
 - pon.xml에 core, web, config 라이브러리를 추가해준다. 단, 버전 다 같게!!

```javaScript
<dependency>
	<groupId>org.springframework.security</groupId>
	<artifactId>spring-security-core</artifactId>
	<version>5.5.2</version>
</dependency>

<dependency>
	<groupId>org.springframework.security</groupId>
	<artifactId>spring-security-web</artifactId>
	<version>5.5.2</version>
</dependency>

<dependency>
	<groupId>org.springframework.security</groupId>
	<artifactId>spring-security-config</artifactId>
	<version>5.5.2</version>
</dependency>
```

### 2) BcryptPassWordEncoder 클래스를 bean으로 등록
 - 기존 root-context.xml에 해도 상관없지만 따로 관리하기 위해 spring-security.xml을 생성하여 bean 등록(xml파일명은 자유)

 1. New - Spring Bean Configuratio File - 이름 설정 후 next
 
 2. Namespace에서 beans와 security 항목 체크 => 라이브러리를 먼저 추가해야 security 항목이 나옴

 ![1](https://user-images.githubusercontent.com/93863500/149475832-ebc896fd-b2bd-43d2-b470-b2b35be81a15.JPG)

### 3. 서버 구동시 바로 해당 xml이 읽히도록(pre-loading) web.xml에서 설정
 - &lt;context-param>태그 내 &lt;param-value>에 해당 xml의 경로를 추가

```javaScript
<context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>
		/WEB-INF/spring/root-context.xml
		/WEB-INF/spring/spring-security.xml			
	</param-value>
</context-param>
```

### 4. 서버 실행 후 에러 없으면 성공!

#### 참고
 - 수업자료
 - <a href='https://devuna.tistory.com/55'>[Spring Security] 스프링시큐리티의 기본 개념과 구조</a>