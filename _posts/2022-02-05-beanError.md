---
layout: post
title: "[Spring] 스프링 에러를 탈출하기 까지"
date: 2022-02-05
excerpt: "Spring 에러"
categories: Spring
tags: [study, Spring]
comments: true
---

 파이널 프로젝트를 시작하고 드디어 back-end단에 들어오면서 mapper와 dao, service 클래스들을 만든 순간,,,
 404에러가 뜨면서 서버에 아얘 연결이 되지 않았고....🤣 
 더 환장인건 그냥 ***요청된 리소스/root명/는 가용하지 않습니다*** 라는 말만 뜰뿐 다른 이유가 뜨지 않는 것이다.... 
 구글링을 통해 각종 xml 파일들에서 설정과 어노테이션 확인, 프로젝트 및 서버 clean 등등 각종 디버깅 방법을 해보면서
 스프링이 왜 악명 높은지 몸소 체험하면서 스프링에 대한 심도있는(?) 공부를 하면서 유익한 주말을 보냈다...🤪 
 그리고 이 과정을 절대 잊지 않기 위해 정리해둔다. 여기서는 대략적인 흐름을 정리하고 세부사항은 각각 따로 포스팅하여 상세하게 정리하려 한다.

<hr>

### 1. 에러의 시작 : mapper파일과 dao, service, serviceImpl 클래스를 만듦
 - 난 단지 클래스 몇개와 xml파일을 만들었을 뿐인데 서버가 구동이 되지 않아 멘붕이 왔다
 - 일단 여기서 확인해야 할 사항으로는
 1. db설정
 2. 어노테이션
 3. mapper파일 오타

#### 1_1) db설정
 - mapper의 db정보를 읽어오기 위해 Mybatis연동을 해야한다
 - `pom.xml`에 `<dependency>` 속성을 이용하여 Mybatis에 필요한 의존성 등록
 - `root-context.xml`에 `<bean>`과 `<property>` 속성을 이용하여 사용할 DB명과 계정, 비밀번호 입력

#### 1_2) 어노테이션 확인
 - 스프링은 `어노테이션`을 이용하여 하나의 컨트롤러 안에서 메소드 단위로 사용자의 요청을 처리할 수 있다.
 - 어노테이션을 통해 Bean으로 등록하거나, Bean에 의존관계 주입을 하는 등의 역할이 가능하다
 - 즉, 생성한 클래스에 맞게 어노테이션을 적어야 한다. 나의 경우 @Controller / @Service / @Repository 어노테이션 설정이 필요했음

<br>
<hr>

### 2. UnsatisfiedDependencyException과 NoSuchBeanDefinitionException 

#### 2_1) NoSuchBeanDefinitionException
 - `@Autowired` 어노테이션 사용 시 대상 객체가 메모리에 없을 때 발생한다고 한다.
 - 즉, 해당 Autowired 사용 시 Bean 객체가 필요한데 그런 Bean을 찾을 수 없다는 것. 즉 bean 생성에 문제가 있다는 오류
 - 따라서 해당 객체를 생성하는 xml 확인, 해당 클래스의 어노테이션 확인, dispatch-servlet.xml에 component-scan 확인, web.xml에 listener 확인 

#### 2_2) UnsatisfiedDependencyException
 - 그래도 계속 동일한 에러가 발생하길래 이번에 이 에러를 자세히 보니 serviceImpl과 sqlSessionTemplate에서 에러가 난다고 되어있어서 어노테이션을 확인해보았지만 다 잘 써잇었음
 - xml문에 오타가 있어도 그럴 수 있다길래 xml문도 확인해보고 아얘 삭제도 해보았지만... 

<hr>
- 위의 것들을 다 확인 했는데도 여전히 에러가 뜨길래 어차피 코드 작성은 아직 안했으므로 새로 만든 파일들은 다 지우고 root-context의 db설정도 다 주석처리 해보았다. 그런데... 이번에는 404가 뜨는 것이 아닌가...? 멘붕
- 다른 컴퓨터에서 실행해보고 싶어서 내 브런치에 푸시 해두고 인강용 놋북에 풀 받아서 실행해보니... 놋북에서는 또 잘 돌아감????? 혼돈이 도가니
- 더 무서웠던 것은 전날에 내가 화면 로딩은 먼저 해보려고 Controller를 만들어서 작동 확인까지 하고 메인에 푸시를 해놨었는데 그걸 풀받은 동료는 갑자기 사진데이터가 안뜨는 현상이 발생... 대난장판😵
- 내가 만든 컨트롤러를 지워보라고 했더니 사진이 잘 뜬다고 하길래... 나도 컴퓨터로 돌아와 컨트롤러까지 지워봤는데 난 여전히 404 에러가 발생하고..🤬
- 다시 기도하면서 구글선생님에게 폭풍 검색 시작


