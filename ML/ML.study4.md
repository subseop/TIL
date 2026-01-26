# 실전 데이터 처리 실습

## 서울 도봉구 미세먼지(PM10) 예측
- AirKorea 데이터
    - 전국의 데이터가 월별, 년도별로 나누어져 저장
    - 2020년 1~12월 데이터가 필요하기 때문에 총 12개의 xlsx 파일 사용
    - 서울 도봉구의 SO2, CO, O3, NO2, PM10 데이터만 추출

- KMA 데이터
    - 특정 지역의 데이터가 년도별로 나누어져 저장
    - 2020년 서울의 데이터가 필요하기 때문에 1개의 csv파일 사용
    - 기온, 강수량, 풍속, 습도, 이슬점 온도 데이터만 추출

- 모델에 적용하기 위한 전처리
    - 현재 시간을 포함한 이전 2시간 데이터, 총 3시간 데이터로 1시간 후 미세먼지(PM10) 값을 예측

### AirKorea 파일 불러오기
```python
air = glob.glob('./2020_airkorea/2020y*')  
// glob.glob는 특정 패턴에 맞는 파일 경로 목록 찾는 함수, 2020_airkorea에 있는 2020y로  시작하는 모든 파일을 리스트(list)로 가져와라
air.sort() // 리스트를 "문자열 기준" 오름차순으로 정렬
air
```

### Dataframe 대입
- globals() 함수를 이용해 변수를 자동으로 생성 
    - golbals() = 현재 파일에 전역 변수들을 dict(딕셔너리) 형태로 반환하는 함수
- {} 안에 format의 형식으로 range(1, 13), 즉 1~12가 대입되어 seoul_air_1 ~ seoul_air_12 변수들이 자동 생성
```python
for i in range(1, 13):
  globals()['seoul_air_{}'.format(i)] = pd.read_excel(air[i-1])
```
```python
seoul_air_1  = pd.read_excel(air[0])
seoul_air_2  = pd.read_excel(air[1])
seoul_air_3  = pd.read_excel(air[2])
seoul_air_4  = pd.read_excel(air[3])
seoul_air_5  = pd.read_excel(air[4])
seoul_air_6  = pd.read_excel(air[5])                            
seoul_air_7  = pd.read_excel(air[6])
seoul_air_8  = pd.read_excel(air[7])
seoul_air_9  = pd.read_excel(air[8])
seoul_air_10 = pd.read_excel(air[9])
seoul_air_11 = pd.read_excel(air[10])
seoul_air_12 = pd.read_excel(air[11])
```
위 두 코드는 완전히 같은 코드이다

### seoul_air_1 Dataframe 확인
- 아래는 전국의 1월 AirKorea 데이터이다
```python
seoul_air_1
----------------------------------------------------------------------------------------
	지역	망	측정소코드	측정소명	측정일시	SO2	CO	O3	NO2	PM10	PM25	주소
0	서울 중구	도시대기	111121	중구	2020010101	0.003	0.5	0.002	0.036	24.0	19.0	서울 중구 덕수궁길 15
1	서울 중구	도시대기	111121	중구	2020010102	0.003	0.6	0.001	0.039	25.0	21.0	서울 중구 덕수궁길 15
2	서울 중구	도시대기	111121	중구	2020010103	0.003	0.9	0.001	0.037	29.0	23.0	서울 중구 덕수궁길 15
3	서울 중구	도시대기	111121	중구	2020010104	0.002	0.6	0.001	0.036	26.0	22.0	서울 중구 덕수궁길 15
4	서울 중구	도시대기	111121	중구	2020010105	0.002	0.6	0.001	0.035	25.0	19.0	서울 중구 덕수궁길 15
...	...	...	...	...	...	...	...	...	...	...	...	...
356755	인천 옹진군	도시대기	831493	영흥	2020013120	0.003	0.4	0.052	0.004	44.0	36.0	인천광역시 옹진군 영흥면 영흥로251번길 90
356756	인천 옹진군	도시대기	831493	영흥	2020013121	0.003	0.4	0.052	0.004	40.0	28.0	인천광역시 옹진군 영흥면 영흥로251번길 90
356757	인천 옹진군	도시대기	831493	영흥	2020013122	0.003	0.4	0.051	0.004	34.0	29.0	인천광역시 옹진군 영흥면 영흥로251번길 90
356758	인천 옹진군	도시대기	831493	영흥	2020013123	0.003	0.4	0.049	0.005	36.0	33.0	인천광역시 옹진군 영흥면 영흥로251번길 90
356759	인천 옹진군	도시대기	831493	영흥	2020013124	0.003	0.4	0.049	0.004	35.0	32.0	인천광역시 옹진군 영흥면 영흥로251번길 9
```

