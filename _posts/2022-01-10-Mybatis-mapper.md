---
layout: post
title: "[Mybatis]mapper.xml파일"
date: 2022-01-10
excerpt: "mapper.xml파일에 대해 알아보자"
categories: Mybatis
tags: [study, Mybatis, mapper]
comments: true
---

<style>
	h3{
		background-color:#D2D2FF;
		color: gray;
	}
</style>

## 1. xml파일이란?
 
 - EXtensible Markup Language의 약자
 - 데이터를 저장하고, 저장되는 데이터의 구조를 기술하여 전달할 목적으로 만들어진 언어<br>

### 1) 특징
   - 다른 목적의 마크업 언어를 만다는데 사용되는 다목적 마크업 언어
   - 다른 시스템끼리 다양한 종류의 데이터를 쉽게 교환하게 해줌
   - 새로운 태그 추가도 가능하고 사용자가 직접 정의도 가능
   - 텍스트 데이터 형식의 언어로 유니코드 문자로만 이루어짐

### 2) 목적
   - 서로 호환되지 않는 데이터 타입을 사용하는 시스템 간의 데이터 교환은 시간이 많이 걸리고 데이터 손실도 발생함<br>
   => 데이터를 텍스트 형식으로 저장하여 os나 브라우저 등에 상관없이 독립적으로 데이터를 저장하고 서로 다른 주체간에 정보를 쉽게 전달할 수 있다.

### 3) 스키마
   - xml이 다른 언어를 정의하기 위해 해당 언어의 필요 요소와 속성 정보를 모아놓은 것
   - 작성방법 : DTD / XSD<br>
 
#### (1)DTD(Document Type Definition)
   - XML문서의 구조 및 해당 문서에서 사용할 엔티티 등을 정의함
   - 문서 내부 명시 또는 별도의 파일로 분리
   - 데이터 교환의 표준으로써 활용되거나 XML문서의 구문 및 구조에 대한 유효셩 검사 가능
   - `<!DOCTYPE 루트요소 DTD식별자 [ 선언1 선언2 ... ]>`
<br><br>

## 2. mapper.xml
### 1) XML과 DTD 선언
```javaScript
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
```

### 2) <mapper> 작성
 - SQL mapper의 루트 엘리먼트로 모든 SQL문은 mapper 안에 기술함
 - namespace : 해당 mapper파일의 고유한 별칭 등록

### 3) select, insert, update, delete
#### (1) DML문
 `<insert|update|delete id="식별자" [parameterType="전달받을자바타입(패키지포함풀클래스명)|별칭"]> </insert|update|delete>`
 - 실행결과가 int이므로 result속성이 필요 X

 ```javaScript
<update id="increaseCount" parameterType="_int">
    update
            board
        set count = count+1
        where board_no = #{boardNo}
        and status = 'Y'
</update>
 ```

 |종류|설명|
 |:---:|:---:|
 |id|해당 SQL문의 구분자(고유 이름)|
 |parameterType|구문에 전달된 파라미터의 전체 클래스명이나 별칭|


#### (2) SELECT문
 `<select id="식별자" [parameterType="전달받을자바타입(패키지포함풀클래스명)|별칭"] resultType="조회결과반환하고자하는자바타입" | resultMap="조회결과를뽑아서mapping할resultMap의 id"> </select>`

 ```javaScript
<select id="selectListCount" resultType="_int">
    select
           count(*)
      from board
     where status = 'Y'
</select>
 ```

 |종류|설명|
 |:---:|:---:|
 |resultType|select문 실행 결과를 담을 객체|
 |resultMap|select문 실행 결과를 담을 객체를 resultMap으로 지정, 따로 선언해야함|

#### - resultMap
 - sql컬럼값과 vo객체의 필드값을 매핑시키는 역할로 **마이바티스의 핵심 기능** 
 - 데이터가 있으면 뽑아서 넣고 없으면 말고

 ```javaScript
<resultMap id="replyResultSet" type="Reply">
    <result column="reply_no" property="replyNo" />
    <result column="user_id" property="replyWriter" />
    <result column="reply_content" property="replyContent" />
    <result column="create_date" property="createDate" />
</resultMap>

<select id="selectReplyList" resultMap="replyResultSet" parameterType="reply">
    select
            reply_no
            , reply_content
        from reply r
        join member on (reply_writer=user_no)
        where ref_bno = #{boardNo}
</select>
 ```

#### - #{}
 - PreparedStatement를 만들어서 파라미터에 값을 세팅하기 위한 문법
 - ''가 자동 삽입되면서 String 형태로 값이 들어감


### 3) mybatis-config.mxl에 해당 mapper 파일 등록
 - mapper 태그 이용 

```javaStript
<mappers>
    <mapper resource="/mappers/board-mapper.xml" />
</mappers>
```
 
#### 출처

  - 수업자료
  - <a href="https://www.w3schools.com/">w3schools</a>
  - <a href="https://mybatis.org/mybatis-3/ko/getting-started.html">마이바티스 공식문서</a>



