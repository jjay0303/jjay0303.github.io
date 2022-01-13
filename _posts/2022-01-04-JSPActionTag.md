---
layout: post
title: "[JSP]JSP ActionTag"
date: 2022-01-04
excerpt: "JSP ActionTag란?"
categories: JSP
tags: [study, JSP]
comments: true
---

<style>
	h3, table>tr>th{
		background-color:#D2D2FF;
		color: gray;
	}
	table{
		table-layout:fixed;
		word-break:break-all;
	}
</style>


## JSP Action Tag
XML기술을 이용하여 기존 JSP문법을 확장시키는 기능을 제공하는 태그<br>


## 1.표준액션태그
<p>미리 정해진 기능들이 JSP 스펙에 명시돼있어서 jsp라는 접두어와 태그명을 제시하면 컨테이너가 해당 기능을 수행하도록 만든 기본 액션 태그 </p>
### 1) jsp:include 
다른 페이지를 포함할 때 사용하는 태그<br>
- 표현식 <jsp:include page="포함할 JSP 페이지"&gt;
<br>

<h4> * 기존 방식과 비교 </h4>
<table>
	<tr align="center">
		<th>기존방식</th>
		<th>표준액션태그이용</th>
	</tr>
	<tr align="center">
		<td>include file="footer.jsp"</td>
		<td>jsp:include page="footer.jsp"</td>
	</tr>
	<tr align="center">
		<td>정적include == 컴파일 시 포함</td>
		<td>동적include == 런타임 시 포함</td>
	</tr>
	<tr align="center">
		<td>include하는 페이지와 변수명을 공유하므로 변수명 중복 X</td>
		<td>변수명을 공유하지 않으므로 동일 이름의 변수 재선언 가능</td>
	</tr>
	<tr align="center">
		<td>오로지 include만 가능</td>
		<td>include 할 페이지로 키/벨류 형식으로 값 전달 가능 => 해당 페이지에서 값 뽑아서 출력 가능</td>
	</tr>
</table>

<br><br>

### 2) jsp:forward
현재 실행중인 JSP페이지의 제어 흐름을 특정 JSP로 넘길 때 사용하는 태그<br>
&nbsp;&nbsp;- 표현식 <jsp:forward page="포워딩할 JSP 페이지"&gt;

#### * include와 forward의 차이 
<table>
	<tr align="center">
		<th>include</th>
		<th>forward</th>
	</tr>
	<tr align="center">
		<td>include file="footer.jsp"</td>
		<td>jsp:include page="footer.jsp"</td>
	</tr>
	<tr align="center">
		<td>정적include == 컴파일 시 포함</td>
		<td>동적include == 런타임 시 포함</td>
	</tr>
	<tr align="center">
		<td>include하는 페이지와 변수명을 공유하므로 변수명 중복 X</td>
		<td>변수명을 공유하지 않으므로 동일 이름의 변수 재선언 가능</td>
	</tr>
	<tr align="center">
		<td>오로지 include만 가능</td>
		<td>include 할 페이지로 키/벨류 형식으로 값 전달 가능 => 해당 페이지에서 값 뽑아서 출력 가능</td>
	</tr>
</table>

<br><br><br>

## 2.커스텀액션태그(JSTL)
<p> JSP 태그 라이브러리를 추가하여 스트립틀릿 태그 사용 없이 보다 간편하게 JSP를 사용할 수 있게 해주는 태그로 대표적으로 JSTL이 있다. </p>

<h4> * 주요 tag들 </h4>
<table id="third">
	<tr align="center">
		<th>태그명</th>
		<th>기능</th>
		<th>url</th>
	</tr>
	<tr align="center">
		<td>core</td>
		<td>변수 지정, 반복문, 조건문 등 흐름제어, url/쿼리스트링 관련</td>
		<td> <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"> </td>
	</tr>
	<tr align="center">
		<td>fmt</td>
		<td>번호 및 날짜 형식화 지원</td>
		<td> <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%> </td>
	</tr>
	<tr align="center">
		<td>fn(function)</td>
		<td>문자열 관련 함수</td>
		<td> <%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %> </td>
	</tr>
	<tr align="center">
		<td>XML</td>
		<td>흐름 제어, 변환 등 제공</td>
		<td> <%@ taglib prefix="x" uri="http://java.sun.com/jsp/jstl/xml" %> </td>
	</tr>
	<tr align="center">
		<td>SQL</td>
		<td>SQL 지원</td>
		<td> <%@ taglib prefix="x" uri="http://java.sun.com/jsp/jstl/sql" %> </td>
	</tr>
