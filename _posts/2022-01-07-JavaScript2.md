---
layout: post
title: "JavaScript2"
date: 2022-01-07
excerpt: "JavaScript의 자료형, 요소, 메소드 등에 대해서 알아보자"
tags: [study, JavaScript]
comments: true
---

<style>
	h3{
		background-color:#D2D2FF;
		color: gray;
	}
</style>

### 1. 변수와 자료형
 
 - 자바스크립트에는 `var`, `let`, `const`로 변수를 선언한다
 - 각 선언 방식 비교
 
	|항목|var|let|const|
	|:--------:|:------:|:------:|:------:|
	|중복선언| O | X | X |
	|재할당| O | O | X |
	|스코프| 선언된 함수 영역 전체 | 블럭{ } 내 | 블럭{ } 내 |

 1. 전역변수와 지역변수
	- 변수 선언 위치에 따라 전역변수와 지역변수로 나뉨
	- 전역변수
		- 특정 함수 영역 밖에 선언된 변수로 어디든 다 사용 가능
		- 전역변수와 지역변수 이름이 같을 때 함수 내에서 전역변수 접근시 `window.` 또는 `this.` 으로 접근
	- 지역변수 
		- 특정 함수 내에 선언된 변수로 함수 내에서는 기본적으로 지역변수 우선
		- 특정 함수 내에 var, let, const 사용하지 않고 선언된 변수는 전역변수로 잡힘<br>
	<br>
 2. 자료형
	- 자바스크립트에서는 변수 선언시 자료형을 별도로 지정하지 않음 
	- 변수에 대입되는 값(리터럴)에 따라서 알아서 자료형이 결정됨
	- 자바스크립트의 자료형
		: String, number, boolean, object(객체), function, null, undefined

        |undefined|null|
        |:--------:|:------:|
        | 타입이 정해지지 않음 | object타입 |
        | 초기화X인 변수나 존재하지 않는 값 접근 시 반환 | 값만 정해지지 않음 |
        | 의미는 같지만 두 값이 일치하지는 않음 ||

### 2. 연산자
 
 - 산술 / 대입 / 증감 / 비교 / 논리 / 비트 등등의 연산자를 제공함

 1. 산술연산자 : +,-,/,*,%
 2. 대입연산자 : =, +=, -=, *=, /=, %=
 3. 증감연산자 : ++x, --x, x++, x--
 4. 비교연산자 : ==, ===, !=, !==, >, >=, <, <=
    > ==과 ===의 차이 
        : ==은 equal, 동등연산자로 타입이 달라도 값이 같으면 true
        : ===은 strict equal, 일치연산자로 타입이 같고 값도 같아야 true
 5. 논리연산자 : &&, \|\|, !
 6. 비트연산자 : &, \|, ^, ~, <<, >>, >>>

 - 연산자의 우선 순위
 
 |우선순위|기능|연산자|
 |:--------:|:------:|:------:|
 |1|괄호|()|
 |2|증감/논리|++ -- !|
 |3|산술연산자| */ % +-|
 |4|비트연산자|<< >> >>>|
 |5|비교연산자|< <= > >=|
 |6|비교연산자|== === != !==|
 |7|비트연산자|& ^ \|' |
 |8|논리연산자| && '\|\|' |
 |9|삼항연산자| ?\: |
 |10|대입연산자|=, +=, -=, *=, /=, %=|

* 더 자세한 연산자 우선순위표는 구글링 요망...^^
<br>

### 3. 제어문

 1. 조건문 
    - 주어진 식의 결과에 따라 각각 명령을 수행하도록 제어하는 실행문
    - if, if/else, if/else if/else, switch

 2. 반복문
    - 프로그램 내에서 일정 횟수만큼 코드를 반복수행하도록 제어하는 실행문
    - while, do/while, for, for/in, for/of
    - for/in
        : 해당 객체에서 열거 가능한 프로퍼티를 순회할 수 있게 하는 제어문
        ```javaScript
            var arr = [1,2,3];
            for (var i in arr){
                document.write(i + " ");
            }
            // 배열의 모든 인덱스를 출력하는 일반적인 for문과 같은 결과 반환
        ```
<br>

