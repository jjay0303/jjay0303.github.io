---
layout: post
title: "[mybatis]RowBounds를 이용한 페이징처리"
date: 2022-01-17
excerpt: "페이징처리를 해보자"
categories: mybatis
tags: [study, mybatis]
comments: true
---

## RowBounds를 이용한 페이징처리

### 1. RowBounds란?
 - mybatis에서 제공하는 클래스로 offset과 limit을 이용해 페이징처리를 함

 ```java
  int offset = 100; // 건너뛸 게시글 갯수  ex) 2페이지 요청하면 1페이지의 게시글수를 건너뜀
  int limit =  10;  // 한 페이지에 표시될 게시글 수 
  RowBounds rowBounds = new RowBunds(offset, limit);
 ```

 - 사용자 요청 페이지까지의 데이터를 전부 조회 후 해당 페이지의 데이터를 출력
    👉 데이터가 대량일 경우 속도 저하 / 데이터가 계속 증가 시 성능 저하
    👉 소량의 데이터 조회시 유리

### 2. Pagination
 - 페이지 번호 클릭 시 해당 페이지로 이동하게 하는 기법

### 1) 순서 
 - 페이지 정보를 담을 PageInfo 객체 생성

 |변수명|의미|
 |:---:|:---:|
 |listCount|총 게시글 수|
 |currentPage|현재 페이지 번호|
 |pageLimit|한 블럭에 들어갈 페이지 수(5면 1,2,3,4,5 표시)|
 |boardLimit|한 페이지에 보여지는 게시글 수|
 |maxPage|전체 페이지 중 가장 마지막 페이지|
 |startPage|현재 페이징 버튼의 시작번호|
 |endPage|현재 페이징 버튼의 끝번호|

 ```java
 public class PageInfo {
	
	private int listCount;		// 총 게시글 수
	private int currentPage;	// 현재 페이지
	private int pageLimit;		// 총 페이지 수
	private int boardLimit;		// 한 페이지에 보여질 게시글수
	
	private int maxPage;		// 총 페이지수의 가장 끝 값 
	private int startPage;		// 현재 페이징의 시작
	private int endPage;        // 현재 페이징의 끝
 }   
 ```

 - `PageInfo pi = Pagination.getPageInfo(listCount, currentPage, pageLimit, boardLimit)`

#### 참고
 - 수업자료
 - <a href='https://okky.kr/article/145481'>url-pattern에서 / 와 /*의 차이점</a>