# 데이터 분석을 위한 pandas library

## 데이터란?
- 데이터(Data): 프로그램을 운용할 수 있는 형태로 기호화·숫자화한 자료
- 판다스(pandas): python으로 작성된 데이터를 조작 및 분석하기 위한 소프트웨어 라이브러리/    
특히 숫자 테이블과 시계열, 행렬을 분석하기 위한 데이터 구조와 연산을 제공함
- 데이터 프레임(DataFrame): 데이터를 행과 열로 구성한 2차원 데이터/  
길이가 깉은 벡터를 열로 구성한 리스트로 이루어진 구조
- 시리즈(Series): 한 개의 컬럼으로 구성된 데이터 프레임/  
1차원 배열과 같은 자료구조로 인덱스가 같이 출력됨

## 데이터 불러오기
- import pandas as pd
- pd.read_csv('파일경로/파일명.csv') 
    - csv 파일을 데이터프레임으로 읽음
```python
import pandas as pd
df = pd.read_excel("document_sample.xlsx")
df.to_csv("document_sample.csv", index=False) //csv파일이 없어서 csv파일로 변환함
csv = pd.read_csv("document_sample.csv")
print(csv)
----------------------------------------------
    A  B    C
0  a1  b1  c1
1  a2  b2  c2
2  a3  b3  c3
``` 
- pd.read_excel('파일경로/파일명.xlsx')
    - xlsx 파일을 데이터프레임으로 읽음
    - 파일 경로에 "\\"주의
```python
import pandas as pd
xls = pd.read_excel("document_sample.xlsx")
print(xls)
----------------------------------------------
    A  B    C
0  a1  b1  c1
1  a2  b2  c2
2  a3  b3  c3
```

## 데이터 만들기
- pd.Series()
    - Array 또는 List를 활용하여 생성
    - indx = []을 통해 원하는 index 지정 가능
```python
s = pd.Series([1,3,5,7,9])
print(s)
------------------------------------
0    1
1    3
2    5
3    7
4    9
dtype: int64
```
```python
s = pd.Series([1,3,5,7,9], index=["a", "b", "c", "d", "e"])
print(s)
---------------------------------------------------
a    1
b    3
c    5
d    7
e    9
dtype: int64
```

- pd.DataFrame()
    - 데이터를 Array, List, Dictionary 형태로 생성하여 함수에 적용
    - index = []을 통해 index 지정 가능
    - columns = []을 통해 column 지정 가능
```python
data = [[2019, "kim", 1],
        [2020, "Lee", 3],
        [2021, "Park", 5],
        [2022, "Choi", 7]]
df = pd.DataFrame(data)
print(df)
------------------------------
      0     1  2
0  2019   kim  1
1  2020   Lee  3
2  2021  Park  5
3  2022  Choi  7
```
```python
df = pd.DataFrame(data, index=["one", "two", "trhee", "four"], columns = ["year", "name", "number"])
print(df)
----------------------------------------
       year  name  number
one    2019   kim       1
two    2020   Lee       3
trhee  2021  Park       5
four   2022  Choi       7
```

## 데이터 연결하기
- pd..concat([])
    - 합칠 DataFrmae들을 넣음
    - 기본적으로 위 -> 아래 방향으로 데이터 연결
```python
df1 = pd.DataFrame({
    'A': ['A1', 'A2', 'A3'],
    'B': ['B1', 'B2', 'B3']
})

df2 = pd.DataFrame({
    'A': ['A4', 'A5', 'A6'],
    'B': ['B4', 'B5', 'B6']
})

df = pd.concat([df1, df2])
print(df)
--------------------------------
    A   B
0  A1  B1
1  A2  B2
2  A3  B3
0  A4  B4
1  A5  B5
2  A6  B6
```

+ [[]]: 이런식으로 리스트 안에 리스트 형태면 행 중심. 즉, 가로로 나열됨  
{[]}: 딕셔너리로 되어있으면 열 중심(key는 열, value는 그 열에 들어갈 행 데이터). 즉, 세로로 나열됨

