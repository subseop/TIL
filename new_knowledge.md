## JOIN의 종류
- inner join: 두 테이블에서 일치하는 행만 가져옴
- left join: 왼쪽 테이블은 모두 + 오른쪽 테이블은 매칭되는 것만
- right join: 오른쪽 테이블은 모두 + 왼쪽 테이블은 매칭되는 것만
- full join: 둘 중 하나라도 있으면 가져옴

## Sales 로직
각 상품마다
    각 월마다
        매출을 INSERT해라

## 테이블 별칭(alias)
```
FROM   merchant_product mp
        JOIN   product p
```
mp = merchant_product
p = product
즉, 테이블 이름을 줄여 쓰기 위한 닉네임

## record란
테이블의 한 줄(row) 전체 데이터를 담은 묶음

    ### 변수 vs record 변수
    - 일반 변수: 한 가지 값만 저장
    - record 변수: 여러 개의 컬럼을 묶어서 한 번에 저장
    ```
    rec = {
    merchant_product_id : 1,
    store_id            : 2,
    product_id          : 3,
    standard_category   : 'Fruit'
    }
    ``` 
    이런식으로 생김


## CASE 문법
- 표현식 CASE: 특정 값에 대해 조건 분기가 필요한 경우
```
CASE mp.product_id
    WHEN 1 THEN 'Banana'
    WHEN 2 THEN 'Milk'
    ELSE '기타'
END
```
- 검색식 CASE: 값 비교가 아니라 조건식 자체로 분기할 때 사용용
```
CASE
    WHEN tm.month BETWEEN 12 AND 2 THEN '겨울'
    WHEN tm.month BETWEEN 3 AND 5 THEN '봄'
    ELSE '기타'
END
```

- PL/SQL에서 쓰는 형태(지금 SLAES 코드)
```
CASE mp.product_id
    WHEN 1 THEN 
        v_base_price := 4500;
        v_base_qty   := 300;
    WHEN 2 THEN
        v_base_price := 2800;
        v_base_qty   := 450;
    ELSE
        v_base_price := 5000;
        v_base_qty   := 100;
END CASE;
```

## DBMS_RANDOM.VALUE 랜덤함수
DBMS_RANDOM.VALUE(low, high)
low 이상, high 이하 범위에서 실수(float) 랜덤값 생성

## ROUND()
숫자 반올림하는 함수

## 전역변수(Global Variable)
어디서든 사용할 수 있는 공용 변수

## 지역변수(Local Variable)
특정 범위 내에서만 쓸 수 있는 변수
```
.card {
  --local-color: red;
  color: var(--local-color);
}
```
이 변수는 .card안에서만 적용됨 

## body 구조
```
<body>
  <header>상단 영역</header>
  <main>주요 콘텐츠</main>
  <footer>하단 영역</footer>
</body>
```

## JSON
JavaScript Object Notation: 자바스크립트 객체처럼 생긴 데이터 표현 방식
```
{
  "name": "banana",
  "price": 1500,
  "qty": 3
}
```
- {} : 개체(오브젝트)
- "키": 값 형태
- 문자열은 반드시 "쌍따움표" 사용

## JS 변수 선언 
let : 변수 선언(재할당 가능)
const : 상수 선언(재할당 불가능)
```
let SALES = 
```

## 비동기함수(ASynchronous)
기다리지 않고 다음 코드로 넘어가는 함수

- 동기함수(Synchronous)
A 작업 끝날 때까지 기다림 → 끝나야 B 작업 실행됨

- 비동기함수(ASynchronous)
A 작업을 “백그라운드”로 보냄 → 바로 다음 코드(B)를 실행함 → A가 끝나면 그때 결과 전달
 - function 앞에 async 를 붙임

## document
현재 열려있는 웹페이지 전체를 나타내는 객체 / 브라우저가 HTML 파일을 읽으면, 그걸 문서(Document) 형태로 메모리에 올림 → 이게 document
- document.getElementById('storeFilter')에서 document는 페이지 전체를 나타내는 객체 / getElementById는 id요소 찾기 기능 내장 함수 / 'storeFilter'는 찾고자 하는 id

## API
Application Programming Interface
- 다른 프로그램에게 '어떻게 요청하고, 어떻게 응답받는지' 정해놓은 규칙'
- 프론트엔드(JS) -> 서버(PHP)에 요청 / 서버 -> 결과를 돌려줌(JSON)  : 이때 어떤 주소로, 어떤 형식으로 무엇을 보내야 하는지 정해놓은 메뉴판이 바로 API
- API를 호출한다 = 그 메뉴판대로 서버에 요청을 보낸다
`fetch("sales_api.php?action=mart_recommendation&store=Hansol")` = "서버야, mart_recommendation 기능을 실행해서 결과를 나한테 보내줘"
-> 그럼 브라우저는 서버에 HTTP요청(GET/POST)을 보내고, 서버는 JSON으로 응답해 줘

- 프론트엔드(JS)가 직접 DB를 읽을 수는 없음. 따라서 다음과 같은 순서로 돌아감
[프론트엔드]  fetch() 호출
      ↓
[API(PHP)] 요청 받음
      ↓
[DB] 데이터 조회
      ↓
[PHP] JSON으로 변환
      ↓
[프론트] 결과 표시
