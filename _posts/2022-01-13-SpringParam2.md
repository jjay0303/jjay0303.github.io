---
layout: post
title: "[Spring] Controller에서 View로 데이터 전달하는 방법"
date: 2022-01-13
excerpt: "Controller에서 View로 데이터 전달"
categories: Spring
tags: [study, Spring, parameter]
comments: true
---

## 사용자 요청 처리 후 View로 데이터 전달하는 방법에 대해서 알아보자

### 1. Model 객체 사용
 - 포워딩할 뷰로 전달할 데이터를 맵형식(key-value 세트)로 담을 수 잇는 영역. 즉, 데이터만 담김
 - 단, addAttribute 메소드 이용

 ```java
  @RequestMapping("login.me")
  public String loginMem(Member m, Model model, HttpSession session){
      Member loginUser = mService.loginMem(m);
		
		if(loginUser == null) {	
			model.addAttribute("errorMsg", "로그인 실패");
			return "/common/errorPage";		
		}else {	
			session.setAttribute("loginUser", loginUser);
			return "redirect:/";		
		}
  }
 ```

### 2. ModelAndView 객체 사용
 - 응답뷰에 대한 정보를 Views에 바로 넘기는 방식 즉, 데이터+이동할 View Page가 같이 담김

 ```java
  @RequestMapping("login.me")
	public ModelAndView loginMember(Member m, HttpSession session, ModelAndView mv) {	
		
		Member loginUser = mService.loginMem(m);
		
		if(loginUser == null) {	
			mv.addObject("errorMsg", "로그인실패");
			mv.setViewName("/common/errorPage");
			
		}else {	
			session.setAttribute("loginUser", loginUser);
			mv.setViewName("redirect:/");
		}
		
		return mv;
	}
 ```

 #### 1) addObject
  - key와 value를 담아서 보낼 수 있는 메소드

 #### 2) setViewName
  - 데이터를 보낼 페이지를 지정