</table>

<br><br>

### 1.Core Library
<p>변수 지정, 반복문, 조건문 등 흐름제어, url/쿼리스트링 관련</p>
<br>

1. <b><c:set var="변수명" value="대입값" [scope=""]&gt;</b>

	- 변수를 선언하고 초기값을 대입하는 기능<u>(무조건 선언시 초기값 작성)</u><br>
	- 해당 변수를 어떤 scope에 담을지 지정가능(생략시 기본 pageScope)<br>
	- EL로 접근해서 사용가능<u>(스크립팅원소로는 접근 불가)</u>
	
	<br>
2. <b><c:remove var="제거할변수명" [scope=""]&gt;</b>
	
	- 해당 scope영역에서 해당 변수를 찾아 제거<br>
	- scope 지정 생략시 모든 scope에서 해당 변수 다 찾아서 제거<br>
	
	<br>
3. <b><c:out value="출력할값" [default="기본값"] [escapeXml="true|false"]&gt;</b>
	
	- 데이터 출력 시 사용<br>
	
	<br>
4. <b><c:if test="조건식"&gt;</b> 
	
	- JAVA의 단일 if문과 비슷<br>
	- 조건식은 EL구문으로 test속성에 작성<br>
	
	<br>
5. <b><c:choose, c:when, c:otherwise&gt;</b> 
	
	- JAVA의 if-else, if-else if문과 비슷<br>
	- 각 조건들을 c:choose의 하위요소로 c:when을 통해서 작성<br>
	
	<br>
6. <b><c:forEach var="변수명" begin="초기값" end="끝값" [step="반복시증가값"]&gt;</b>
	
	- JAVA의 for loop문<br>
	
	<br>
7. <b><c:forEach var="변수명" items="순차적으로 접근하고자 하는 객체(배열|컬렉션)" [varStatuss="현재접근된요소의상태값을보관할변수명"]&gt;</b>
	
	- JAVA의 향상된 for문<br>
	
	<br>
8. <b><c:forTokens var="변수명" items="분리시키고자하는문자열" delims="구분자"&gt;</b>
	
	- JAVA의 split("구분자") 또는 StringTokenizer와 비슷<br>
	- 구분자로 분리된 각각의 문자열에 순차적으로 접근하면서 반복 수행 <br>
	
	<br>
9. <b><c:url var="" value="요청할url주소">
	<c:param name="키값" value="전달값" &gt;</b>
	
	- url경로를 생성하고, 쿼리스트링을 정의해 둘 수 있는 태그<br>
	


<br><br>

### 2. fmt
<p>번호 및 날짜 형식화 지원</p>
<br>

1. <b><fmt:formatNumber value="출력할 값" [groupingUsed="true|false" type="percent|currency" currencySymbol="통화기호"]></b>

	- formatNumber 태그로 숫자 형식 지정<br>
	- groupingUsed : 세자리마다 구분자 표시 여부<br>
	- type : 백분율(%), 통화기호 형식 지정<br>
	- currencySymbol : 통화 기호 문자 지정<br>
<br>

2. <b><c:set var="current" value="<%= new java.util.Date() %>"&gt;</b>

	- formatDate 태그로 java.util.Date 객체 이용하여 날짜 및 시간 포멧 지정<br>
	- groupingUsed : 세자리마다 구분자 표시 여부<br>
	- type : 백분율(%), 통화기호 형식 지정<br>
	- currencySymbol : 통화 기호 문자 지정<br>
<br>


<br><br>

### 3. fn 
<p>문자열 관련 함수</p>
<br>

1. <b>${ fn:length(str) }</b>

	- 문자열의 길이 출력<br>
<br>

2. <b>${ fn:toUpperCase(str)|toLowerCase(str) }</b>

	- 모두 대문자|소문자 출력<br>
<br>

3. <b>${ fn:indexOf(str, 'are') }</b>

	- 'are'의 시작 인덱스<br>
<br>

4. <b>${ fn:replace(str, "are", "were") }</b>

	- 'are'을 'were'로 변경<br>
<br>

5. <b>${ fn:contains(str, 'are') }</b>

	- str이 'are'을 포함하는지<br>
<br>
