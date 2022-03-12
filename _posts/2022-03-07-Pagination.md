---
layout: post
title: "[Java] Pagination"
date: 2022-03-07
excerpt: "게시판 페이징 구현"
categories: Java
tags: [study, Java]
comments: true
---

## 게시판의 페이징바를 구현해보자

게시판 구현 시 수많은 데이터 중 해당 화면에 필요한 데이터만 뽑아서 출력하고 다루기 위해 페이징처리가 필요하다. 
만약 페이징 처리를 하지 않으면 게시글의 갯수가 아주 많을 경우 로딩 시간이 매우 길어지고 보기도 불편할 것이다.
한 페이지에 몇 개의 게시글을 출력할지, 페이징바를 몇 번까지 보여줄지를 정해서 페이징바로 게시글을 정렬하면 깔끔한 게시판을 유지할 수 있다.
이러한 페이징바를 STS에서 Mybatis를 이용해 아래 순서로 구현해보자.

1. Pagination 클래스 생성
2. 총 게시글 개수 확인
3. controller, Service, Dao에 게시글 읽어올 메소드 추가
4. 게시글 목록 조회를 위한 쿼리문 작성
5. 화면단에서 데이터 넘겨받아 출력하기


### 1. Pagination 클래스 생성

1.  페이징처리를 하기 위해 필요한 요소 나열

	- 총 게시글 수 listCount

	- 현재 페이지 = 사용자 요청 페이지 currentPage

	- 페이징바의 페이지 수 단위(페이징바를 몇 번 단위로 보여줄지) pageLimit

	- 페이지당 보여질 게시글 수 boardLimit

	- 가장 마지막 페이지(총 페이지 수) maxPage
		- 게시글이 8개인 게시판에서 게시글을 10개씩 보여주고 싶을 때 <br>
			=> 총 페이지 수는 1페이지
		- 게시글이 15개인 게시판에서 게시글을 10개씩 보여주고 싶을 때 <br>
			=> 총 페이지 수는 2페이지<br>
		- 즉 총 페이지수는 ***총게시글수 / 페이지당 보여질 게시글 수***이다. <br>

		- 하지만 int형으로 계산 시 15/10 = 1이므로 0.5가 날라가 버린다 <br>
		=> ***변수 하나를 실수형으로 형변환*** 후 계산 하면 1.5

		- 여기서 페이징바 숫자는 무조건 정수형이므로 소숫점이 나올 시 무조건 올림처리를 해줌
		=> ***Math.ceil함수 이용***

		- 마지막으로 올림처리 한 수를 정수형으로 형변환
		
		👉 즉, maxPage는 **(int)Math.ceil((double)listCount/boardLimit)**

	- 페이징바의 시작수 startPage
		- pageLimit이 10일 경우 => 1, 11, 21...
		- pageLimit이 5일 경우 => 1, 6, 11, 16...
		=> ***n x pageLimit + 1***

		- pageLimit이 10일 때 currentPage가 1~10일 경우 => 시작수는 1 => n x pageLimit = 0 => n=0
		- pageLimit이 10일 때 currentPage가 11~20일 경우 => 시작수는 2 => n x pageLimit = 1 => n=1
		=> ***n = currentPage - 1 / pageLimit***

		👉 즉, startPage는 **currentPage - 1 / pageLimit x pageLimit + 1**

	- 페이징바의 끝 수endPage
		- pageLimit이 10일 경우 => 10, 20, 30...
		👉 즉, endPage는 **startPabe + pageLimit - 1**

		- 만약 startPage가 11일 때 endPage는 20이다. 하지만 maxPage가 20보다 작다면 maxPage = endPage가 되야함
		👉 즉, **maxPage < endPage 이면 endPage=maxPage**라는 조건을 추가해줌

	즉, 총 게시글이 28개이고 한 화면에 5개 씩, 페이지 수 단위를 5씩 해서 페이징바를 구현할 경우
	=> 페이징바의 시작수 : 1 / 끝 수 : 6 / 총 페이지 수 :  6 
