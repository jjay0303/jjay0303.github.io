---
layout: post
title: "[Mybatis]mybatis-config.xml파일"
date: 2022-01-10
excerpt: "mybatis-config.xml파일에 대해 알아보자"
categories: Mybatis
tags: [study, Mybatis, mybatis-config]
comments: true
---

<style>
	h3{
		background-color:#D2D2FF;
		color: gray;
	}
</style>

## 1. mybatis-config.xml파일이란?
 
 - mybatis에 필요한 설정들을 기술하는 파일

### 1) mybatis-config.xml 생성
 1. Java Resources 아래 Source Folder 안에 위치
 2. DTD file로 생성

### 2) 설정 태그들
#### (1)environment
 - 연동할 DB 정보 입력하는 태그

```javaStript
 <environments default="development">
	
    <environment id="development">
        
        <transactionManager type="JDBC" />
        
        <dataSource type="POOLED">
            <property name="driver" value="oracle.jdbc.driver.OracleDriver" />
            <property name="url" value="jdbc:oracle:thin:@localhost:1521:xe" />
            <property name="username" value="DB계정" />
            <property name="password" value="비밀번호" />
        </dataSource>
        
    </environment>
    
</environments>
```                   

 |종류|설명|
 |:---:|:---:|
 |transactionManager|JDBC : 수동 commit / MANAGED : 자동 commit | 
 |dataSource|POOLED : ConnectionPool 사용 O / UNPOOLED : 사용 x |

#### (2)setting
 - mybatis 구동시 선언할 설정들을 작성

 ```javaStript 
    <settings>
        <setting value="NULL" name="jdbcTypeForNull" />
    </settings>
 ```  

 ** 더 많은 name값은 공식 문서 참고

#### (3)typeAlias
 - VO/DTO 클래스들의 풀클래스명을 단순한 클래스명으로 사용하기 위한 별칭 등록

```javaStript
 <typeAliases>
    <typeAlias type="com.mybatis.board.model.vo.Board" alias="Board" />
 </typeAliases>
``` 

#### (4)mapper
 - 실행할 sql문들을 등록

```javaStript
 <mappers>
    <mapper resource="/mappers/board-mapper.xml" />
 </mappers> 
``` 
 

#### 출처

  - 수업자료
  - <a href="https://www.w3schools.com/">w3schools</a>
  - <a href="https://mybatis.org/mybatis-3/ko/getting-started.html">마이바티스 공식문서</a>