### 필요한 자료 추출
- 여기서 주소가 '서울 도봉구 시루봉로2길 34'인 자료만 추출한다
- 필요한 column(SO2, CO, O3, NO2, PM10)을 지정하여 추출한 데이터를 seoul_air_1부터 seoul_air_12에 대입
```python
for i in range(1, 13):
    globals()["seoul_air_{}.format(i)"] = globals()['seoul_air_{}.format(i)'][globals()['seoul_air_{}'.format(i)]['주소']
    == '서울 도봉구 시루봉로2길 34'][['측정일시', 'SO2', 'CO', 'O3', 'NO2', 'PM10']]

seoul_air_1.columns
------------------------------------------------------------------------------
Index(['측정일시', 'SO2', 'CO', 'O3', 'NO2', 'PM10'], dtype='object')
```

### 월별 데이터 연결
- concat()을 이용해 1월 데이터부터 12월 데이터까지 위->아래 방향으로 연결
- reset_index(drop=True)를 활용해 index를 재정의한다
```python
Total_Seoul_airkorea = pd.DataFrame()  //빈 데이터 프레임 생성

for i in range(1, 13):
  Total_Seoul_airkorea = pd.concat([Total_Seoul_airkorea, globals()['seoul_air_{}'.format(i)]])

Total_Seoul_airkorea = Total_Seoul_airkorea.reset_index(drop=True)
Total_Seoul_airkorea
----------------------------------------------------------------------------

측정일시	SO2	CO	O3	NO2	PM10
0	2020010101	0.002	0.5	0.011	0.024	19.0
1	2020010102	0.002	0.6	0.005	0.030	19.0
2	2020010103	0.002	0.6	0.002	0.033	27.0
3	2020010104	0.002	0.6	0.003	0.031	20.0
4	2020010105	0.002	0.7	0.003	0.031	21.0
...	...	...	...	...	...	...
8779	2020123120	0.002	0.4	0.014	0.026	29.0
8780	2020123121	0.002	0.4	0.017	0.021	23.0
8781	2020123122	0.002	0.4	0.025	0.013	28.0
8782	2020123123	0.002	0.3	0.030	0.008	24.0
8783	2020123124	0.002	0.3	0.027	0.011	15.0
8784 rows × 6 columns
```
air_korea 데이터를 보려면 반복문을 계속 써야 하므로 빈 데이터프레임에 전부 넣고 인덱스 초기화

### nan값(결측값) 확인
- isnull().values.any()는 해당 column에 nan값이 하나라도 있으면 True를 반환
```python
for i in range(len(Total_Seoul_airkorea.columns)):
  if(Total_Seoul_airkorea[Total_Seoul_airkorea.columns[i]].isnull().values.any() == True):
    print('nan이 하나라도 포함되어 있는 column:', Total_Seoul_airkorea.columns[i])
--------------------------------------------------------------------------------
nan이 하나라도 포함되어 있는 column: SO2
nan이 하나라도 포함되어 있는 column: CO
nan이 하나라도 포함되어 있는 column: O3
nan이 하나라도 포함되어 있는 column: NO2
nan이 하나라도 포함되어 있는 column: PM10
```

### -999값(결측값) 확인
- -999값도 결측값이므로 확인한다
```python
for i in range(len(Total_Seoul_airkorea.columns)):
  for j in range(len(Total_Seoul_airkorea)):
    if Total_Seoul_airkorea[Total_Seoul_airkorea.columns[i]][j] == -999:
      print('-999값이 있는 column:', Total_Seoul_airkorea.columns[i])
      break
```
### 평균
    - mean()을 이용해 각 컬럼의 평균값을 구하고 round()를 이용해 소수점 3자리까지 표시
```python
Total_Seoul_airkorea_mean = round(Total_Seoul_airkorea.mean(), 3)
Total_Seoul_airkorea_mean // round()는 원하는 소수점까지 반올림하는 내장 함수
--------------------------------------------------------------------------
0
측정일시	2.020067e+09
SO2	3.000000e-03
CO	4.030000e-01
O3	2.900000e-02
NO2	1.800000e-02
PM10	3.205700e+01
```
```python
for value in Total_Seoul_airkorea_mean:
  print(value)
------------------------------------------
2020066724.795
0.003
0.403
0.029
0.018
32.057
```

