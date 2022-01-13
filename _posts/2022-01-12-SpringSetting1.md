---
layout: post
title: "[Spring] DTD mapper 설정하기"
date: 2022-01-12
excerpt: "DTD mapper 설정"
categories: Spring
tags: [study, Spring, Spring setting]
comments: true
---


## xml파일 생성시 DTD mapper까지 등록되어서 생성할 수 있도록 설정하기

### 1. XML Catalog에 DTD 추가
- window-preferences-XML-XML Catalog

![1](https://user-images.githubusercontent.com/93863500/149146974-bf506854-62a7-4c77-ae46-6e0269f5864c.JPG)
<br>

- User Specitied Entries에서 Add 클릭
- Location과 key값 넣은 뒤 OK(값은 Mybatis 공식사이트 참고)

![2](https://user-images.githubusercontent.com/93863500/149147311-3bb020f1-1aba-4483-bfc7-43940b30366d.JPG)

![3](https://user-images.githubusercontent.com/93863500/149147520-d86225f8-d7d4-4baf-8c4e-da94ab67af41.JPG)
<br>

- DTD가 추가됨
![4](https://user-images.githubusercontent.com/93863500/149147825-0428e0e2-e690-4077-95bd-b05364bb7b75.JPG)

<br>

### 2. xml파일 생성
- 파일명 설정 후 next
- Create XML file from a DTD file 선택 후 next

![5](https://user-images.githubusercontent.com/93863500/149148166-479dd086-6ea1-46b4-b30e-1c7593e9c722.JPG)
<br>

- Select XML Catalog entry 선택 후 해당하는 DTD 선택하고 next

![6](https://user-images.githubusercontent.com/93863500/149148276-c56c779a-a4e0-4805-b037-5ef4484aea7e.JPG)
<br>

- 정보 확인 후 Finish

![7](https://user-images.githubusercontent.com/93863500/149148355-376c0665-77e4-437e-804b-5a52e05ce8a2.JPG)
<br>

- 생성된 mapper파일에 DTD정보 추가된 것을 확인할 수 있음

![8](https://user-images.githubusercontent.com/93863500/149148510-a1ad8dbc-cd65-4de9-9b5f-fcadd5680e1e.JPG)
<br>

- 필요한 속성으로 mapper파일 설정해줌

![9](https://user-images.githubusercontent.com/93863500/149148594-7611114f-19d5-48bd-af35-78f9f02010e1.JPG)
<br>