<br>

<hr>
### 3. <요청된 리소스/root명/는 가용하지 않습니다>

#### 3_1) root 경로 확인
 - 나의 경우 팀원들과 경로를 똑같이 맞춰놨기 때문에 해당이 없었지만 `root 경로` 확인도 필요하다고 한다.
 - 프로젝트 우클릭 - Properties - Web Project Settings의 context root 경로와
 - 서버탭의 톰캣 더블클릭 - modules - Path의 경로가 일치해야 한다!
 ❓ 구글링을 해보면 다들 context root를 /로 하라고 하던데 나중에 이유를 더 찾아봐야 겠다. 우리는 프로젝트명으로 했는데 보안상 문제가 되나? 

#### 3_2) 톰캣 삭제, 재설정 후 restart

#### 3_3) index.jsp가 아닌 프로젝트를 선택하여 실행
 - view 폴더의 jsp 파일을 실행하면 외부에서 중요한 파일이 많은 WEB-INF에 직접 접근하는 식으로 되어서 스프링이 막아버린다. 
 - 따라서 프로젝트를 선택해서 실행하면 web.xml에서 DispatcherServlet이 servlet-context.xml로 요청을 전달하고
 - servlet-context에서 HandlerMapping과 HandlerAdaapter작업을 함
    - `<context:component-scan>`으로 해당 클래스를 찾아 bean으로 등록
    - `<annotation-driven /> `으로 URL 매핑을 하여 지정된 URL을 생성(나의 경우는 index 앞에 /WEB-INF/views, 뒤에 .jsp를 붙여 실행시켜 주는 것이다.)

#### 3_4) maven 라이브러리 재설치
 - `C:\Users\계정명\.m2` 폴더의 모든 폴더를 삭제 후 STS 실행하면 자동으로 메이븐 라이브러리 재설치
 - 또는 pom.xml의 <org.springframework-version> 버전을 수정하여 메이븐 라이브러리를 재설치
 👉 프로젝트 탭에서 Maven-Dependencies 라이브러리들 잘 설치 됐는지 확인 가능

#### 3_5) maven update
 - 프로젝트 우클릭-Maven-Update Project

#### 3_6) 내 컴퓨터에 사용자 이름이 한글도 되어 있는데 혹시 이게 문제인가? 싶어서 이참에 사용자 이름도 영어로 바꿈... 하지만 실패ㅠㅠ

<br>
<hr>

### 4. 틀린그림찾기..
 - 여기까지 다 해도 컴퓨터에서는 계속 안되길래 정말 컴퓨터와 놋북 동시에 프로젝트를 켜두고 실행파일 하나하나 비교를 하고 오타를 찾아다녔다^^:;(틀린그림찾기를 좋아하긴 함..)
 - 그러다가 pom.xml에서 빨간줄을 발견! **Missing artifact oracle:ojdbc6:jar:11.2.0.3**
 - 1_1)에서 mybatis의존성 주입을 할 때 `ojdbc6`을 사용하는 경우 maven에서 라이브러리 제공을 자동으로 안해줘서 pom.xml에 사설 라이브러리를 등록해야 했던 것!
 - 나의 경우 pom.xml에 다음 코드를 추가해줌
 ```xml
    <repositories>
		<repository>
			<id>Datanucleus</id>
			<url>http://www.datanucleus.org/downloads/maven2/</url>
		</repository>
	</repositories>
 ```
 여기까지 하고 디버깅 실행해봤더니 또 다른 오류 발생! 그래도 404가 아니라 기쁜 마음^^;;

<br>
<hr>

### 5. org.springframework.web.context.ContextLoaderListener
#### 5_1) Maven을 업뎃한 후 발생할 수 있는 문제
 - 2_5)의 방법으로 메이븐 업뎃 시 등록된 `Maven Dependencies 라이브러리`가 삭제되면 해당 오류가 발생한다고 함!
 - 프로젝트 우클릭 - properties - Deployment Assembly - Add - java Build Path Entries - MavenDependencies-Apply를 추가

<br>
<hr>

👉 이렇게 길고 긴 하루의 여정으로 디버깅에 성공하였다ㅠㅠㅠ!! 아쉬웠던 점은 처음에 오류가 났을 때 좀 더 빨리 설정파일에 접근했으면 어땟을까 하는 마음? 계속 어노테이션과 mapper 파일만 봤는데 스프링은 다양한 xml 설정파일들에서 모든 것을 각자 관리하면서 전체적으로 이어져있는 느낌? 그래서 오류를 찾기도 매우 어려운 것 같다. 앞으로는 한번 훝어보고 설정파일을 빠르게 확인하는 습관을 들여야겠다!