- axis=1 : 왼 -> 오 방향으로 데이터 연결
```python
df = pd.concat([df1, df2], axis=1)
print(df)
-------------------------------------
    A   B   A   B
0  A1  B1  A4  B4
1  A2  B2  A5  B5
2  A3  B3  A6  B6
```

- join = 'inner': 교집합으로 데이터 연결
- df1은 A, B 열, df3는 A, C 열
```python
df3 = pd.DataFrame({
    'A': ['A1', 'A2', 'A3'],
    'C': ['C1', 'C2', 'C3']
})

df1 = pd.DataFrame({
    'A': ['A1', 'A2', 'A3'],
    'B': ['B1', 'B2', 'B3']
})

df = pd.concat([df1, df3], join='inner')
print(df)
------------------------------------------
    A
0  A1
1  A2
2  A3
0  A1
1  A2
2  A3
```

- join = 'outer': 합집합으로 데이터 연결, NaN값이 생김
```python
df = pd.concat([df1, df3], join='outer')
print(df)
----------------------------------------
    A    B    C
0  A1   B1  NaN
1  A2   B2  NaN
2  A3   B3  NaN
0  A1  NaN   C1
1  A2  NaN   C2
2  A3  NaN   C3
```

- ignore_index=True : index 초기화
```python
df = pd.concat([df1, df2], ignore_index=True)
print(df)
-------------------------------------------------
    A   B
0  A1  B1
1  A2  B2
2  A3  B3
3  A4  B4
4  A5  B5
5  A6  B6
```

- pd.merge(Dataframe1, Dataframe2)
    - 기본적으로 두 데이터프레임에서 공통되는 것을 기준으로 그 행만 데이터 연결
```python
df4 = pd.DataFrame({
    'A': ['A1', 'A2', 'A3', 'A4'],
    'B': ['B1', 'B2', 'B3', 'B4']
})

df5 = pd.DataFrame({
    'A': ['A1', 'A2', 'A3', 'A5'],
    'C': ['C1', 'C2', 'C3', 'C5']
})

df = print(pd.merge(df4, df5))
print(df)
---------------------------------
   A   B   C
0  A1  B1  C1
1  A2  B2  C2
2  A3  B3  C3
None
```

- on = 'A' : A를 기준 데이터프레임으로 연결
- how = 'outer' : 어느 한 쪽이라도 값이 없으면 NaN값을 나타냄
```python
df = pd.merge(df4, df5, how='outer', on='A')
print(df)
----------------------------------------------
    A    B    C
0  A1   B1   C1
1  A2   B2   C2
2  A3   B3   C3
3  A4   B4  NaN
4  A5  NaN   C5
```

- pd.merge(Dataframe1, Dataframe2)
    - how = 'left' : 두 데이터프레임 중 왼쪽값을 기준으로 데이터 연결
    - 'right'로 설정하면 오른쪽 값을 기준으로 데이터 연결
```python
df = pd.merge(df4, df5, how='left')
print(df)
----------------------------------
    A   B    C
0  A1  B1   C1
1  A2  B2   C2
2  A3  B3   C3
3  A4  B4  NaN
```

- 열단위 추출 - 데이터 프레임[column 이름]
    - column을 하나만 지정하면 series 형태
    - 여러 개 지정하면 dataframe 형태로 추출
    + Series는 1D array, 열 개념 없음  
    Dataframe은 2D array, 행과열, Series들의 집합
```python
data = pd.DataFrame(
    {
        'A': ['A1', 'A2', 'A3'],
        'B': ['B1', 'B2', 'B3'],
        'C': ['C1', 'C2', 'C3']
    },
    index=['one', 'two', 'three']
)
print(data)
-------------------------------------------
        A   B   C
one    A1  B1  C1
two    A2  B2  C2
three  A3  B3  C3
```
```python
a = data['A']
print(a)
--------------
one      A1
two      A2
three    A3
Name: A, dtype: object
```
```python
print(data[['A', 'C']])
---------------------------
        A   C
one    A1  C1
two    A2  C2
three  A3  C3
```

- 행 단위 추출 1: loc[index]
    - index를 기준으로 행 데이터 추출
    - 문자열 데이터 입력
    - index를 기준으로 하기 때문에 음수를 넣으면 keyerror 발생
