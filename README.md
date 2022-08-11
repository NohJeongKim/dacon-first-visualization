# 쇼핑몰 지점별 매출액 분석시각화 경진대회

안녕하세요, 처음으로 대회를 나갑니다. 제가 Promotion 부분에 NaN 값이 많아서, 어떤 값을 채워야되나 고민이 많았습니다. 통계학적 지식이 부족하여 잘 모르겠어서 공부를 더 하고 채워보도록 하겠습니다. (다른 분들 코드를 살펴보았을 때, 시계열에 관한 내용이 많았는데, 잘 몰라서 공부한 뒤에 다시 분석해보도록 하겠습니다.) 첫 대회인 만큼 주어진 시간 안에서 해결하고 싶었습니다. 부족한 점이 있으면, 댓글로 조언 남겨주시길 바랍니다. 감사합니다.

## 1. 필요한 라이브러리 불러오기.

제가 사용할 라이브러리를 별칭을 붙여서 불러왔습니다.

## 2. 데이터 불러오기.

csv 파일이기 때문에, pd.read_csv() 함수를 이용해서 데이터를 불러왔습니다. train data와 test data가 따로 존재하기 때문에, 따로 불러왔습니다. 그리고 데이터 프레임의 모양을 확인했습니다.

하지만 데이터는 전처리 과정을 거쳐야 하기 때문에, 깊은 복사를 이용해서 데이터를 변경해도 원본은 그대로 보존될 수 있도록 만들었습니다. (전처리를 잘 못하는 경우, 다시 되돌아가기 위함입니다.)

## 3. 데이터 확인하기 및 원하는 방식으로 조정하기.

우리가 시각화에 이용할 데이터는 train data 입니다. 그 데이터가 어떻게 생겼는지 확인해보았습니다. 일단 데이터 타입을 확인하고, 데이터 타입 변환을 진행하였습니다. Date는 datetime이라는 데이터 타입이 있음에도 불구하고, object이기에 datetime으로 변경하였습니다. 이를 통해 Date column에서 원하는 정보를 얻을 수 있었습니다. 그래서 일단 새로운 column들을 만들어서 원하는 정보를 넣어주었습니다.

그리고 Temperature가 화씨 온도라서 아래의 식을 이용해서 섭씨 온도로 바꾸어 주었습니다. 아래의 식이 담긴 함수를 만들어서 apply() 함수를 이용하여 섭씨 온도로 변경하였습니다.

```
섭씨 온도 = (화씨 온도 - 32) x (5/9) # 섭씨 온도 구하는 식
```

그리고 datetime의 형태를 Year, Month, Day로 쪼갰습니다.

여기까지 처리하면, 데이터 타입에 관하여 데이터를 잘 조정하였습니다.

그리고 결측치가 몇 개가 있는지 확인하였습니다. Promotion columns을 제외하고는 모두 결측치가 존재하지 않습니다. Promotion 결측치를 어떤 값을 넣어서 어떻게 처리할 것인지는 더 고민해봐야 합니다.

Store 정보와 Date 정보를 알아보도록 하겠습니다.
Store는 1 ~ 45 종류의 지점이 존재하고, 모든 지점은 139개씩 있었습니다. 그리고 Date는 139개의 날짜가 존재하고, 각각의 날짜가 45개씩 존재하였습니다. 둘 다 결측치는 없었습니다.

이러한 인사이트를 통해, 우리는 일단 결측치가 없는 값 부터 시각화를 처리할 수 있습니다. 그런 다음에 결측치를 어떻게 처리할지 고민하고, 처리한 값을 통해서 시각화를 하여 인사이트를 얻을 수 있습니다.

## 4. 데이터 전처리 및 시각화하기.

### 4-1. 휴일 유뮤의 개수 파악하기.

압도적으로 휴일이 아닌 날이 휴일인 날보다 더 많았습니다. (5805개 > 450개)

