---
layout: post
title: "[mybatis]selectOne/selectList"
date: 2022-01-17
excerpt: "selectOne/selectList에 대해 알아보자"
categories: mybatis
tags: [study, mybatis]
comments: true
---

## selectOne과 selectList에 대해 알아보자

### 1. SqlSession
 - mybatis에서 제공하는 클래스로 구문 실행, 트랜젝션 커밋/롤백, mapper 관련 메소드 등등을 제공. 즉, CRUD를 할 수 있는 메소드들을 제공하는 클래스

### 2. selectOne
 - `selectOne(query문id)`
 - select문 실행 후 오직 하나의 객체만 리턴할 때 사용
 - xml에서의 resultType과 selectOne으로 받는 데이터 타입을 맞춰서 받아줌
 - 값이 없으면 null 반환

### 3. selectList
 - `selectList(query문id, '조건')` 
 - select문을 실행하면서 조건을 전달
	- 조건 : 쿼리문에서 사용하는 인자
 - 퀴리문의 parameterType이라는 속성에서 해당 조건을 받아주므로 조건의 데이터타입을 명시해둠
 - 쿼리의 결과를 List<E>로 반환하며 결과가 없으면 빈 List 반환


#### 참고
 - 수업자료
 - <a href='https://mybatis.org/mybatis-3/ko/java-api.html#sqlSessions'>url-pattern에서 / 와 /*의 차이점</a>