<br>

2. pagination VO 클래스와 Pagenation 클래스 생성
위에서 나열한 6개의 필드를 가지는 VO클래스와 해당 필드값을 계산하는 클래스를 만들자
(여러개의 게시판에서 공통으로 사용가능한 기능이므로 클래스를 따로 만들었다.)
```java
	public class PageInfo {

		private int listCount;		// 현재 총 게시글 갯수
		private int currentPage;	// 현재페이지 = 사용자가 요청한 페이지
		private int pageLimit;		// 하단 페이징바의 페이지 최대 갯수(몇 개 단위)
		private int boardLimit;		// 페이지당 보여질 게시글의 수
		private int maxPage;		// 가장 마지막 페이지(총 페이지 수)
		private int startPage;		// 페이징바의 시작수
		private int endPage;		// 페이징바의 끝수
		
		public PageInfo() {}

		public PageInfo(int listCount, int currentPage, int pageLimit, int boardLimit, int maxPage, int startPage,
				int endPage) {
			super();
			this.listCount = listCount;
			this.currentPage = currentPage;
			this.pageLimit = pageLimit;
			this.boardLimit = boardLimit;
			this.maxPage = maxPage;
			this.startPage = startPage;
			this.endPage = endPage;
		}

		// setter/getter 메소드는 생략~~

	}	
```

```java
	public class Pagination {

	public static PageInfo calPageInfo(int listCount, int currentPage, int pageLimit, int boardLimit) {
		
		int maxPage = (int)Math.ceil((double)listCount/boardLimit);
		int startPage = (currentPage-1) / pageLimit * pageLimit + 1;
		int endPage = startPage + pageLimit - 1;
		if(endPage > maxPage) {
			endPage = maxPage;
		}
		return new PageInfo(listCount, currentPage, pageLimit, boardLimit, maxPage, startPage, endPage);
	}
}
```


### 2. 총 게시글 갯수를 구하기
게시판에 있는 게시글 총 갯수를 COUNT를 이용하여 구함
```xml
	<select id="selectListCount" resultType="_int">
		select count(*)
		  from notice
	</select>
```


### 3. Controller, Service, Dao에 게시글 읽어올 메소드 추가

1. Controller
컨트롤러에서 Pagination 클래스로 listCount, currentPage, boardLimit, pageLimit을 매개변수로 넘겨서 pi정보를 받아온 후 사용할 게시판의 리스트를 받아올 함수에 pi를 넘겨서 현재 페이지에 해당하는 게시글 목록 받아옴
```java
	@RequestMapping("boardList")
	public String boardList(@RequestParam(value="cpage", defaultValue="1") int currentPage, Model model) {
		int listCount = hService.selectListCount();
		PageInfo pi = Pagination.getPageInfo(listCount, currentPage, 5, 5);  
											// boardLimit과 pageLimit을 직접 숫자로 넘겨줌
		ArrayList<Notice> list = hService.selectList(pi);

		model.addAttribute("pi", pi);
		model.addAttribute("list", list);		
		return "boareList";
	}
```

2. Service, ServiceImpl
- Service
```java
	int selectListCount();
	ArrayList<Notice> selectList(PageInfo pi);
```

- ServiceImpl
```java
	@Override
	public int selectListCount() {
		return hDao.selectListCount(sqlSession);
	}

	@Override
	public ArrayList<Notice> selectList(PageInfo pi) {
		return hDao.selectList(sqlSession, pi);
	}
```

3. Dao
- Dao에서 Mybatis의 RowBounds를 이용
- RowBounds는 Mybatis에서 제공하는 API로 offset과 limit를 이용한다.
	- offset : 시작점에서 얼마나 떨어진 데이터인지(몇 개의 게시글을 건너뛰고 조회할건지)
	- limit : 몇 개의 값을 가져올지
	- offset + limit만큼의 데이터를 가져온 뒤 offset만큼 건너뛰고 출력한다. (데이터가 많다면 속도가 느려질 수도 있겠다)