### mean()을 이용한 nan값 확인 및 nan컬럼 삭제
- 모든 값이 nan인 column을 저장할 remove_airkorea_columns list를 생성
- mean()을 적용한 값이 nan이면 그 column의 값은 모두 nan이다. 지금 데이터에는 이런 경우가 없음. // 모든 값이 nan이어서 계산 가능한 숫자가 없었다는 뜻
- np.isnan의 파라미터가 nan이면 True를 반환
- 모든 값이 nan인 column을 출력하고 remove_airkorea_columns에 그 column을 추가한다
- del을 이용해 remove_airkorea_columns에 있는 column을 삭제
```python
remove_airkorea_columns = []
for i in range(len(Total_Seoul_airkorea_mean)):
  if(np.isnan(Total_Seoul_airkorea_mean.iloc[i])):
    print('모든 값이 nan인 column:', Total_Seoul_airkorea.columns[i])
    remove_airkorea_columns.append(Total_Seoul_airkorea.columns[i])
```
```python
for i in range(len(remove_airkorea_columns)):
  print(remove_airkorea_columns[i], '제거')
  del Total_Seoul_airkorea[remove_airkorea_columns[i]]
```

### nan값 처리
- fillna 함수를 통해 nan값을 mean()값으로 대체한다
- 측정일시는 nan값이 없으므로 적용되지 않는다
```python
Total_Seoul_airkorea = Total_Seoul_airkorea.fillna(Total_Seoul_airkorea_mean)
Total_Seoul_airkorea
----------------------------------------------------------------------------------
측정일시	SO2	CO	O3	NO2	PM10
0	2020010101	0.002	0.5	0.011	0.024	19.0
1	2020010102	0.002	0.6	0.005	0.030	19.0
2	2020010103	0.002	0.6	0.002	0.033	27.0
3	2020010104	0.002	0.6	0.003	0.031	20.0
4	2020010105	0.002	0.7	0.003	0.031	21.0
...	...	...	...	...	...	...
8779	2020123120	0.002	0.4	0.014	0.026	29.0
8780	2020123121	0.002	0.4	0.017	0.021	23.0
8781	2020123122	0.002	0.4	0.025	0.013	28.0
8782	2020123123	0.002	0.3	0.030	0.008	24.0
8783	2020123124	0.002	0.3	0.027	0.011	15.0
8784 rows × 6 columns
```

### KMA 파일 불러오기
```python
kma = pd.read_csv('./2020_kma/2020_Seoul.csv', encoding='cp949')
print(kma)
-----------------------------------------------------------------
       지점 지점명                일시  기온(°C)  기온 QC플래그  강수량(mm)  강수량 QC플래그  \
0     108  서울  2020-01-01 00:00    -6.5       NaN      0.0        NaN   
1     108  서울  2020-01-01 01:00    -5.9       NaN      NaN        9.0   
2     108  서울  2020-01-01 02:00    -5.7       NaN      NaN        9.0   
3     108  서울  2020-01-01 03:00    -5.6       NaN      0.0        NaN   
4     108  서울  2020-01-01 04:00    -5.4       NaN      NaN        9.0   
...   ...  ..               ...     ...       ...      ...        ...   
8779  108  서울  2020-12-31 19:00    -7.1       NaN      NaN        9.0   
8780  108  서울  2020-12-31 20:00    -7.1       NaN      NaN        9.0   
8781  108  서울  2020-12-31 21:00    -7.2       NaN      NaN        9.0   
8782  108  서울  2020-12-31 22:00    -7.4       NaN      NaN        9.0   
8783  108  서울  2020-12-31 23:00    -7.6       NaN      NaN        9.0   

      풍속(m/s)  풍속 QC플래그  풍향(16방위)  ...  최저운고(100m )  시정(10m)  지면상태(지면상태코드)  \
0         0.0       NaN         0  ...          NaN     2000           NaN   
1         1.7       NaN        50  ...          7.0     2000           NaN   
2         0.1       NaN         0  ...          7.0     1988           NaN   
3         0.0       NaN         0  ...         14.0     2000           NaN   
4         0.0       NaN         0  ...          6.0     1908           NaN   
...       ...       ...       ...  ...          ...      ...           ...   
8779      2.4       NaN       250  ...          NaN     2000           NaN   
8780      3.2       NaN       250  ...          NaN     2000           NaN   
8781      2.7       NaN       250  ...          NaN     2000           NaN   
8782      2.5       NaN       270  ...          NaN     2000           NaN   
8783      2.2       NaN       290  ...          NaN     2000           NaN   

      현상번호(국내식)  지면온도(°C)  지면온도 QC플래그  5cm 지중온도(°C)  10cm 지중온도(°C)  \
0           5.0      -2.8         NaN          -0.8            0.7   
1           5.0      -2.4         NaN          -0.8            0.7   
2           5.0      -2.4         NaN          -0.8            0.6   
3           5.0      -2.7         NaN          -0.8            0.6   
4           5.0      -2.5         NaN          -0.8            0.6   
...         ...       ...         ...           ...            ...   
8779        NaN      -4.3         NaN          -0.3           -0.5   
8780        NaN      -5.2         NaN          -0.5           -0.5   
8781        NaN      -5.7         NaN          -0.6           -0.6   
8782        NaN      -6.1         NaN          -0.7           -0.6   
8783        NaN      -6.5         NaN          -0.8           -0.7   

      20cm 지중온도(°C)  30cm 지중온도(°C)  
0               2.1            3.2  
1               2.1            3.2  
2               2.0            3.1  
3               2.0            3.1  
4               2.0            3.0  
...             ...            ...  
8779            0.4            1.7  
8780            0.4            1.6  
8781            0.4            1.6  
8782            0.3            1.6  
8783            0.3            1.6  

[8784 rows x 38 columns]
```

