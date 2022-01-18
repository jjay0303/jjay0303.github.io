---
layout: post
title: "[Spring] 파일업로드에 대해 알아보자"
date: 2022-01-17
excerpt: "파일업로드 순서와 사용하는 라이브러리에 대해 알아보자"
categories: Spring
tags: [study, Spring, Spring Library]
comments: true
---

## 파일업로드 순서와 사용하는 라이브러리에 대해 알아보자!

### 1. 파일 업로드 순서

### 1) 화면단 jsp에서 form 태그 속성 설정

```html
<form id="" method="post" action="mapping값" enctype="multipart/form-data">
	<input type="file" id="upfile" name="upfile">
</form>
```

 - method 속성값
 
	|get|post|
	|:---:|:---:|
	|url에 폼데이터 추가하여 전달|폼데이터를 별로도 첨부하여 전달|
	|길이 제한 있음|길이 제한 없음|
	|보안에 취약|보안성 높음|
	|브라우저에 캐시되어 저장=> 히스토리 o|캐시 X => 히스토리 x|

 - enctype 속성값
    - 폼 데이터가 서버로 제출될 때 해당 데이터가 인코딩되는 방법 명시<br>
	- form요소의 method 속성값이 post일 경우에만 사용가능
 
	|속성값|설명|
	|:---:|:---:|
	|application/x-www-form-urlencoded|기본값, 모든 문자들은 서버로 보내기 전 인코딩됨을 명시|
	|text/plain|공백(space)는 "+"로, 나머지 문자는 모두 인코딩되지 않음을 명시|
	|multipart/form-data|모든 문자 인코딩 x, 파일이나 이미지 전송시 주로 사용|


### 2) 사용할 라이브러리와 빈 등록
 
 - Apache Commons Fileupload과 Apache Commons IO 라이브러리를 pom.xml에 등록

 - root-context.xml에 관련 빈 등록

 ```xml
    <bean class="org.springframework.web.multipart.commons.CommonsMultipartResolver" id="multipartResolver">
	 	<property name="maxUploadSize" value="100000000" />
	 	<property name="maxInMemorySize" value="100000000" />
	</bean>
 ```

 |프로퍼티|타입|설명|
 |maxUploadSize|long|최대 업로드 가능한 바이트 크기. 기본값 -1(크기 제한 X)|
 |maxInMemorySize|int|디스크데 임시파일 생성 전 메모리에 보관할 수 있는 최대 바이트크기. 기본값 10240바이트|
 |defaultEncoding|String|요청을 파싱할 때 사용할 캐릭터 인코딩. 아무 값도 없을 때는 ISO-8859-1 사용|
 |uploadTempDir|Resource|임시디렉토리 지정|  


### 3) 원본파일의 이름을 업로드파일명으로 수정할 메소드 생성  
 
 - jsp에서 넘긴 input의 name(키값)을 이용하여 바로 매개변수로 받음

```java
    public String saveFile(MultipartFile upfile, HttpSession session) {
		String originName = upfile.getOriginalFilename();	// 원본명
		
		String upTime = new SimpleDateFormat("yyyyMMddHHmmss").format(new Date());	
        //업로드 년월일시분초를 문자열로 받기
		int ran = (int)(Math.random()*90000 + 10000);								
        // 5자리의 랜덤숫자 ( 10000 : 시작숫자 / 90000 : 랜덤숫자 총 갯수)
		String ext = originName.substring(originName.lastIndexOf("."));					
        // 원본파일 확장자
		
		String changeName = upTime + ran + ext;		// 수정된 파일명
		
		// 업로드할 폴더의 절대 경로 추출
		String savePath = session.getServletContext().getRealPath("/resources/uploadFiles/");
		
		try {
			upfile.transferTo(new File(savePath + changeName));
		} catch (IllegalStateException | IOException e) {
			e.printStackTrace();
		}
		
		return changeName;
	}
```

 - MultipartFile 인터페이스
	- 업로드한 파일 및 데이터를 표현하기 위한 용도
	- `org.springframework.web.MutipartFile`
	- 주요메소드
	
	|메소드|설명|
	|:---:|:---:|
	|getName()|파라미터 이름|
	|getOriginalFilename()|업로드한 파일 원래 이름|
	|isEmpty()|업로드 파일 존재유무, 없으면 true|
	|getSize()|업로드한 파일 크기|
	|transferTo(file dest)|업로드한 파일 데이터를 지정한 파일에 저장|


### 4) 필요한 곳에서 해당 메서드 호출 후 객체에 담아 DB에 보내고 결과를  trannsferTo를 이용해 지정한 파일에 담기
  


#### 참고
 - 수업자료
 - <a href='https://m.blog.naver.com/javaking75/140203390797'>[스프링] 스프링의 MVC - 파일 업로드 처리, 다운로드 처리</a>
 - <a href='https://docs.spring.io/spring-framework/docs/3.2.10.RELEASE/javadoc-api/org/springframework/web/multipart/commons/CommonsMultipartResolver.html'>Class CommonsMultipartResolver</a>
  - <a href='https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/multipart/MultipartFile.html'>Interface MultipartFile</a>