```python
data.loc['one']
------------------
one
A	A1
B	B1
C	C1

dtype: object
```
- 행 단위 추출 2: iloc[행 번호]
    - 행 번호를 기준으로 행 데이터 추출
    - 행 번호를 정수형으로 입력
    - 음수를 넣으면 마지막 행부터 추출
```python
data.iloc[0]
-------------

one
A	A1
B	B1
C	C1

dtype: object
```

- loc[index, column이름], iloc[행 번호, 열 번호]
    - 다음과 같은 형태로 값을 지정해서 추출 가능

- index, row, column 자리에 가능한 것
    - 리스트
    - 슬라이싱 - 시작:끝
    - range(시작, 끝, 간격)
    - 조건
```python
l = [0,2]
data.iloc[l,0]
---------------

A
one	A1
three	A3

dtype: object
```
```python
data.iloc[:, 0]
-------------------

A
one	A1
two	A2
three	A3

dtype: object
```
```python
data.iloc[range(0, 3, 2), 0]
------------------------------

A
one	A1
three	A3

dtype: object
```

## 데이터 다루기
- head()
- tail()
- shape: 행과 열의 크기 반환
- columns: 열 정보 반환
- dtypes: 자료형 반환
- info(): 자세한 정보들 반환
- drop(row/column 이름, axis): 행이나 열 삭제
- count(axis): 행이나 열의 개수 반환
- sample(frac=1): 행 기준 무작위로 데이터 섞음
- sort_index: 오름차순 정렬
- sort_value(column 이름): 오름차순 정렬

## 데이터 처리하기
- 최대, 최소값 함수
    - max(), min()
    - axis=0 / 행 기준  
### (중요!) axis=0 은 0번축(행)을 없앤다 -> 열만 남는다 -> 열별 계산

- 합계, 평균 함수
    - sum(), mean()

- groupby()
    - 지정한 값을 기준으로 통계 또는 집계 결과를 그룹화
```python
g_data = pd.DataFrame({
    'city': ['부산', '부산', '부산', '부산', '서울', '서울', '서울'],
    'fruits': ['apple', 'orange', 'banana', 'banana', 'apple', 'apple', 'banana'],
    'price': [100, 200, 250, 300, 150, 200, 400],
    'quantity': [1, 2, 3, 4, 5, 6, 7]
})
print(g_data)
g_data.groupby('city').sum()
----------------------------------------------------------------------
  city  fruits  price  quantity
0   부산   apple    100         1
1   부산  orange    200         2
2   부산  banana    250         3
3   부산  banana    300         4
4   서울   apple    150         5
5   서울   apple    200         6
6   서울  banana    400         7

city			
부산	appleorangebananabanana	850	10
서울	appleapplebanana	750	18
```

## 데이터 저장하기
- data.to_csv("파일명.csv")
- data.to_xlsx("파일명.xslx")

## 데이터 시각화하기
- plot()
    - kind를 통해 plot의 종류 지정 가능
    - bar = 막대그래프
```python
df.plot(kind=:"bar")
```
    - bash = 가로 막대 그래프
    - stacked = 누적
    - hist = 히스토그램, column별로 그래프가 그려짐
```python
df.plot(kind="bash", stacked=True)
```
```python
df.hist()
```
    - box = 박스 그래프
    - scatter = column간의 상관관계를 시각화한 그래프
```python
df.plot(kind="box")
```
```python
plt.scatter(df['부상(명)'], df['사고(건)'])
```
- matploblib
```python
import matplotlib.pyplot as plt
plt.figure(figsize=(15, 2))            // 그래프 사이즈 지정
plt.bar(df['구분'], df['사고(건)'])     // x축=구분, y축 사고 인 막대그래프(bar니까)
plt.title("2014년 월별 사고(건)수")     // 그래프 제목 지정
plt.xlabel("날짜")                    // x축 제목 지정
plt.ylabel("사고(건)")                // y축 제목 지정
plt.show()                          // 그래프 출력
```