### 4. 배열
 
 1. 자바스크립트에서의 배열 특징
    - 타입이 고정되있지 않아 같은 배열의 요소들끼리도 타입이 다를 수 있음
    - 인덱스가 연속적이지 않아도 됨 => 중간에 비어 있을 수 있음
    - Array 객체로 다뤄짐
 
 2. 배열의 생성
    - 리터럴 이용 : `var arr = [1,2,3];`
    - 생성자 이용 : `var arr = Array(1,2,3);`
    - new 연산자 이용 : `var arr = new Array(1,2,3);`
 
 3. 배열 요소 추가
    - push()메소드 이용 : arr.push(추가할요소)  => 배열 마지막에 추가됨
    - length 프로퍼티 이용 : arr[arr.length] = 추가할요소   => 배열 마지막에 추가됨
    - 특정인덱스지정 : arr[인덱스] = 추가할요소

    ```javaScript
        var arr = [1, "coffee", 100]
        arr.push("cake");
        arr[arr.length] = 33;
        arr[7] = "맛있겠다";
        document.write(arr + "<br>");
        document.write(arr[10]);
    ```

### 5. 함수
 
  - 표현식
  ```javaScript
    function 함수명(매개변수){
        실행할 코드;
        [return 리턴값;]
    }
  ```
<br>

### 6. 요소 접근

 1. 아이디 이용 : `document.getElementById("아이디명")`
 ```javascript
	<div id="area1" class="area"></div>
	<button onclick="accessId();">아이디</button>

	<script>
		function accessId(){
			var area1 = document.getElementById("area1");
			area1.innerHTML += "아이디로 접근성공! <br>";   

			area1.style.backgroundColor = "black";
			area1.style.color = "pink";
			area1.style.width = "150px";
			area1.style.height = "150px";

		}
	</script>
 ```
	<img src="https://user-images.githubusercontent.com/93863500/148339443-92b669de-7326-406b-b3ff-c47d44297721.JPG" width="100%" height="100%" align="center">
 <br>

 2. 태그명 이용 : `document.getElementsByTagName("태그명")` => 선택된 요소 객체들 배열로 반환
 ```javascript
	<ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>

    <button onclick="accessTagName();">태그명으로 접근</button>

    <script>
        function accessTagName(){

            var list = document.getElementsByTagName("li");    
            var blueColor = 50;
            for(var i=0; i<list.length; i++){
                list[i].innerHTML = "하이욤";
                list[i].style.backgroundColor="rgb(130, 220, " + blueColor + ")";
                blueColor += 100;
            }

        }
    </script>
 ```
	<img src="http://user-images.githubusercontent.com/93863500/148339470-8d5cc971-eeaa-43f2-8f3f-8b25b5ffd6e2.JPG" width="50%" height="100%">
	<img src="https://user-images.githubusercontent.com/93863500/148339512-1bfc826f-23ac-45c8-812c-171b44c48efb.JPG" width="50%" height="100%">
 <br>

 3. name 속성값을 이용 : `document.getElementsByName("name속성값")` => 선택된 요소 객체들 배열로 반환
 ```javascript
	<form action="">
        <fieldset>
            <legend>메뉴</legend>

            <input type="checkbox" name="food" value="뜨끈한 추어탕" id="chu">
            <label for="game">추어탕</label>

            <input type="checkbox" name="food" value="매콤한 순대볶음" id="soon">
            <label for="reading">순대볶음</label>

            <input type="checkbox" name="food" value="배터지는 샤브샤브" id="sha">
            <label for="sport">샤브샤브</label>

        </fieldset>
    </form>
    <br>
    <div class="area" id="area3"></div>
    <button onclick="accessName();">name으로 접근</button>

    <script>
        function accessName(){
            var food = document.getElementsByName("food");        

            console.log(food);              
            var area3 = document.getElementById("area3");

            for(var i=0; i<food.length; i++){
                console.log(food[i].checked); 
                if(food[i].checked){           
                    console.log(food[i].value);
                    area3.innerHTML += food[i].value + "<br>";
                }
            }
        }
    </script>  
 ```
	<img src="https://user-images.githubusercontent.com/93863500/148340075-76f5a31c-436d-46a4-93cc-0c1d38363dc8.JPG" width="100%" height="100%" align="center">
	<img src="https://user-images.githubusercontent.com/93863500/148340112-e15175a1-388f-4e77-8bc4-792a42f03629.JPG" width="100%" height="100%" align="center">
 <br>

 4. 클래스 이용 : `document.getElementsByClassName("class속성값")` => 선택된 요소 객체들 배열로 반환
 <br>

 5. 선택자 활용 : `document.querySelector("선택자") | document.querySelectorAll("선택자")`
<br>



<h4>출처</h4>
  - 수업자료
  - https://www.w3schools.com/