### 필요한 자료 추출
- 필요한 column을 지정하여 추출한 데이터를 seoul_kma에 대입
- reset_index(drop=True)를 활용해 index 재정의
```python
seoul_kma = kma[['일시', '기온(°C)', '강수량(mm)', '풍속(m/s)', '습도(%)', '이슬점온도(°C)']]
seoul_kma = seoul_kma.reset_index(drop=True)
print(seoul_kma)
-----------------------------------------------------------------------------
                    일시  기온(°C)  강수량(mm)  풍속(m/s)  습도(%)  이슬점온도(°C)
0     2020-01-01 00:00    -6.5      0.0      0.0     38      -18.5
1     2020-01-01 01:00    -5.9      NaN      1.7     40      -17.3
2     2020-01-01 02:00    -5.7      NaN      0.1     42      -16.5
3     2020-01-01 03:00    -5.6      0.0      0.0     46      -15.4
4     2020-01-01 04:00    -5.4      NaN      0.0     50      -14.2
...                ...     ...      ...      ...    ...        ...
8779  2020-12-31 19:00    -7.1      NaN      2.4     58      -13.9
8780  2020-12-31 20:00    -7.1      NaN      3.2     59      -13.7
8781  2020-12-31 21:00    -7.2      NaN      2.7     61      -13.4
8782  2020-12-31 22:00    -7.4      NaN      2.5     66      -12.6
8783  2020-12-31 23:00    -7.6      NaN      2.2     65      -13.0

[8784 rows x 6 columns]
```

### nan값 확인
- isnull().values.any()는 해당 column에 nan값이 하나라도 있으면 True를 반환
```python
for i in range(len(seoul_kma.columns)):
  if(seoul_kma[seoul_kma.columns[i]]).isnull().values.any() == True:
    print('nan이 하나라도 있는 column:',seoul_kma.columns[i] )
------------------------------------------------------------------------
nan이 하나라도 있는 column: 강수량(mm)
```

### -999값 확인
- -999값도 결측값이므로 확인한다
```python
for i in range(len(seoul_kma.columns)):
  for j in range(len(seoul_kma)):
    if seoul_kma[seoul_kma.columns[i]][j] == -999:
      print('-999값이 있는 column:', seoul_kma.columns[i])
      break
```

### 평균
- mean()을 이용해 각 컬럼의 평균값을 구하고 round()를 이용해 소수점 3자리까지 표시
```python
seoul_kma_mean = round(seoul_kma.select_dtypes(include='number').mean(), 3)
seoul_kma_mean // 숫자타입만 평균을 내기 위해 select_dtypes를 씀
----------------------------------------------------------------------------
0
기온(°C)	13.270
강수량(mm)	1.556
풍속(m/s)	2.374
습도(%)	63.259
이슬점온도(°C)	5.725

dtype: float64
```

### mean()을 이용한 nan값 확인 및 nan 컬럼 삭제
- 모든 값이 nan인 column을 저장할 remove_kma_columns list를 생성
- mean()을 적용한 값이 nan이면 그 column의 값은 모두 nan이다. 지금 데이터에는 이런 경우가 없음. // 모든 값이 nan이어서 계산 가능한 숫자가 없었다는 뜻
- np.isnan의 파라미터가 nan이면 True를 반환
- 모든 값이 nan인 column을 출력하고 remove_kma_columns에 그 column을 추가한다
- del을 이용해 remove_kma_columns에 있는 column을 삭제
```python
remove_kma_columns = []
for i in range(len(seoul_kma_mean)):
  if(np.isnan(seoul_kma_mean.iloc[i])):
    print('모든 값이 nan인 column:', seoul_kma.columns[i])
    remove_kma_columns.append(seoul_kma.columns[i])

for i in range(len(remove_kma_columns)):
  print(remove_airkorea_columns[i], '제거')
  del seoul_kma[remove_kma_columns[i]]
```