![](https://velog.velcdn.com/images/tino-kim/post/ec10599d-34c2-4caf-b17d-382dcb928bb8/image.png)

### 4-2. 연도 별 Store 지점과 매출액 비교하기.

2010년이나 2011년에 최고 매출인 경우가 많았습니다. 그리고 최소 매출인 경우도 꽤 있었습니다. 한 달 내에서 매출액의 변동이 대체로 큰 편입니다. 하지만, 2012년은 대체로 한 달 내에서 매출액의 변동이 작은 편에 속합니다. 대체로 한 구역에 분포하기 때문입니다. 그리고 Store 지점 중 매출액 변동이 큰 지점들이 존재합니다.

![](https://velog.velcdn.com/images/tino-kim/post/c6cbbbc0-414b-43ca-b739-6069be8dc968/image.png)

1. 2010년 Store 지점과 매출액, 휴일 유무 비교하기.

대체로 휴일날 매출액이 높을 줄 알았지만, 휴일이 아닌 날에 매출액이 높았습니다. 그리고 Store 1 ~ 27 지점까지 변동이 대체로 큰 편 입니다.

![](https://velog.velcdn.com/images/tino-kim/post/ed184e59-d5b4-4e27-8bc5-33c9cabd1663/image.png)

2. 2011년 Store 지점과 매출액, 휴일 유무 비교하기.

대체로 휴일날 매출액이 높을 줄 알았지만, 휴일이 아닌 날에 매출액이 높았습니다. 그리고 Store 1 ~ 27 지점까지 변동이 대체로 큰 편 입니다.

![](https://velog.velcdn.com/images/tino-kim/post/f54d9a52-fe31-4223-ba37-1baa1433b5ea/image.png)

3. 2012년 Store 지점과 매출액, 휴일 유무 비교하기.

대체로 휴일날 매출액이 높을 줄 알았지만, 휴일이 아닌 날에 매출액이 높았습니다. 그리고 대체로 변동은 2010년, 2011년 기준 그래프에 비하면 적은 편에 속합니다.

![](https://velog.velcdn.com/images/tino-kim/post/994628ba-9912-4294-bdf2-92ce695f97bd/image.png)

### 4-3. 연도 별 실업률과 매출액 비교하기.

대체로 실업률과 매출액은 반비례 관계입니다. 실업률이 낮을 수록 매출액이 큰 편이고, 실업률이 높을 수록 매출액이 적은 편입니다. 제 생각에는 직원의 수가 적으면, 그 만큼 회전율이 적어져서 매출액이 감소하는 경향을 보여주는 것 같습니다.

1. 2010년 Store 지점 별 실업률과 매출액 비교하기.

Store 지점 별 실업률의 차이가 굉장히 크며, 그에 따라 매출액의 변동도 굉장히 큰 것을 볼 수 있습니다. 그리고 달 마다 실업률의 변동은 그리 크지 않음을 알 수 있습니다.

![](https://velog.velcdn.com/images/tino-kim/post/b4c6f9f0-692b-42b4-8037-2a6322a532ec/image.png)

2. 2011년 Store 지점 별 실업률과 매출액 비교하기.

Store 지점 별 실업률의 차이가 굉장히 크며, 그에 따라 매출액의 변동도 굉장히 큰 것을 볼 수 있습니다. 그리고 달 마다 실업률의 변동은 그리 크지 않음을 알 수 있습니다.

![](https://velog.velcdn.com/images/tino-kim/post/8ab9e0d2-d680-4fe4-ae77-75b66886e0b1/image.png)

3. 2012년 Store 지점 별 실업률과 매출액 비교하기.

Store 지점 별 실업률의 차이가 굉장히 크며, 그에 따라 매출액의 변동도 굉장히 큰 것을 볼 수 있습니다. 그리고 달 마다 실업률의 변동은 그리 크지 않음을 알 수 있습니다.

![](https://velog.velcdn.com/images/tino-kim/post/41d73f79-d916-4b26-9e9e-b183119c168a/image.png)

3개의 그래프 모두 비슷한 경향을 보여주고 있습니다. 달 마다 실업률의 변동은 그리 크지 않지만, 지점 별로 실업률의 차이가 굉장히 큽니다.

### 4-4. 연도 별 월별 온도의 평균 비교하기.

연도 별로 월별 평균 온도가 크게 차이나지 않습니다. 기온은 다 비슷합니다.

![](https://velog.velcdn.com/images/tino-kim/post/617856f3-37a8-4b20-8386-7bd6381d187e/image.png)

온도와 매출액의 변동과 관계가 있는지 확인하기 위해서, 선 그래프를 그려보았습니다.

![](https://velog.velcdn.com/images/tino-kim/post/384d3600-739d-4035-866d-d552c075e775/image.png)

1. 2010년 월별 (최대 매출액) - (최소 매출액) 의 변동 비교하기.

4월, 8월에 (최대 매출액) - (최소 매출액) 변동이 적은 편에 속합니다.
4월에는 14도이며, 8월에는 26도 입니다.

2. 2011년 월별 (최대 매출액) - (최소 매출액) 의 변동 비교하기.

7월에 (최대 매출액) - (최소 매출액) 변동이 적은 편에 속합니다.
7월에는 27도 입니다.

3. 2012년 월별 (최대 매출액) - (최소 매출액) 의 변동 비교하기.

7월에 (최대 매출액) - (최소 매출액) 변동이 적은 편에 속합니다.
7월에는 26도 입니다.

이 그래프를 통해서 알 수 있는 사실은 2012년은 다른 연도에 비해서 (최대 매출액) - (최소 매출액) 변동이 큰 편에 속합니다. 이는 하루 매출액의 차이가 다른 연도에 비해서 큰 편임을 알 수 있습니다. 그리고 날씨와는 관련성이 없어 보입니다. 다른 연도의 14도, 26도라도 변동이 큰 월도 있기 때문입니다.

### 4-5. 표준 상관 계수를 구해서 비교하기.

1에 가까우면 정비례 관계이고, -1에 가까우면 반비례 관계임을 알 수 있습니다.

![](https://velog.velcdn.com/images/tino-kim/post/bb650d2c-ae03-4fe0-9bb6-8fe908b5600d/image.png)

1. 강한 정비례 관계 : Promotion1 ~ Promotion4 간의 관계, Year ~ Fuel_price 간의 관계

2. 위의 경우 보다는 약한 정비례 관계 : Weekly_Sales ~ Promotion 1 간의 관계, Weekly_Sales ~ Promotion 4 간의 관계, IsHoliday ~ Promotion 2 간의 관계, IsHoliday ~ Promotion 3 간의 관계

### 4-6. 강한 정비례 관계 확인하기.

1. Promotion1 ~ Promotion4 간의 관계

groupby를 이용해서 연도 별 지점 별 Promotion1, Promotion4 최빈도를 구했습니다. 2010년에는 Promotion1 NaN 값이 많아서 그래프를 그릴 수 없었습니다. 그래서 subplots 도화지 간격을 다르게 하기 위해서 gridspec을 이용하였습니다. 연도 별로 지점 별 Promotion1, Promotion4 간의 관계를 그려보니, 대체로 정비례 관계와 비슷한 경향을 보여주었습니다.

![](https://velog.velcdn.com/images/tino-kim/post/e21875e6-64e1-4b8a-a4a4-6b8eda170c58/image.png)

2. Year ~ Fuel_Price 간의 관계

연도 별 Fuel Price를 알기 위해서 평균, 중앙값, 그리고 최빈값을 구해주었습니다. 그리고 연도 별 Fuel Price와의 평균, 중앙값, 최빈값의 그래프를 그려보니, 3개의 그래프 모두 정비례 관계와 비슷한 경향을 보여주었습니다.

![](https://velog.velcdn.com/images/tino-kim/post/54b8d214-c880-4f32-8200-9d8639577127/image.png)

### 4-7. 위의 경우보다 약한 정비례 관계 확인하기.

1. Weekly_Sales ~ Promotion1, Promotion5 간의 관계

x축을 Weekly_Sales로, y축을 Promotion1/ Promotion5 로 설정하고 사이즈와 색깔은 Weekly_Sales를 기준으로 만들어주었습니다. 2개의 그래프 모두 위의 경우보다는 약하지만 정비례 관계와 비슷한 경향을 띄었습니다.

![](https://velog.velcdn.com/images/tino-kim/post/56ce7751-0846-4f57-b321-c928dc1a1b2e/image.png)

2. IsHoliday ~ Promotion2, Promotion3 간의 관계

원하는 색상으로 팔레트를 지정해주고 싶어서, 팔레트를 직접 만들었습니다. 그런 다음 Promotion2, Promotion3의 평균과 중앙값을 각각 구해주었습니다. 그리고 JointGrid를 이용해서 hist와 scatter를 한번에 보여주었습니다. 대략적인 분포를 더욱 편하게 보기 위해서 이상점을 제외하고 x축과 y축 범위를 지정해주었습니다. 그리고 axvline, axhline을 이용해서 Promotion2, Promotion3의 평균과 중앙값을 찍어주었습니다. scatter를 보면, 위의 경우보다 약하지만 정비례 관계를 확인할 수 있습니다.

![](https://velog.velcdn.com/images/tino-kim/post/36512ed5-be03-4bf9-8c81-9e96c4a307eb/image.png)

## 결론

여러 요인끼리 묶어서 데이터의 분포를 알아보았습니다. Promotion1 값을 어떻게 채워야 할지 몰라서 못 한점이 많이 아쉽습니다. 제 자료를 봐주셔서 감사합니다.

<hr>

### 참고 문서 링크 모음

- 참고 블로그 : [figure, figsize가 먹히지 않는 경우](https://chunggaeguri.tistory.com/entry/Data-Analysis-seaborn-figure-%EC%82%AC%EC%9D%B4%EC%A6%88-%EC%A1%B0%EC%A0%88%ED%95%98%EB%8A%94-%EB%B2%95)

- 참고 블로그 : [matplotlib Image Save](https://codetorial.net/matplotlib/savefig.html)

- 참고 블로그 : [변수 비교 | \*args VS \*\*kwargs](https://brunch.co.kr/@princox/180)

- 참고 블로그 : [matplotlib title 조정](https://hojjimin-statistic.tistory.com/7)

- 참고 StackoverFlow : [FacetGrid add title](https://stackoverflow.com/questions/29813694/how-to-add-a-title-to-seaborn-facet-plot)

- 참고 StackoverFlow : [plot point marker](https://stackoverflow.com/questions/52385428/plot-point-markers-and-lines-in-different-hues-but-the-same-style-with-seaborn)

- 참고 공식 문서 : [pivot 안 되는 경우 pivot_table 이용하기](https://www.statology.org/valueerror-index-contains-duplicate-entries-cannot-reshape/)

- 참고 공식 문서 : [groupby mode apply](https://www.statology.org/pandas-groupby-mode/)

- 참고 git 문서 : [JointGrid Text & axline 이용하는 경우](https://github.com/mwaskom/seaborn/issues/2306)

- 참고 StackoverFlow : [JointGrid에 line 지정하는 방식](https://stackoverflow.com/questions/56116021/add-arbitrary-lines-on-seaborn-jointplot)