## 누락값
- NaN이라고 표현하며 값이 없음
    - ""(공백), 0, False 모두와 같지 않고 비교 불가능
    - 심지어 다른 NaN값과도 비교 불가능
- NaN값의 식별
    - isnull() = True면 NaN값
    - notnull() = True면 NaN값이 아님

## 누락값 - 처리
- nan값 파악
    - (전체 데이터의 개수-nan값이 아닌 데이터의 개수) = nan개수
```python
df = pd.DataFrame({
    'A': [15, np.nan, 68, 7, 23, 0, 97],
    'B': [4, np.nan, 5, 15, 31, np.nan, np.nan],
    'C': [0, 42, 3, np.nan, np.nan, 71, 18],
    'D': [np.nan, 81, 0, 56, np.nan, 7, 44]
})

print(df)
--------------------------------------------------
      A     B     C     D
0  15.0   4.0   0.0   NaN
1   NaN   NaN  42.0  81.0
2  68.0   5.0   3.0   0.0
3   7.0  15.0   NaN  56.0
4  23.0  31.0   NaN   NaN
5   0.0   NaN  71.0   7.0
6  97.0   NaN  18.0  44.0
```
```python
n = df.shape[0] - df.count() // df.shape[0]는 행 개수, df.count()는 nan은 세지 않음
print(n) // 자동으로 브로드캐스팅해서 계산
----------------------------

0
A	6
B	4
C	5
D	5
```

- 아무것도 하지 않음
    - 모델에 따라 nan값을 처리하지 않으면 적용할 수 없는 모델이 있음
    - 계산 결과에 문제 발생할 수 있음

- nan값이 있는 행 제거: dropna()
    - 데이터의 개수가 과도하게 줄어들 수 있음
    - 데이터가 편향될 수 있음
    - 중요한 정보를 가진 데이터를 잃을 수 있음
```python
df_d = df.dropna()
print(df_d)
--------------------
      A    B    C    D
2  68.0  5.0  3.0  0.0
```

- 0또는 지정 상수로 대체 처리: fillna()
    - 0또는 상수를 지정하여 괄호 안에 넣어서 nan값을 그 값으로 대체
```python
df_f0 = df.fillna(0)
print(df_f0)
------------------------
      A     B     C     D
0  15.0   4.0   0.0   0.0
1   0.0   0.0  42.0  81.0
2  68.0   5.0   3.0   0.0
3   7.0  15.0   0.0  56.0
4  23.0  31.0   0.0   0.0
5   0.0   0.0  71.0   7.0
6  97.0   0.0  18.0  44.0
```
- fillna(method='ffill') = 바로 전 값으로 처리 // 행렬에서 바로 위에 있는 값
- fillna(method='bfill') = 바로 이후 값으로 처리 // 행렬에서 바로 아래 있는 값
- 보간값 처리
    - df.interpolate() = 데이터프레임이 일정한 간격을 유지하는것처럼 수정  
    // 행렬에서 위와 아래 값의 평균을 nan에 입력

- 앞의 방법들은 nan값들을 완전히 없애지 못함
- 평균 또는 중앙값 처리
    - df.fillna(df.mean()) // 각 열의 평균값을 nan에 입력
    - df.fillna(df.median()) // 각 열의 중앙값을 nan에 입력
    - mean() 과 median()은 기본적으로 axis=0

- skipna
    - True로 설정할 경우 nan값을 무시하고 계산
    - False일 경우 nan값이 포함된 채로 계산되어 결과도 nan값
```python
df.B.sum(skipna=True)
----------------------
55.0
```
```python
df.B.sum(skipna=False)
-----------------------
nan
```

# 피벗 
- 많은 양의 데이터에서 필요한 자료만을 뽑아 새롭게 표로 작성해주는 기능

- melt()
    - 지정한 ID 변수를 기준으로 나머지 column의 이름과 column의 값들을 아래로 쭉 나열하여 데이터 재구조화
    - id_vars=위치를 유지할 column지정
    - value_vars=값에 들어갈 column 지정
    - var_name = 'var' column 이름 지정
    - value_name = 'value' column 이름 지정