### nan값 처리
- fillna 함수를 통해 nan값을 mean()값으로 대체 // fillna는 nan에만 값을 채우고 정상값은 건드리지 않음
- 측정일시는 nan값이 없으므로 적용되지 않는다
```python
Total_Seoul_kma = seoul_kma.fillna(seoul_kma_mean)
Total_Seoul_kma
----------------------------------------------------

일시	기온(°C)	강수량(mm)	풍속(m/s)	습도(%)	이슬점온도(°C)
0	2020-01-01 00:00	-6.5	0.000	0.0	38	-18.5
1	2020-01-01 01:00	-5.9	1.556	1.7	40	-17.3
2	2020-01-01 02:00	-5.7	1.556	0.1	42	-16.5
3	2020-01-01 03:00	-5.6	0.000	0.0	46	-15.4
4	2020-01-01 04:00	-5.4	1.556	0.0	50	-14.2
...	...	...	...	...	...	...
8779	2020-12-31 19:00	-7.1	1.556	2.4	58	-13.9
8780	2020-12-31 20:00	-7.1	1.556	3.2	59	-13.7
8781	2020-12-31 21:00	-7.2	1.556	2.7	61	-13.4
8782	2020-12-31 22:00	-7.4	1.556	2.5	66	-12.6
8783	2020-12-31 23:00	-7.6	1.556	2.2	65	-13.0
8784 rows × 6 columns
```

### columns 이름 지정
- AirKorea 데이터와 날짜, 시간을 기준으로 병합
- '일시'의 이름을 AirKorea와 똑같이 '측정일시'로 변경  
=> AirKorea의 측정일시를 기준으로 KMA를 병합할 것이기 때문
- 다른 column들도 이름을 알아보기 쉽게 변경(이건 본인 마음대로)
```python
Total_Seoul_kma.columns = ['측정일시', 'Seoul_Temp(°C)', 'Seoul_Precipitation(mm)', 'Seoul_Wind_Speed(m/s)', 'Seoul_Humidity(%)', 'Seoul_Dew_Point(°C)']
Total_Seoul_kma
---------------------------------------------------------------------------------
	측정일시	Seoul_Temp(°C)	Seoul_Precipitation(mm)	Seoul_Wind_Speed(m/s)	Seoul_Humidity(%)	Seoul_Dew_Point(°C)
0	2020-01-01 00:00	-6.5	0.000	0.0	38	-18.5
1	2020-01-01 01:00	-5.9	1.556	1.7	40	-17.3
2	2020-01-01 02:00	-5.7	1.556	0.1	42	-16.5
3	2020-01-01 03:00	-5.6	0.000	0.0	46	-15.4
4	2020-01-01 04:00	-5.4	1.556	0.0	50	-14.2
...	...	...	...	...	...	...
8779	2020-12-31 19:00	-7.1	1.556	2.4	58	-13.9
8780	2020-12-31 20:00	-7.1	1.556	3.2	59	-13.7
8781	2020-12-31 21:00	-7.2	1.556	2.7	61	-13.4
8782	2020-12-31 22:00	-7.4	1.556	2.5	66	-12.6
8783	2020-12-31 23:00	-7.6	1.556	2.2	65	-13.0
8784 rows × 6 columns
```

- Airkorea와 KMA의 시간 형식이 다름  
=> 2020010101(AirKorea) vs 2020-01-01 00:00(KMA)
- 또한, AirKorea의 경우 다음날 0시를 전날 24시로 표현하는 반면, KMA는 24시라고 표현하지 않고 
다음날 0시로 표현  
=> 예를 들어, AirKorea는 12월 31일 24시, KMA는 1월 1일 0시 

### 이렇게 형식이 다르면 AirKorea오 KMA를 병합할 수 없기 때문에 시간은 0~23시로, format은 datetime 형식으로 통일하는 함수를 생성

### AirKorea 측정일시 데이터 처리
- AirKorea의 시간을 00시~23시로 맞추기 위해 '측정일기'의 format을 지정하는 airkorea_num_to_datetime 함수를 생성
- 해당 함수는 24시 데이터를 다음날 00시로 처리한다.
- '측정일시'의 8,9번째 숫자(즉,시간)가 24이면, 
(기존의 년, 월) + (00시) + (기존의 분)에 일(days)만 1을 더하여 다음날 00시로 변경
- return 시 format을 datetime 형식으로 반환
- format='%y%m%d%H' : 연.월.일.시 즉, 2020010101가 2020-01-01 01:00로 해석됨
```python
def airkorea_num_to_datetime(date_num):
  date_str = str(date_num)
  if date_str[8:10] != '24':
    return pd.to_datetime(date_str, format='%Y%m%d%H')
  else: 
    date_str = date_str[0:8] + '00' + date_str[10:]
    return pd.to_datetime(date_str, format='%Y%m%d%H', errors='raise') + dt.timedelta(days=1)
```

