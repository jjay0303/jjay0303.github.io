---
layout: post
title: "[Spring] Interceptor"
date: 2022-01-24
excerpt: "Interceptor에 대해 알아보자"
categories: Spring
tags: [study, Spring]
comments: true
---

## Interceptor에 대해 알아보자

### 1. Interceptor란?
 - 사용자의 요청을 받은 Controller가 핸들러를 호출하기 전/후에 낚아채서 추가작업을 한 뒤 핸들러로 보낼 수 있도록 해주는 것.
 - 핸들러마다 세션 체크를 한다면 서버의 부하가 늘어나고 메모리가 낭비되지만 인터셉터 클래스에 한번만 작성한 뒤 인터셉터가 필요한 url을 servlet-context.xml에 설정하면 스프링에서 일괄적으로 해당 url의 핸들러에 인터셉터를 적용해주므로 관리가 더 용이함.

### 2. Interceptor 동작 위치 및 순서
 1. 사용자가 요청을 보냄
 2. Dispatcher Servlet이 해당 Request를 받아 핸들러 매핑으로 해당 핸들러를 찾음
 3. 핸들러 실행체인(HandlerExectuonChanin)이 등록된 인터셉터를 거친 뒤 controller를 실행함 

### 3. 메서드
 - 스프링이 제공하는 `HandlerInterceptor` 인터페이스와 `HandlerInterceptorAdapter` 추상클래스에 정의되어있는 메서드는 preHandle(), postHandle(), afterCompletion()이 있다.

 1. preHandle()
    - 컨트롤러 호출 전 실행되며 controller로 가기 전 처리할 작업이 있거나 사용자의 request를 가공 또는 추가해야할 경우 사용
    - `boolean`값을 리턴하여 값이 true인 경우 preHandle()실행 후 핸들러에 접근 / false인 경우 작업 중단

 2. postHandle()  
    - 핸들러 실행 후 View로 가기 전 호출
    - ModelAndView 타입 정보가 인자값으로 받으므로 Model객체의 정보를 참조하거나 조작 가능
    - 인터셉터가 여러개인 경우 역순 호출
    - 비동기적 요청처리에는 처리 X
 
 3. afterCompletion()
    - 모든 작업이 완료된 후 실행되므로 요청 처리중에 사용한 리소스를 반환하기 적당한 메서드

### 4. 스프링에서 구현
 1) HandlerInterceptor 혹인 HandlerInterceptorAdater를 상속받아 클래스를 생성
  
  - 회원만 이용 가능한 서비스에 대한 요청을 인터셉터 

  ```java
    package com.spring.common.interceptor;

    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import javax.servlet.http.HttpSession;

    import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;

    public class LoginInterceptor extends HandlerInterceptorAdapter{
        
        @Override
        public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception{
            
            HttpSession session = request.getSession();
            
            if(session.getAttribute("loginUser") != null) { 
                return true;
            }else { 
                session.setAttribute("alertMsg", "로그인 후 이용바랍니다.");
                response.sendRedirect(request.getContextPath());
                return false;
            }
            
        }

    }
  ```
  <br>

 2) 해당 인터셉터를 거쳐갈 url을 servlet-context.xml에 등록

  ```xml
    <interceptors>
      <interceptor>
        <mapping path="/myPage.me" />
        <mapping path="/update.me" />
        <mapping path="/enrollForm.bo" />
        <beans:bean class="com.spring.common.interceptor.LoginInterceptor" id="LoginInterceptor" />
      </interceptor>
    </interceptors>
  ```

#### 참고
 - 수업자료
 - <a href='https://popo015.tistory.com/115'>[Spring] 스프링 인터셉터(Interceptor)란?</a>