```python
df = pd.DataFrame({
    'store': [
        'Costco', 'Costco', 'Costco',
        'Wal-Mart', 'Wal-Mart', 'Wal-Mart',
        "Sam's Club", "Sam's Club", "Sam's Club"
    ],
    'product': [
        'Potato', 'Onion', 'Cucumber',
        'Potato', 'Onion', 'Cucumber',
        'Potato', 'Onion', 'Cucumber'
    ],
    'price': [3000, 1600, 2600, 3200, 1200, 2100, 2000, 2300, 3000],
    'quantity': [25, 31, 57, 32, 36, 21, 46, 25, 9]
})

print(df)
--------------------------------------------------------------------------
        store   product  price  quantity
0      Costco    Potato   3000        25
1      Costco     Onion   1600        31
2      Costco  Cucumber   2600        57
3    Wal-Mart    Potato   3200        32
4    Wal-Mart     Onion   1200        36
5    Wal-Mart  Cucumber   2100        21
6  Sam's Club    Potato   2000        46
7  Sam's Club     Onion   2300        25
8  Sam's Club  Cucumber   3000         9
```
```python
df_m1 = df.melt(id_vars=['product', 'store']) 
print(df_m1)
--------------------------------------------------
     product       store  variable  value
0     Potato      Costco     price   3000
1      Onion      Costco     price   1600
2   Cucumber      Costco     price   2600
3     Potato    Wal-Mart     price   3200
4      Onion    Wal-Mart     price   1200
5   Cucumber    Wal-Mart     price   2100
6     Potato  Sam's Club     price   2000
7      Onion  Sam's Club     price   2300
8   Cucumber  Sam's Club     price   3000
9     Potato      Costco  quantity     25
10     Onion      Costco  quantity     31
11  Cucumber      Costco  quantity     57
12    Potato    Wal-Mart  quantity     32
13     Onion    Wal-Mart  quantity     36
14  Cucumber    Wal-Mart  quantity     21
15    Potato  Sam's Club  quantity     46
16     Onion  Sam's Club  quantity     25
17  Cucumber  Sam's Club  quantity      9
```
```python
df_m2 = df.melt(id_vars=['product', 'store'], value_vars='quantity', 
                var_name='product_info', value_name='product_value')
print(df_m2)
----------------------------------------------------------------------
    product       store product_info  product_value
0    Potato      Costco     quantity             25
1     Onion      Costco     quantity             31
2  Cucumber      Costco     quantity             57
3    Potato    Wal-Mart     quantity             32
4     Onion    Wal-Mart     quantity             36
5  Cucumber    Wal-Mart     quantity             21
6    Potato  Sam's Club     quantity             46
7     Onion  Sam's Club     quantity             25
8  Cucumber  Sam's Club     quantity              9
```

- pivot()
    - 인덱스와 행,열을 지정해서 데이터 재구조화
    - 중복된 데이터가 있으면 error 발생
    - index=행 위치에 들어갈 값
    - columns=열 위치에 들어갈 값
    - values=데이터로 사용할 값
```python
df_p = df.pivot(index='product', columns='store', values='price')
print(df_p)
------------------------------------------------------------------
store     Costco  Sam's Club  Wal-Mart
product                               
Cucumber    2600        3000      2100
Onion       1600        2300      1200
Potato      3000        2000      3200
```

- pivot_table()
    - 인덱스와 행, 열, 함수를지정해서 데이터 재구조화
    - aggfunc = 데이터 집계 함수
    - 중복된 데이터를 집계 함수를 통해 처리
```python
df = pd.pivot_table(df, index='grade', columns='subject',
                        values='score', aggfunc='sum')
// 행은 grade, 열을 subject, 값은 score를 각각 합한 값
```

## 중복데이터
- duplicatied(): 중복 요소 확인
- drop_duplicates(): 중복 요소 삭제

## 대용량 데이터
- glob.glob()을 통해 파일의 리스트 추출

## 형 변환
- dtypes: dataframe type 확인
- astype(): astype({'column':'type'})을 통해 특정 column의 타입만 변경할 수 있음
- pd.to_numeric(): 문자열 데이터를 숫자형으로 변환해주는 method
- to_numeric()의 errors 인자
- category 자료형