### KMA 측정일시 데이터 처리
- KMA 측정일시 데이터는 00시~23시이다.
- 하지만, KMA의 측정일시 데이터 format은 string이기 때문에 datetime형식으로 반환하는 KMA_num_datetime 함수를 생성한다.  
=> AirKorea의 측정일시 데이터 처리와 동일하게 맞춤
```python
def KMA_num_to_datetime(date_num):
  date_str = str(date_num)
  date_str = date_str[0:4] + date_str[5:7] + date_str[8:10] + date_str[11:13]
  return pd.to_datetime(date_str, format='%Y%m%d%H')
  ```

### 구현한 함수 적용
- (Total_Seoul_airkorea의 행의 수) = (Total_Seoul_kma의 행의 수)이므로 for문의 range에 len(Total_Seoul_kma['측정일시'])를 해도 상관없다
- 각각의 Total 데이터(Total_Seoul_airkorea, Total_Seoul_kma)에 구현한 함수를 적용한다
```python
for i in range(len(Total_Seoul_airkorea['측정일시'])):
  Total_Seoul_airkorea.at[i, '측정일시'] = airkorea_num_to_datetime(Total_Seoul_airkorea['측정일시'][i])
  Total_Seoul_kma.at[i, '측정일시'] = KMA_num_to_datetime(Total_Seoul_kma['측정일시'][i])
  ```
### AirKorea 데이터
```python
Total_Seoul_airkorea
---------------------------------------------------------
측정일시	SO2	CO	O3	NO2	PM10
0	2020-01-01 01:00:00	0.002	0.5	0.011	0.024	19.0
1	2020-01-01 02:00:00	0.002	0.6	0.005	0.030	19.0
2	2020-01-01 03:00:00	0.002	0.6	0.002	0.033	27.0
3	2020-01-01 04:00:00	0.002	0.6	0.003	0.031	20.0
4	2020-01-01 05:00:00	0.002	0.7	0.003	0.031	21.0
...	...	...	...	...	...	...
8779	2020-12-31 20:00:00	0.002	0.4	0.014	0.026	29.0
8780	2020-12-31 21:00:00	0.002	0.4	0.017	0.021	23.0
8781	2020-12-31 22:00:00	0.002	0.4	0.025	0.013	28.0
8782	2020-12-31 23:00:00	0.002	0.3	0.030	0.008	24.0
8783	2021-01-01 00:00:00	0.002	0.3	0.027	0.011	15.0
8784 rows × 6 columns
```

### KMA 데이터
```python
Total_Seoul_kma
--------------------------------------------------------------
	측정일시	Seoul_Temp(°C)	Seoul_Precipitation(mm)	Seoul_Wind_Speed(m/s)	Seoul_Humidity(%)	Seoul_Dew_Point(°C)
0	2020-01-01 00:00:00	-6.5	0.000	0.0	38	-18.5
1	2020-01-01 01:00:00	-5.9	1.556	1.7	40	-17.3
2	2020-01-01 02:00:00	-5.7	1.556	0.1	42	-16.5
3	2020-01-01 03:00:00	-5.6	0.000	0.0	46	-15.4
4	2020-01-01 04:00:00	-5.4	1.556	0.0	50	-14.2
...	...	...	...	...	...	...
8779	2020-12-31 19:00:00	-7.1	1.556	2.4	58	-13.9
8780	2020-12-31 20:00:00	-7.1	1.556	3.2	59	-13.7
8781	2020-12-31 21:00:00	-7.2	1.556	2.7	61	-13.4
8782	2020-12-31 22:00:00	-7.4	1.556	2.5	66	-12.6
8783	2020-12-31 23:00:00	-7.6	1.556	2.2	65	-13.0
8784 rows × 6 columns
```

