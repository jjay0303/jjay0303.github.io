---
layout: post
title: "[Spring] Encodingfilter "
date: 2022-01-14
excerpt: "Encoding 필터 등록"
categories: Spring
tags: [study, Spring, Spring setting]
comments: true
---

## Encoding 필터 등록하는 방법에 대해 알아보자

한글 깨짐을 방지하기 위해 Spring에서 제공하는 encodingfilter를 등록하여 사용해봅시다.

### 1. web-xml에 filter태그로 encodingfilter를 등록
```javaScript
<filter>
	<filter-name>encodingFilter</filter-name>
	<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
	<init-param>
		<param-name>encoding</param-name>
		<param-value>UTF-8</param-value>
	</init-param>
</filter>
```

### 2. web-xml에 encodingfilter에 맵핑할 url도 등록하면 끝! 
```javaScript
<filter-mapping>
	<filter-name>encodingFilter</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
```
 
### /*과 /의 차이
 1. /* : 들어오는 모든 url패턴을 처리
 2. / :	모든 url-mapping을 비교해도 매칭이 안되는 패턴을 처리


#### 참고
 - 수업자료
 - <a href='https://okky.kr/article/145481'>url-pattern에서 / 와 /*의 차이점</a>