---
layout: post
title: "[Spring] 스프링 xml 설정파일"
date: 2022-02-07
excerpt: "스프링 xml 설정파일"
categories: Spring
tags: [study, Spring]
comments: true
---

## 스프링 xml 설정파일들에 대해 알아보자

 - Spring 설정 파일들이 너무 헷갈려서 다시 한번 정리 하기 위한 포스트

### 전체적인 구조
  ![1](https://user-images.githubusercontent.com/93863500/153712161-2883ab71-5916-4adc-9306-b1ac44069f2b.JPG)
<br>

### 1. pom.xml
 - 구조 : project{프로젝트정보 + properties + [repositories] + dependencies + build}
 - dependecies : 다운받으려는 라이브러리들을 dependenty 태그를 이용하여 등록
 - 스프링, DB관련(마이바이트, 오라클), json등 사용할 `라이브러리들을 등록`

```xml
	<dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${org.springframework-version}</version>
			<exclusions>
				<!-- Exclude Commons Logging in favor of SLF4j -->
				<exclusion>
					<groupId>commons-logging</groupId>
					<artifactId>commons-logging</artifactId>
					</exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
	</dependencies>
```
<br>

### 2. web.xml
 - 서버 구동시 web.xml이 제일 먼저 읽히는 파일로 `여러 xml 파일을 인식`하도록 각종 설정 정의
 - 웹 어플리케이션 실행 시 함께 실행되야할 설정들을 정의해 놓은 것

```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://java.sun.com/xml/ns/javaee https://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">

		<!-- web application context 단위의 설정파일
			root-context.xml과 spring-security.xml를 설정파일로 지정-->
		<context-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>
				/WEB-INF/spring/root-context.xml
				/WEB-INF/spring/spring-security.xml
			</param-value>
		</context-param>
		<listener>
			<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>	
		</listener>

		<!-- Dispatcher Servlet을 appServlet이란 이름으로 생성하고 servlet-context.xml을 설정파일로 지정 -->
		<servlet>
			<servlet-name>appServlet</servlet-name>
			<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
			<init-param>
				<param-name>contextConfigLocation</param-name>
				<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
			</init-param>
			<load-on-startup>1</load-on-startup>
		</servlet>

		<!-- Dispatcher Servlet이 처리할 url 패턴을 설정. / 경로로 들어오는 모든 요청을 appServlet이 처리함 -->	
		<servlet-mapping>
			<servlet-name>appServlet</servlet-name>
			<url-pattern>/</url-pattern>
		</servlet-mapping>
		
		<!-- 스프링이 지원하는 인코딩필터 -->
		<filter>
			<filter-name>encodingFilter</filter-name>
			<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
			<init-param>
				<param-name>encoding</param-name>
				<param-value>UTF-8</param-value>
			</init-param>
		</filter>
		
		<!-- 인코딩필터가 처리할 url-pattern을 설정. '/*' 모든 경로에 지정 -->
		<filter-mapping>
			<filter-name>encodingFilter</filter-name>
			<url-pattern>/*</url-pattern>
		</filter-mapping>
			
	</web-app>
```
<br>

###	3. root-context.xml
 - 서버 구동과 동시에 바로 셋팅할 내용들을 Bean으로 등록하는 파일
 - 모든 서블릿이 공유하는 Bean들을 모아둔 파일 / `공통 Bean`을 설정
 - 어플리케이션 영역의 설정으로 MVC 설정과 관련된 여러 처리 담당
 - 주로 비즈니스 로직 관련 설정 파일

```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd">
		
		<!-- 1. Database 연결 생성 -->
		<bean class="org.apache.commons.dbcp.BasicDataSource" id="dataSource" destroy-method="close">
			<property name="driverClassName" value="oracle.jdbc.driver.OracleDriver" />
			<property name="url" value="사용할url" />
			<property name="username" value="DB계정명" />
			<property name="password" value="DB비밀번호" />
		</bean>
		
		<!-- 2. sqlSessionFactory 설정
			*SqlSessionFactoryBean: Mabatis 구성 파일 없이도 SqlSessionFactory를 빌드할 수 있게 해주는 요소 -->
		<bean class="org.mybatis.spring.SqlSessionFactoryBean" id="sqlSessionFactory">
			<property name="configLocation" value="classpath:mybatis-config.xml" />
			<property name="dataSource" ref="dataSource" />
		</bean>
		
		<!-- 3. sqlSession bean에 sqlSessionFactory 객체를 주입 
			*SqlSessionTemplate: 마이바티스-스프링 연동모듈의 핵심으로 SqlSession을 구현하고 코드에서 SqlSession을 대체 -->
		<bean class="org.mybatis.spring.SqlSessionTemplate" id="sqlSession">
			<constructor-arg ref="sqlSessionFactory" />
		</bean>

		<!-- 파일업로드 관련 빈 -->
		<bean class="org.springframework.web.multipart.commons.CommonsMultipartResolver" id="multipartResolver">
			<property name="maxUploadSize" value="100000000" />
			<property name="maxInMemorySize" value="100000000" />
		</bean>
		
	</beans>
```

### 4. servlet-context.xml
 - 클라이언트의 요청과 관련된 객체를 정의 즉, view에 관련된 객체를 정의
 - DispatcherServlet과 관련된 설정
 - 클라이언트의 요청을 받아 핸들링을 통해 해당 컨트롤러를 찾아가 요청 처리 후 view에 출력

```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<beans:beans xmlns="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

	<!-- DispatcherServlet Context: defines this servlet's request-processing infrastructure -->

	<!-- 컴포넌트 스캔을 통해 만들어진 빈에 uri 맵핑 => 어노테이션 사용 가능하도록 해주는 -->
	<annotation-driven />	

	<resources mapping="/resources/**" location="/resources/" />

	<!-- view resolver(뷰리졸버, 뷰해석지) 
			jsp뷰의 접두어, 접미어 설정 => 파일명만 작성하면 스프링이 알아서 접두어,접미어 붙여서 전달해줌 -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>

	<context:component-scan base-package="com.kh.spring" />		
	<!-- 서비스에 필요한 빈(어노테이션 기준)을 찾아서 생성-->

	</beans:beans>
```