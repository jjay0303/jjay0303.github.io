---
layout: post
title: "[Spring] 스프링에서 Ajax로 데이터 전달"
date: 2022-01-20
excerpt: "스프링에서 Ajax를 써보자"
categories: Spring
tags: [study, Spring, Ajax]
comments: true
---

## 스프링에서 Ajax로 데이터 전달하는 방법

### 먼저 jsp에서 Controller로 데이터를 넘겨보자

```jsp
   <h3>1. 요청시 값 전달, 응답 결과 받기</h3>
	하고싶은 말 : <input type="text" id="sentence"> <br>
	<button onclick="test()">전송</button>
	
	<!-- 응답데이터 있을 경우 출력할 영역 -->
	<div id="result"></div>
	
	<script>
		function test(){
			$.ajax({
				url:"ajax.do",
				data:{
					name:$("#sentence").val()
				},success:function(result){
					console.log(result);
					$("#result").text(result);
				},error:function(){
					console.log("ajax통신 실패");
				}
			});
		}
	</script>
```

### 해당 데이터를 처리한 뒤 Controller에서 jsp로 데이터를 보내는 방법은 2가지가 있다.

### 1. HttpServletResponse 객체로 응답하기 
 - 기존 jsp/servlet의 Stream방식

```java
@Controller
public class AjaxController {

   @RequestMapping("ajax.do")
	public void ajax1(String sentence, HttpServletResponse response) throws IOException {
		String responseData = "응답문자열: " + sentence;
		
		response.setContentType("text/html; charset=UTF-8");
		response.getWriter().print(responseData);			

	}
} 
```

### 2. 응답 데이터를 문자열로 리턴
   - 단, 문자열 리턴은 원래 `포워딩 방식`으로 리턴하는 문자열을 응답뷰로 인식하여 해당 뷰페이지를 찾으므로 404 에러가 발생<br>
   👉 @ResponseBody 어노테이션을 이용하여 문자열이 응답뷰가 아닌 응답 데이터라는 것을 명시해줌
   👉 단, 응답데이터에 한글이 포함된 경우 RequestMapping에서 `produces 속성`으로 content타입 지정

```java
@Controller
public class AjaxController {

   @ResponseBody
	@RequestMapping(value="ajax.do", produces="text/html; charset=UTF-8")
	public String ajax1(String sentence) {
		String responseData = "응답문자열: " + name + "," + age;
		return responseData;	
	}
}   
```

### 결과화면

![1](https://user-images.githubusercontent.com/93863500/150482959-bacb7015-a23d-4d29-b5fd-5687f9005b7c.JPG)


#### 참고
 - 수업자료