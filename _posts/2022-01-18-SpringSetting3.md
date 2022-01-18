---
layout: post
title: "[Spring] taglib 코드가 등록되있는 jsp 파일 생성하기"
date: 2022-01-17
excerpt: "jsp파일 설정"
categories: Spring
tags: [study, Spring]
comments: true
---

## jsp파일 생성 시 taglib이 자동 등록된 채로 생성되도록 설정 해보자!

### 1. window - preference - Web - JSP Files - Editor - Templates로 이동

### 2. New JSP File(html5)를 찾아 더블클릭 or Edit

### 3. 해당 템플릿 Pattern에 원하는 코드 추가 후 Apply
 - 나의 경우에는 `<%@ taglib prefix="c" uri="http://java.sum.com/jsp/jstl/core" %>` 추가

![1](https://user-images.githubusercontent.com/93863500/149938240-1ddf4789-f1bf-4b16-ab20-14c8f8f874b8.JPG)


### 4. Apply 후 jsp파일을 생성해보면 다음과 같이 해당 코드가 등록된 채로 생성됨!

![2](https://user-images.githubusercontent.com/93863500/149938292-367fe2a6-3d3d-4fb4-8171-cccff2897103.JPG)

#### 참고
 - 수업자료