### AirKorea + KMA = Total_Date_Seoul
- Airkorea 데이터의 '측정일시'를 기준으로 KMA 데이터를 병합하기 때문에 
2020-01-01 01:00:00 ~ 2021-01-01 00:00:00의 데이터만 정상적으로 병합된다
- KMA 데이터의 2020-01-01 00:00:00 데이터는 병합자체는 되지만 2021_01_01 00:00:00는 데이터가 없기 때문에 nan값으로 병합된다
```python
Total_Data_Seoul = pd.merge(Total_Seoul_airkorea, Total_Seoul_kma, on='측정일시', how='left')
Total_Data_Seoul
--------------------------------------------------------------------------------
	측정일시	SO2	CO	O3	NO2	PM10	Seoul_Temp(°C)	Seoul_Precipitation(mm)	Seoul_Wind_Speed(m/s)	Seoul_Humidity(%)	Seoul_Dew_Point(°C)
0	2020-01-01 01:00:00	0.002	0.5	0.011	0.024	19.0	-5.9	1.556	1.7	40.0	-17.3
1	2020-01-01 02:00:00	0.002	0.6	0.005	0.030	19.0	-5.7	1.556	0.1	42.0	-16.5
2	2020-01-01 03:00:00	0.002	0.6	0.002	0.033	27.0	-5.6	0.000	0.0	46.0	-15.4
3	2020-01-01 04:00:00	0.002	0.6	0.003	0.031	20.0	-5.4	1.556	0.0	50.0	-14.2
4	2020-01-01 05:00:00	0.002	0.7	0.003	0.031	21.0	-5.2	1.556	0.0	55.0	-12.8
...	...	...	...	...	...	...	...	...	...	...	...
8779	2020-12-31 20:00:00	0.002	0.4	0.014	0.026	29.0	-7.1	1.556	3.2	59.0	-13.7
8780	2020-12-31 21:00:00	0.002	0.4	0.017	0.021	23.0	-7.2	1.556	2.7	61.0	-13.4
8781	2020-12-31 22:00:00	0.002	0.4	0.025	0.013	28.0	-7.4	1.556	2.5	66.0	-12.6
8782	2020-12-31 23:00:00	0.002	0.3	0.030	0.008	24.0	-7.6	1.556	2.2	65.0	-13.0
8783	2021-01-01 00:00:00	0.002	0.3	0.027	0.011	15.0	NaN	NaN	NaN	NaN	NaN
8784 rows × 11 columns
```

### 마지막 행 삭제
- 마지막 행 2020-01-01 00:00:00의 KMA데이터가 nan값으로 병합되었으므로 삭제한다
- 최종적으로 행의 개수는 8783
```python
Total_Data_Seoul = Total_Data_Seoul.drop(len(Total_Data_Seoul)-1)
```

Total 데이터 저장
- 완성한 Total_Data_Seoul을 csv로 저장한다
- 빨간색 부분은 파일이 저장될 경로 및 이름을 설정하는 부분
- indx=True를 해주면 index에 대한 행이 추가로 생기므로 False로 설정한다
```python
Total_Data_Seoul.to_csv('./Total_Data_Seoul.csv', header=True, index=False)
```

### 시계열 데이터 전처리
- 우리는 현재 시간을 포함한 이전 2시간 데이터, 즉, 총 3시간의 데이터를 활용하여 1시간 뒤의 미세먼지를 예측하고자 한다
- 따라서, 우리는 시간적 특성을 반영하기 위해 1개의 행에 3시간씩 들어가게끔 Dataframe을 재구성한다.  
=> 이를 window sliding이라고 한다.

### extract_inputoutput 함수를 통한 window sliding 구현
```python
def extract_inputoutput(dataframe, lookback_time=3, predict_time=1): // (시계열 데이터, 과거 몇 개 시점을 볼지, 몇 step뒤를 예측할지)

// X, y를 담을 빈 Dataframe 생성
  dfx = pd.Dataframe() // 입력데이터 X
  dfy = pd.Dataframe() // 정답데이터 y

  for i in range(len(dataframe) - (lookback_time - 1) - (predict_time)): // 뒤에 최소 3개는 남아있어야 예측이 가능하기 때문

    if i % 1000 == 0: // 1000개 단위로 진행 상황 출력, 데이터가 많아서 작업이 멈춘게 아님을 확인하기 위함
      print(i)
    
    rowx = []
    for timestep in range(lookback_time):
      dfRename = dataframe.lioc[[i + timestep]]
      dfRename.index = [i]
      rowx.append(dfRename)
    rowx = pd.concat(rowx, axis=1)
    dfx = pd.concat([dfx, rowx])
    // 3개 데이터를 rowx에 넣음 -> 그걸 가로로(열) 합침 -> 그걸 dfx에 전부 세로로(행) 추가 

    rowy = []
    rowy = pd.Dataframe([dataframe['PM10'][i + lookback_time]])
    dfy = pd.concat([dfy, rowy], ignore_index=True)
  
  print('X, Y 데이터 분류 완료')
  return dfx, dfy
```
- 즉, 1시,2시,3시 미세먼지 농도를 보고 4시에는 미세먼지가 이러했으므로 너가 이걸 보고 학습해서 예측해봐 라고 하기 위한 학슴데이터를 만든것임

