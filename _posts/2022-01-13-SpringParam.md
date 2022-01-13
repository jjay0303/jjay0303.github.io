---
layout: post
title: "[Spring] Controller에서 parameter 받는 방법들 4가지"
date: 2022-01-13
excerpt: "Controller에서 parameter 받는 방법 4가지"
categories: Spring
tags: [study, Spring, parameter]
comments: true
---

## 사용자가 서비스 요청시 전달값들을 넘겨받는 방법에 대해 알아보자

### 1. HttpServletRequest 이용(기존 jsp/Servlet방식)
 - 해당 메소드의 매개변수를 HttpServletRequest로 해서 스프링 컨테이너가 해당 메소드 호출시 자동으로 해당 객체 생성하고 인자로 주입

 ```java
   @RequestMapping("login.me")
   public String loginMem(HttpServletRequest request){

      String userId = request.getParameter("id");
      String userPwd = request.getParameter("pwd");
			
		return "main";
   }
 ```

### 2. @RequestParam 어노테이션 이용
 - request.getParameter의 역할을 대신 해줌

 ```java
   @RequestMapping("login.me")
   public String loginMem(@RequestParam(value="id", defaultValue="admin") String userId,
                            @RequestParam(value="pwd") String userPwd){

      String userId = request.getParameter("id");
      String userPwd = request.getParameter("pwd");
			
		return "main";
   }
 ```

### 3. @RequestParam 어노테이션 생략 
 - 키값을 아얘 매개변수로 넣어도 스프링이 알아서 받는다... 똑똑하군

 ```java
   @RequestMapping("login.me")
   public String loginMem(String id, String pwd){
      
      Member m = new member();
      m.setUserId(id);
      m.setUserPwd(pwd);

      return "main";
   }
 ```

### 4. 커맨드 객체 방식
 - 해당 메소드의 매개변수로 요청시 담고자 하는 vo클래스 타입을 세팅 후 요청시 전달값의 키값을 vo클래스의 담고자하는 필드명으로 작성<br>
 👉 내부적으로 스프링 컨테이너가 해당 객체를 기본생성자로 생성 후 **setter**를 찾아 요청시 전달값을 해당 필드에 담아줌<br>
 ❗❗ 단, 반드시 키값과 담고자하는 필드명이 같아야 함 ❗❗
 
 ```java
   @RequestMapping("login.me")
   public String loginMem(Member m){

      Member loginUser = mService.loginMem(m);

      return "main";

   }
 ```