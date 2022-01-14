---
layout: post
title: "[Spring] Lombok(롬복) Library "
date: 2022-01-14
excerpt: "Lombok(롬복)에 대해 알아보자"
categories: Spring
tags: [study, Spring, Spring Library]
comments: true
---

## Lombok(롬복)에 대해 알아보자!

### 1. Lombok(롬복)이란?
 - 객체안의 생성자, getter, setter, toString을 Annotation 기술만으로 자동 생성하고 변경사항도 반영해주는 라이브러리

### Lombok의 장단점
 1. 장점 
	- 변수 수정이 잦을 경우 편리
	- 어노테이션 기반 코드를 자동 생성해주므로 생산성 향상
	- 가독성과 유지보수성 향상
 
 2. 단점
	- 프로젝트 공유하는 팀원 모두가 세팅이 되어있어야 함
	- API와 내부동작을 숙지하고 있어야함

### 3. Lombok 설치과정

#### 1) 라이브러리 다운
 - maven을 이용

```javaScript
<dependency>
	<groupId>org.projectlombok</groupId>
	<artifactId>lombok</artifactId>
	<version>1.18.12</version>
	<scope>provided</scope>
</dependency>
```

#### 2) 다운로드된 jar파일 찾아서 작업할 IDE에 설치

#### 3) IED 재실행

### 4. Annotation
 1. NoArgsConstructor : 기본생성자 생성
 2. AllArgsConstructor : 매개변수생성자 생성
 3. Setter 
 4. Getter
 5. ToString
 6. RequiredArgsConstructor : 초기화되지 않은 final필드나 @NonNull필드에 대한 생성자 생성

### 5. 주의사항
 - 소문자 1개로 시작하는 필드명은 사용 XXX ex)pName <br>
	👉 lombok의 네이밍 규칙이 소문자를 대문자로 바꿔버림


#### 참고
 - 수업자료