![3](https://user-images.githubusercontent.com/93863500/158008645-4191b310-e34e-49cc-af47-73b2bcce511e.JPG)

- boardLimit이 5일 때
	* currentPage=1 -> 1~5 조회		offset : 0		limit : 5
	* currentPage=2 -> 6~10 조회		offset : 5		limit : 5
	* currentPage=3 -> 11~15 조회		offset : 10		limit : 5

	👉 즉, offset = (pi.getCurrentPage() - 1) * pi.getBoardLimit();
			limit = pi.getBoardLimit();

```java
	public int selectListCount(SqlSessionTemplate sqlSession) {
		return sqlSession.selectOne("helpMapper.selectListCount");
	}

	public ArrayList<Notice> selectList(SqlSessionTemplate sqlSession, PageInfo pi){
		int offset = (pi.getCurrentPage() - 1) * pi.getBoardLimit();
		int limit = pi.getBoardLimit();
		RowBounds rowBounds = new RowBounds(offset, limit);
		
		return (ArrayList)sqlSession.selectList("helpMapper.selectList", null, rowBounds);
	}
```	


### 4. 게시글 목록 조회를 위한 쿼리문 작성

```xml
	<select id="selectList" resultMap="noticeResult">
		select
		       nno
		     , mno
		     , category
		     , n_title
		     , count
		     , to_char(create_date, 'YYYY-MM-DD') as "create_date"
		  from notice
		 order
		    by nno desc
	</select>
```

### 5. 화면단에서 데이터 넘겨받아 출력하기

```jsp
	<div id="paging">
			<ul class="pagination">
				<c:choose>
					<c:when test="${ pi.currentPage eq 1 }">
						<li class="page-item disabled">			// 1페이지 클릭한 경우 << 로 표시되고 클릭 불가하게
							<a class="page-link" href="#" aria-label="Previous"><span aria-hidden="true">&laquo;</span></a>
						</li>
					</c:when>
					<c:otherwise>							// 1페이지 아닐 경우 Previous로 표시되고 현재 페이지 - 1인 페이지 출력
						<li class="page-item"><a class="page-link" href="helpList.he?cpage=${ pi.currentPage-1 }">Previous</a></li>
					</c:otherwise>
				</c:choose>
									// 시작페이지부터 끝페이지에 해당하는 숫자 반복문으로 출력하고 클릭 시 해당 페이지 출력
				<c:forEach var="p" begin="${ pi.startPage }" end="${ pi.endPage }">
					<li class="page-item"><a class="page-link" href="helpList.he?cpage=${ p }">${ p }</a></li>
				</c:forEach>
				
				<c:choose>			// 최대페이지를 클릭한 경우 >>로 표시되고 클릭 불가하게
					<c:when test="${ pi.currentPage eq pi.maxPage }">
						<li class="page-item disabled">
							<a class="page-link" href="#" aria-label="Next"><span aria-hidden="true">&raquo;</span></a>
						</li>
					</c:when>
					<c:otherwise>	// 아닐 경우 Next로 표시되고 현재 페이지 + 1인 페이지 출력
						<li class="page-item"><a class="page-link" href="helpList.he?cpage=${ pi.currentPage+1 }">Next</a></li>
					</c:otherwise>
				</c:choose>
            </ul>
        </div>
```


### 6. 결과 화면

![1](https://user-images.githubusercontent.com/93863500158005919-867d2c34-00f7-4d3d-9db8-d9f5782d60d4.JPG)

![2](https://user-images.githubusercontent.com/93863500/158005959-fcbdf04f-4510-4fa1-a0eb-e97c25e0c310.JPG)


#### 출처

- <a href='https://mybatis.org/mybatis-3/getting-started.html'>Mybatis Java API</a>
- 내 프로젝트