### 함수 적용
- extract_inputoutput 함수에 최종 Dataframe인 Total_Data_Seoul을 파라미터로 넣어준다
```python
X, Y = extract_inputoutput(Total_Data_Seoul)
-------------------------------------------------
0
1000
2000
3000
4000
5000
6000
7000
8000
X, Y 데이터 분류 완료
```
- 만든 dfx, dfy를 X, Y에 넣음. 그리고 진행을 확인하기 위해 1000 단위로 진행상황을 print함

### X(독립변수) 최종 데이터
- 11개의 columns을 3시간씩 같은 행에 연결했기 때문에 총 33개의 columns을 생성한다
```python
X
------------------------------------------------------------------------------

측정일시	SO2	CO	O3	NO2	PM10	Seoul_Temp(°C)	Seoul_Precipitation(mm)	Seoul_Wind_Speed(m/s)	Seoul_Humidity(%)	...	SO2	CO	O3	NO2	PM10	Seoul_Temp(°C)	Seoul_Precipitation(mm)	Seoul_Wind_Speed(m/s)	Seoul_Humidity(%)	Seoul_Dew_Point(°C)
0	2020-01-01 01:00:00	0.002	0.5	0.011	0.024	19.0	-5.9	1.556	1.7	40.0	...	0.002	0.6	0.002	0.033	27.0	-5.6	0.000	0.0	46.0	-15.4
1	2020-01-01 02:00:00	0.002	0.6	0.005	0.030	19.0	-5.7	1.556	0.1	42.0	...	0.002	0.6	0.003	0.031	20.0	-5.4	1.556	0.0	50.0	-14.2
2	2020-01-01 03:00:00	0.002	0.6	0.002	0.033	27.0	-5.6	0.000	0.0	46.0	...	0.002	0.7	0.003	0.031	21.0	-5.2	1.556	0.0	55.0	-12.8
3	2020-01-01 04:00:00	0.002	0.6	0.003	0.031	20.0	-5.4	1.556	0.0	50.0	...	0.002	0.7	0.002	0.032	23.0	-4.8	0.000	1.9	58.0	-11.8
4	2020-01-01 05:00:00	0.002	0.7	0.003	0.031	21.0	-5.2	1.556	0.0	55.0	...	0.002	0.7	0.002	0.032	22.0	-4.6	1.556	2.1	62.0	-10.7
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
8775	2020-12-31 16:00:00	0.002	0.3	0.031	0.008	20.0	-5.5	1.556	3.5	47.0	...	0.002	0.3	0.027	0.011	24.0	-6.7	0.000	2.5	54.0	-14.4
8776	2020-12-31 17:00:00	0.002	0.3	0.029	0.010	24.0	-6.1	1.556	1.8	50.0	...	0.002	0.4	0.016	0.023	26.0	-7.1	1.556	2.4	58.0	-13.9
8777	2020-12-31 18:00:00	0.002	0.3	0.027	0.011	24.0	-6.7	0.000	2.5	54.0	...	0.002	0.4	0.014	0.026	29.0	-7.1	1.556	3.2	59.0	-13.7
8778	2020-12-31 19:00:00	0.002	0.4	0.016	0.023	26.0	-7.1	1.556	2.4	58.0	...	0.002	0.4	0.017	0.021	23.0	-7.2	1.556	2.7	61.0	-13.4
8779	2020-12-31 20:00:00	0.002	0.4	0.014	0.026	29.0	-7.1	1.556	3.2	59.0	...	0.002	0.4	0.025	0.013	28.0	-7.4	1.556	2.5	66.0	-12.6
8780 rows × 33 columns
```

### Y(종속변수) 최종 데이터
- 가장 처음에 2020-01-01:00:00~2020-01-01 03:00:00 데이터를 기준으로 2020-01-01 04:00:00의 PM10 값을 예측한다
- 2020-01-01 04:00:00의 PM10값인 20.0이 가장 첫 행에 나타난다
- 2020-01-01 04:00:00~2020-12-31 23:00:00의 'PM10'값은 총 8780행이다.
```python
Y
-------------------------------------------------------------------------------
	0
0	20.0
1	21.0
2	23.0
3	22.0
4	21.0
...	...
8775	26.0
8776	29.0
8777	23.0
8778	28.0
8779	24.0
8780 rows × 1 columns
```

### X, Y 최종 데이터 저장
- 완성한 X, Y를 csv로 저정한다
- 빨간색 부분은 파일이 저장될 경로 및 이름을 설정하는 부분
- index=True를 해주면 index에 대한 다른 행이 추가로 생기므로 False로 설정
```python
X.to_csv('./Total_Data_Seoul_X.csv', header=True, index=False)
Y.to_csv('./Total_Data_Seoul_Y.csv', header=True, index=False)
```

### 전처리 완료!!!! 하하하
