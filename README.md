# Obsidian-dataview-template-table
This is dataviewjs code for Obsidian's data view.

![image](https://github.com/kim365my/Obsidian-dataview-template-table/assets/102598905/3b7efba9-f19e-4f97-bb6a-7dea1b2ea5f9)

css class를 추가해서 마음에 드는대로 조절할 수 있습니다.  
![image](https://github.com/kim365my/Obsidian-dataviewjs-template-table/assets/102598905/9aac6d7f-2400-438f-807f-beda862f225d)
![image](https://github.com/kim365my/Obsidian-dataviewjs-template-table/assets/102598905/272f79ef-c1c4-4d3e-9dc2-38925c94ef8b)


## 사용 전 필요한 플러그인

- 필수 : advanced-uri (이미지 클릭시 해당 md 파일로 넘어가게 해줌)
- 선택 (없이도 작동됨) : templater / customjs

## 작성형식

```dataviewjs
let input = {
	"pages": '#독서',
	"row" : 'author, finish_read_date, tags',
	"imgLocal" : 'false',
	"selectedValue": 20,
	"filter" : [
		{
			"label" : "📕 완독서",
			"class": "진행여부",
			"type": "property",
			"target": "status",
			"target_content": "true",
		},
		{
			"label" : "📖 읽고 있는 책",
			"class": "진행여부",
			"type": "property",
			"target": "status",
			"target_content": "false"
		},
		{
			"label" : "eBook만",
			"class": "도서분류",
			"type": "property",
			"target": "category",
			"target_content": "eBook"
		},
		{
			"label" : "영어공부만",
			"class": "도서분류",
			"type": "tags",
			"target": "영어",
			"target_isInclude": "true"
		},
		{
			"label" : "프로그래밍만",
			"class": "도서분류",
			"type": "tags",
			"target": "프로그래밍",
			"target_isInclude": "true"
		},
	],
	"filterDefault": ["📖 읽고 있는 책"],
	"sort" : [
		{
			"label": "완독일순 (최신순)",
			"type": "finish_read_date",
			"sort": "desc"
		},
		{
			"label": "완독일순 (오래된순)",
			"type": "finish_read_date",
			"sort": "asc"
		}
	],
	"sortDefault": 1,
	"option": ["useFilterModal", "useSearchBtn"]
}
await dv.view("etc/module/views/pageDataRenderer", input) 
```

## 필수 변수

- pages
- row

## 선택적 변수

- header : (string) 제목추가, multi 콜아웃에서 해당 코드를 사용할 경우 유용
- imgLocal : (true/ false)
- selectedValue : (number) 표시할 페이지 수를 작성
- sortDefault : (number 0 / 1) : 최신순 / 오래된순으로 기본적으로 정렬할 것인지 선택
- filter
	- 배열형식으로 작성
	- 변수 목록
		- label : 표시될 값을 작성
		- type : tags(프로퍼티 내의 tags만 검사), property, file.tags (파일내 존재하는 tags를 모두 검사)
		- target : 태그나 속성명을 작성
		- target_isInclude : (true/ false) tags나 property가 해당 md 문서에 존재하는지 판단
		- target_content : property만 사용) property에 해당 내용이 존재하는지 체크
		- class : (선택적) 필터 분류
- filterDefault : filter 중 기본적으로 사용하고 싶은 필터가 있을 경우 해당 필터의 label을 작성해서 사용
- sort : sort를 추가할 수 있음
- sortDefault : sort 중에서 기본적으로 사용하고 싶은 필터가 있을 경우 해당 번호 작성 (0부터 시작하므로 주의)
- option
	- useFilterModal : 필터를 모달로 사용해서 받음
	- useSearchBtn : 검색창을 버튼으로 교체

## 변경기록

- 24/02/04 : 초기버전
- 24/02/19 : imgLocal 버그 수정
- 24/03/24
	- header, option, sortDefault 추가
	- filter 개선
		- 중첩필터 가능
		- 필터된 아이템을 한눈에 볼수 있도록
	- csv 지원 (모든 기능 지원)
		- 엑셀과 호환되므로 엑셀을 사용하는 것도 좋음 (주의 : csv UTF-8로 저장할 것)
		- csv 작성규칙
			- 내부 이미지를 불러오고 싶은 경우
				- csv를 통해 이미지를 표시하고 싶은 경우 ![[파일이름]] 형식으로 작성
				- 이미지를 링크로 사용하고 싶은 경우 [![[파일이름]]](링크) 형식으로 작성
			- 외부 이미지는 <img src="이미지 경로"> 형식으로 작성
			- 이미지 파일을 두개이상 추가할 경우 "" 따음표로 묶고 ,으로 구분할것
			- 필터는 metadata라는 col를 추가해야 작동
		- 해당 dataviewjs 파일을 통해 csv를 불러올 경우 작성형식
			- "pages": "csv/파일/경로.csv"로 작성 할 것
			- metadata col을 통해서 배열 값을 불러오므로 "filter"를 작성할 필요 없음
		- 모바일에서는 csv 데이터 추가기능이 작동하지 않음
	- cssClass 추가
		- cards-wide: 카드 너비를 340px로 넓힘
- 24/03/27 : 필터 분류기능 추가, 새로운 필터 변수 class를 통해 필터를 분류가능
- 24/04/02 : 깃허브 업로드
- 24/04/13
	- 기본적으로 file.link와 cover_url을 추가하던 것을 폐기. 이제부터는 row에 작성해야 표시
	- HasImgLocal 변수 제거
	- filter 기능에 file.name, file.aliases, file.inlinks, file.outlinks 변수 지원 추가
	- 이제부터 검색은 file.name 뿐만 아니라 file.aliases도 검사
- 24/04/15
	- header 변수 제거
	- 이제부터 cover_url에 일일히 대표 이미지를 지정하지 않아도 자동으로 문서 내부의 이미지를 불러와 줌 (아쉽게도 로컬 이미지만 가능)
