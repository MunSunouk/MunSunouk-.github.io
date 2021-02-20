---
layout: post
title:  "싼 임대료, 비싼 수입 찾을수 있을까?"
date:   2020-08-26
excerpt: "요식업을 중심으로 하여 업종별, 지역별 로 나누어서 최적의 매출을 낼수 있는 곳을 추천한다"
project: true
tag:
- 부동산 
- blog
- about
- project
comments: true
---
<center><img src="/assets/img/real_estate/real_estate.png"></center>
   
<center>상권분석을 통한 부동산 추천시스템▪</center>
     
 ▪.한국의 자영업자 비율이 25.1 % 로 높은 가운데 음식업 폐업률이 18.1 % 로 업종중에 가장 높다. 코로나 사태로 언택트 시대가 가속화 되고 있어서 요식업은 배달이 필수인 시대로 가고있다.
   이러한 이유로 음식업은 배달을 염두하며 창업을 해야하며, 영업을 이어나아가야 하기 때문에 업종과 지역을 신중히 선택해야 한다.

 ▪ 상권조사를 하기 위해서는 여러가지 조사가 필요하고 각각의 조사에는 전문성이 필요하다. 이러한 이유로 상권조사를 개개인이 하기에는 힘듦으로 전문적인 시스템이 필요하다

 ▪ 서울시 우리마을가게 서비스는 자료를 제공해주지 추천해주지도 판단해 주지도 않는다. 개발하려는 서비스는 추천을 통하여 판단을 도와준다


 ## 과정
<figure>
    <img src="/assets/img/real_estate/process.png">
</figure>

 필터링

 좌표생성기

 크롤링

 데이터 분석 그래프
  
 시스템 구현

 1. 홈페이지 검색란에 검색을 하면 검색어를 중점으로하여 데이터를 필터링을 하여 데이터 처리를 하고 정제된 데이터를 지도상에 표기한다.

 느낀점
 
 개발 일정

 역할분담

 next

 결과물


<center>필터링</center>
      
```
def filter_model(market_name, location):
    df_result3 = df1[df1['서비스_업종_코드_명'] == market_name]
    df_result4 = df_result3[df_result3['읍면동명'] == location]
    
    df_result5 = df_result4.sort_values(by='당월_매출_금액_x', ascending= False)
    
    return list(df_result5['상권_코드'].unique()[:5])

filter_model('한식음식점','인수동')

# -> [1000334, 1000343, 1000344, 1000345]
```
---

<center>좌표생성기</center>

```      
def get_x_y(market_name, location):
    df_result3 = df1[df1['서비스_업종_코드_명'] == market_name]
    df_result4 = df_result3[df_result3['읍면동명'] == location]
    
    df_result5 = df_result4.sort_values(by='당월_매출_금액_x', ascending= False)
    
		location = []
    for i in df_result5['상권_코드'].unique()[:5]:
    a = df_result5[df_result5['상권_코드'] == i]
    result = list(a[['엑스좌표_값_x','와이좌표_값_x']].values[0])
    location.append(result)
    
    return location

get_x_y(market_name, location)

# -> [[201388, 459892], [201323, 459612], [201136, 460373], [200764, 460432]]
```

카카오 WTM -> WGS84좌표 변환기를 활용하여 좌표 획득후 네이버 부동산 API 를 활용한 해당 좌표 근처 임대 시세 획득하기
```
import requests
import json
import logging
def ch_LatLng(x,y):
    url =f"https://dapi.kakao.com/v2/local/geo/transcoord.json?input_coord=WTM&output_coord=WGS84&x={x}&y={y}"

    APP_KEY = 'app_key 획득후 넣기'
    headers = {'Authorization': 'KakaoAK {}'.format(APP_KEY)}

    api_test = requests.get(url,headers=headers)

    url_text = json.loads(api_test.text)

    return(url_text['documents'][0].values())

print(ch_LatLng(get_x_y[0][0],get_x_y[0][1]))

def get_area(x,y):
    url = f"https://dapi.kakao.com/v2/local/geo/coord2address.json?input_coord=WTM&x={x}&y={y}" # 변환계, x좌표,y 좌표
    APP_KEY = 'app_key 획득후 넣기'
    headers = {'Authorization': 'KakaoAK {}'.format(APP_KEY)}

    api_test = requests.get(url,headers=headers)

    url_text = json.loads(api_test.text)

    return((url_text['documents'][0]['address']['region_2depth_name']))

print(get_area(get_x_y[0][0],get_x_y[0][1]))

# -> dict_values([126.95614529124468, 37.47818636030597]) , 관악구
```
---

<center>크롤링</center>

## 1. 사용자의 입력을 기준으로 매출을 정렬

<figure>
    <img src="/assets/img/real_estate/data_preprocess1.png">
	<img src="/assets/img/real_estate/data_preprocess2.png">
</figure>

```
AREA_LOC={"강남구":11680, "강동구":11740, "강북구":11305, "강서구":11500, "관악구":11620,
          "광진구":11215, "구로구":11530, "금천구":11545, "노원구":11350, "도봉구":11320, 
          "동대문구":11230, "동작구":11590, "마포구":11440, "서대문구":11410, "서초구":11650, 
          "성동구":11200, "성북구":11290, "송파구":11710, "양천구":11470, "영등포구":11560, 
          "용산구":11170, "은평구":11380, "종로구":11110, "중구":11140, "중랑구":11260}

def get_soup(area,latitude,longitude,page):
    url = "https://m.land.naver.com/cluster/ajax/articleList"
    HEAD = {
        'User-Agent': "PostmanRuntime/7.20.0",
        'Accept': "*/*",
        'Cache-Control': "no-cache",
        'Postman-Token': "adbba748-cb85-4fb4-8f6a-4be441f19cc3",
        'Host': "m.land.naver.com",
        'Accept-Encoding': "gzip, deflatitudee",
        'Connection': "keep-alive",
        'cache-control': "no-cache"
        }
    area = str(area * 100000)
    latitude = str(latitude)
    longitude = str(longitude)
    
    ##행정동 및 위도 경도##변수 입력 
    dong={"cortarNo":area}
    lat={"lat":latitude}
    lon={"lon":longitude}
    page={"page":page}
  ##고정값
    con1= {"rletTpCd":"SG"} ## 검색물건 상가 
    con2= {"tradTpCd":"B2"} ##거래방법 B1=전세, B2=월세
    con3={"showR0":""}    ## 그외조건 없음
    con4 = {"z":"14"}  ## 축척

  ##mapping 고정값
    rgt = {"rgt": str(float(lat["lat"])+0.02)}
    lft = {"lft": str(float(lat["lat"])-0.02)}
    top = {"top": str(float(lon["lon"])+0.02)}
    btm = {"btm": str(float(lon["lon"])-0.02)}
    qstring={**con1,**con2,**con3,**con4,**dong,**lat,**lon,**lft,**rgt,**top,**btm,**page}
    result=requests.request("GET",url,headers=HEAD,params=qstring,timeout=5)
    
		if result.status_code != 200:
        logging.error('invalid status: %d' % resp.status)
        exit

		data = json.loads(resp.text)

    # 가져올것 : body['tradTpNm','flrinfo','prc','rentPrc']매매방식, 층, 보증금, 월세

    for item in data['body']:
        # if btm,top,lft,rgt
        logging.info('%s층 %s보증금 %s월세' % (item['tradTpNm'],item['flrinfo'],item['prc'],item['rentPrc']))

# 예제
area = AREA_LOC[get_area(get_x_y[0][0],get_x_y[0][1])]
latitude = ch_LatLng(get_x_y[0][0],get_x_y[0][1])[0]
longitude = ch_LatLng(get_x_y[0][0],get_x_y[0][1])[1]
page = 1
get_soup(area,latitude,longitude,page)

# -> INFO:root:08월 4층 51,500만원
	INFO:root:08월 9층 55,000만원
	INFO:root:08월 16층 56,000만원
	INFO:root:08월 14층 54,000만원
	INFO:root:08월 18층 53,000만원
	INFO:root:08월 10층 53,700만원
	INFO:root:07월 24층 54,300만원
	INFO:root:06월 5층 50,500만원
	INFO:root:05월 12층 50,000만원
	INFO:root:05월 9층 50,800만원
	INFO:root:05월 5층 48,000만원
	INFO:root:05월 21층 50,000만원
	INFO:root:05월 3층 43,200만원
	INFO:root:05월 26층 48,000만원

```

<center>점수 생성기</center>

```
def score(market_code,industry_name):
    df_result1 = df1[df1['서비스_업종_코드_명'] == industry_name]
    
    score = []
    for i ,code in enumerate(market_code):
        df_result2 = df_result1[df_result1['상권_코드'] == code]
        
        store_score = df_result2['점포수_x'][0] * 5 * 2
        population_score = df_result2['총_유동인구_수'][0] * 1.5 * 2
        land_score = df_result2['공시지가'][0] * 2 * 2
        industry_score =  df_result2['업종수'][0] * 2 * 2
        result = store_score + population_score + land_score + industry_score
        print(f'추천{i + 1}의 점수 : {result}')

score([1000334,1000334],'한식음식점')

# -> 추천1의 점수 : 677143.1227010442
추천2의 점수 : 677143.1227010442
```


<center>데이터 분석 그래프	</center>


## 1. 공시지가 비교 그래프

<figure>
    <img src="/assets/img/real_estate/real_estate_estimate.png">
</figure>

```
# df_1 : '공시지가_비교_data.csv'

gong_mean_rate = df_1['매출별 공시지가 비율'].mean()
money_mean = df_1['리얼_매출'].mean()

mean_data = {'상권_코드_명':'서울평균','리얼_매출':money_mean,'매출별 공시지가 비율':gong_mean_rate,'리얼_qcut':'3단계'}

# 뀰지표에서 나온 상권 다섯가지의 상권 코드를 리스트로
service = '한식음식점'
list_=[1000744, 1000773, 1000774, 1000739, 1000743] #상권코드


def visual(service,list_):
    service = service
    list_=list_
    df_ = df_1.copy()
    
    df_ = df_1[df_1['서비스_업종_코드_명'] == service]

    df_qcut = pd.qcut(df_['매출별 공시지가 비율'],5,labels=['1단계','2단계','3단계','4단계','5단계'])

    df_['리얼_qcut'] = df_qcut
    df_ = df_[df_['상권_코드'].isin(list_)]
    
    df_ = df_[['상권_코드_명','리얼_매출','매출별 공시지가 비율','리얼_qcut']]
    df_ = df_.append(mean_data, ignore_index=True)

    df_ = df_.set_index('상권_코드_명')
    
    for i in range(len(list_)):
        if i==0:
            df_['리얼_매출'].plot.bar(color=('#0099CC','#FFCC33', '#FFCC33',  '#FFCC33','#FFCC33','#666666'),alpha=0.5)

        elif i==1:
            df_['리얼_매출'].plot.bar(color=('#FFCC33', '#0099CC','#FFCC33',  '#FFCC33','#FFCC33','#666666'),alpha=0.5)

        elif i==2:
            df_['리얼_매출'].plot.bar(color=('#FFCC33', '#FFCC33', '#0099CC', '#FFCC33','#FFCC33','#666666'),alpha=0.5)

        elif i==3:
            df_['리얼_매출'].plot.bar(color=('#FFCC33', '#FFCC33', '#FFCC33','#0099CC', '#FFCC33','#666666'),alpha=0.5)

        elif i==4:
            df_['리얼_매출'].plot.bar(color=('#FFCC33', '#FFCC33','#FFCC33','#FFCC33', '#0099CC', '#666666'),alpha=0.5)
            
        plt.ylabel('매출_금액')
        plt.xlabel('상권명')
        plt.twinx()
        df_['매출별 공시지가 비율'].plot(color='r',legend=True)

        plt.savefig(f'공시지가_비교_{list_[i]}.png')    # 그래프 png로 저장
        plt.show()
        
    print(df_)
    
visual(service, list_)

```
## 1. 공시지가 예측 그래프

<figure>
    <img src="/assets/img/real_estate/real_estate_estimate(1).png">
</figure>

```
# 공시지가 예측
# df : '공시지가_predict_data.csv'
# district_list : 상권 5개 코드 리스트

from sklearn.preprocessing import PolynomialFeatures
from sklearn.pipeline import make_pipeline  # 데이터 변환 과정과 머신러닝을 연결해주는 파이프라인
from sklearn.linear_model import LinearRegression, Ridge, Lasso, ElasticNet

df = pd.read_csv('공시지가_predict_data.csv',encoding='cp949')

def population_pred(district_list):
    df__ = df.copy()
    
    # 5개의 상권 코드 리스트로 for 돌림
    for code in district_list:
        
        # 불러온 상권만 뽑아서 유동인구 데이터 테이블 생성
        df_com_code = df__[df__['상권_코드']==code]
        # x축은 년과 분기
        x = pd.DataFrame(df_com_code['기준년월'])
        # y축은 유동인구
        y = df_com_code['공시지가_동평균']
        # 유동인구는 19년2분기까지 존재하고, 예측을 20년 4분기까지 진행
        a = pd.DataFrame(np.array([2020.7,2021.1,2021.7]).reshape(-1,1), columns = ['기준년월'])
        # 이전 년도 데이터까지 예측을 위해 만듬
        b = pd.concat([x, a], axis=0)
        b = pd.DataFrame(b).reset_index().drop('index',axis=1)
        b = b.drop_duplicates()
        # LinearRegression 으로 예측 진행
        model_lr = make_pipeline(PolynomialFeatures(degree=9, include_bias=False),
                                 LinearRegression())
        model_lr.fit(x, y)
        # 예측값 저장
        prediction_lr = model_lr.predict(X = b)
        
        # title을 만들어 주기 위해 상권명 뽑아오기
        code_name = df_com_code['상권_코드_명'].unique().tolist()
        code_name = "".join(code_name)
        # 그래프 작성
        plt.title(f'{code_name}의 그래프')
        plt.plot(b["기준년월"], prediction_lr, color="blue", label='예상 공시지가')
        plt.scatter(df_com_code['기준년월'], df_com_code['공시지가_동평균'], color='green', label='실제 공시지가')
        plt.xticks(rotation=90)
        plt.xlabel('기준 년도')
        plt.ylabel('공시지가')
        plt.legend()
        # 저장
        plt.savefig(f'savefig_gongsi_{code}.png', bbox_inches='tight', pad_inches=0.5, edgecolor='grey')
        plt.show()

district_list=[1000744, 1000773, 1000774, 1000739, 1000743]
population_pred(district_list)

```

## 2. 유동인구 비교 그래프

<figure>
    <img src="/assets/img/real_estate/move_pop_estimate.png">
</figure>

```
# 필요 라이브러리
import csv
import numpy as np
import pandas as pd

import matplotlib.pyplot as plt
import seaborn as sns 

# 한글 폰트가 적용 안되어서 폰트를 적용해주는 것
from matplotlib import font_manager, rc

font_name = font_manager.FontProperties(fname="c:/Windows/Fonts/malgun.ttf").get_name()
rc('font', family=font_name)

# 경고메시지 무시
import warnings
warnings.filterwarnings("ignore")

from sklearn.preprocessing import PolynomialFeatures
from sklearn.pipeline import make_pipeline  # 데이터 변환 과정과 머신러닝을 연결해주는 파이프라인
from sklearn.linear_model import LinearRegression, Ridge, Lasso, ElasticNet

data1 = pd.read_csv('./data/서울시 우리마을가게 상권분석서비스(상권영역).csv',encoding = 'cp949')
data2 = pd.read_excel('./data/행정동_법정동_20200814.xlsx')
data3 = pd.read_csv('./data/서울시 우리마을가게 상권분석서비스(상권-추정매출)_2019_2분기.csv',encoding = 'cp949')
data4 = pd.read_csv('./data/서울시 우리마을가게 상권분석서비스(상권-추정유동인구).csv', encoding='cp949')

# 필요 데이터 프레임 생성
def chose_data(data1, data2, data3, data4, industry, area):
    # data1 : 상권 영역
    data1['행정동_코드'] = data1['행정동_코드']*100
    data1 = data1.drop(['기준_년월_코드', '형태정보'],axis = 1)
    
    # data2 : 상권 행정동코드
    data2.drop(['생성일자','말소일자'], axis=1, inplace=True)
    data2.dropna(axis=0, inplace=True)
    data2 = data2[data2['시도명'] == '서울특별시']
    data2.columns = ['행정동_코드', '시도명', '시군구명', '읍면동명', '법정동_코드', '동리명']
    data2 = data2.loc[:, ['행정동_코드', '시군구명','읍면동명']]
    data2 = data2.drop_duplicates()
    
    # 상권 정보
    df1 = pd.merge(data1, data2, on='행정동_코드')
    
    # data3 : 상권 추정 매출
    data3 = data3.dropna(axis=0)
    data3 = data3.drop(['기준_년_코드','기준_분기_코드'],axis = 1)
    # 상권코드 int형 전환
    data3_code = []
    for i in data3['상권_코드']:
        data3_code.append(int(i))

    data3['상권_코드'] = data3_code

    # 점포수가 0개인 곳은 해당 업종의 총 평균으로 넣어줌
    mean_num = {}
    for i in data3['서비스_업종_코드_명'].unique():
        mean_num.update({i : np.trunc(data3[data3['서비스_업종_코드_명']==i]['점포수'].mean())})

    for i in mean_num.items():
        data3.loc[(data3['서비스_업종_코드_명']==i[0])&(data3['점포수']==0), '점포수'] = i[1]
        
    # data3_0
    df3_1 = data3[data3['서비스_업종_코드_명']==industry]
    df3_1 = df3_1.loc[:, ['상권_구분_코드','상권_구분_코드_명','상권_코드','상권_코드_명','당월_매출_금액','당월_매출_건수','점포수']]
    df3_1.columns = ['상권_구분_코드','상권_구분_코드_명','상권_코드','상권_코드_명','업종_매출_금액','업종_매출_건수','업종_점포수']

    # 상권 전체 데이터 또한 보여줌
    df3_2 = data3.groupby(by=['상권_구분_코드','상권_구분_코드_명','상권_코드','상권_코드_명'])['당월_매출_금액','당월_매출_건수','점포수'].sum()
    # df3_2
    df3_3 = data3.groupby(by=['상권_구분_코드','상권_구분_코드_명','상권_코드','상권_코드_명'])['서비스_업종_코드'].count()
    df3_3 = pd.DataFrame(df3_3)
    df3_3.columns = ['서비스업종수']
    # df3_3
    df3_4 = pd.merge(df3_3, df3_2, left_index=True, right_index=True, how='left')
    df3_4.columns = ['소분류업종수', '총_매출_금액', '총_매출_건수','총_점포수']
    df3_4 = df3_4.reset_index()

    # 상권별 전체 업종의 합과 치킨만의 데이터를 생성
    df3 = pd.merge(df3_4, df3_1, on=['상권_구분_코드','상권_구분_코드_명','상권_코드','상권_코드_명'], how='outer')
    # 상권에 해당 업종이 없는 곳은 0 으로 채워줌)
    df3.fillna(0, inplace=True)
    #print(df3_0.info())
    
    # data4 : 상권 추정유동인구
    df4 = data4[(data4['기준 년코드']==2019) & (data4['기준_분기_코드']==2)]
    df4 = df4.loc[:, [' 상권_구분_코드',' 상권_구분_코드_명','상권_코드','상권_코드_명','총_유동인구_수','남성_유동인구_수',
                      '여성_유동인구_수']]
    df4.columns = ['상권_구분_코드', '상권_구분_코드_명','상권_코드','상권_코드_명','총_유동인구_수','남성_유동인구_수','여성_유동인구_수']
    # 등급별 나누기
    df4['유동인구등급']  = pd.qcut(df4['총_유동인구_수'], 5, labels=np.arange(1, 6, 1))
    
    # 전체 데이터 만들기
    df = pd.merge(df1, df3, on=['상권_구분_코드','상권_구분_코드_명','상권_코드','상권_코드_명'])
    df = pd.merge(df, df4, on=['상권_구분_코드','상권_구분_코드_명','상권_코드','상권_코드_명'])
    # 골목 상권만 진행
    df = df[df['상권_구분_코드']=='A']
    
    # 원하는 지역 검색
    # 시군구명 이름 찾기
    data_list = []
    kor_list = []
    for word in df['시군구명'].unique():
        if area in word:
            kor_list.append(word)

    for kor in kor_list:
        data_list.append(df[df['시군구명']==kor])

    # 음면동명 이름 찾기
    kor_list = []
    for word in df['읍면동명'].unique():
        if area in word:
            kor_list.append(word)

    for kor in kor_list:
        data_list.append(df[df['읍면동명']==kor])

    # 상권명 이름 찾기
    kor_list = []
    for word in df['상권_코드_명'].unique():
        if area in word:
            kor_list.append(word)

    for kor in kor_list:
        data_list.append(df[df['상권_코드_명']==kor])

    df_area = 0

    # 데이터를 검색했는데 빈리스트로 돌아올 시 concat을 해결하기 위해서 진행
    if data_list != []:
        # 하나의 테이블로 만들어 준 후 중복된 값 제거
        df_area = pd.concat(data_list).sort_values(by='업종_매출_금액', ascending=False)
        df_area.dropna(axis=0, inplace=True)
        df_area = df_area.drop_duplicates()
        # df_area_rank = pd.qcut(df_area['총_유동인구_수'], 5, labels=np.arange(1, 6, 1))
        # df_area['유동등급']  = df_area_rank
        df_area = df_area.head()
        district_list = df_area['상권_코드'].unique()

    else:
        print('에러에러에러에러에러')

    return df, df_area, district_list

df, df_area, district_list = chose_data(data1, data2, data3, data4, '치킨전문점', '강남')

district_list
# 출력
# array([1000935, 1000922, 1000926, 1000930, 1000931], dtype=int64)

# district_list 에서 사용자가 선택한 코드를 고를 수 있게 해줘야함
# data_area : 업종, 지역으로 필터링된 상권,
# area: 찾고자 하는 상권 명
# code : 사용자가 선택한 상권
def code_fig(data_area, area, district_list):
    for code in district_list:
        fig, ax0 = plt.subplots()
        # 오른쪽 y축 작성을 위해 twinx()
        ax1 = ax0.twinx()
        ax0.set_title(f"{area}으로 검색한 유동인구와 상권의 전체 매출금액")
        # 사용자가 선택한 상권만 들거 옴
        mask1 = data_area[data_area['상권_코드']==code]
        # data_area 전체 그래프
        ax0.bar(x='상권_코드_명',height='총_유동인구_수',data=data_area, label='총 유동인구',alpha=0.5)
        # 사용자가 선택한 그래프
        ax0.bar(x='상권_코드_명', height='총_유동인구_수', data=mask1, label='선택한 유동인구', alpha=0.5, color='black')
        ax0.set_ylabel("총 유동인구")
        ax0.grid(False)
        # 총_매출_금액 을 업종_매출_금액으로 변경하면 업종 매출 금액으로 생성
        ax1.plot(data_area['상권_코드_명'], data_area['업종_매출_금액'], label='상권 매출금액', color='red')
        ax1.scatter(mask1['상권_코드_명'], mask1['업종_매출_금액'], label='선택한 상권 매출금액', color='y', marker='*', s=80)
        ax1.set_ylabel("업종 총 매출 금액")
        ax1.grid(False)
        ax0.set_xlabel("상권명")
        ax0.set_xticklabels(labels=data_area['상권_코드_명'], rotation=90)
        plt.legend()
        plt.savefig(f'savefig_area_pop_{area}.png', bbox_inches='tight', pad_inches=0.5, edgecolor='grey')
        plt.show()

def population_visual(list_):
    df_ = df_area.copy()

    df_qcut = pd.qcut(df_['총_유동인구_수'],5,labels=['1단계','2단계','3단계','4단계','5단계'])

    df_['유동인구_등급'] = df_qcut
    df_ = df_[df_['상권_코드'].isin(list_)]
    
    df_ = df_[['상권_코드_명','업종_매출_금액','총_유동인구_수','유동인구_등급']]
    df_ = df_.append(mean_data, ignore_index=True)

    df_ = df_.set_index('상권_코드_명')
    
    for i in range(len(list_)):
        if i==0:
            df_['총_유동인구_수'].plot.bar(color=('#0099CC','#FFCC33', '#FFCC33',  '#FFCC33','#FFCC33','#666666'),alpha=0.5)

        elif i==1:
            df_['총_유동인구_수'].plot.bar(color=('#FFCC33', '#0099CC','#FFCC33',  '#FFCC33','#FFCC33','#666666'),alpha=0.5)

        elif i==2:
            df_['총_유동인구_수'].plot.bar(color=('#FFCC33', '#FFCC33', '#0099CC', '#FFCC33','#FFCC33','#666666'),alpha=0.5)

        elif i==3:
            df_['총_유동인구_수'].plot.bar(color=('#FFCC33', '#FFCC33', '#FFCC33','#0099CC', '#FFCC33','#666666'),alpha=0.5)

        elif i==4:
            df_['총_유동인구_수'].plot.bar(color=('#FFCC33', '#FFCC33','#FFCC33','#FFCC33', '#0099CC', '#666666'),alpha=0.5)
            
        plt.ylabel('매출_금액')
        plt.xlabel('상권명')
        plt.twinx()
        df_['업종_매출_금액'].plot(color='r',legend=True)

        plt.savefig(f'유동인구_비교_{list_[i]}.png', bbox_inches='tight', pad_inches=0.5, edgecolor='grey')    # 그래프 png로 저장
        plt.show()
        
    print(df_)

population_visua(district_list)

```

## 2. 유동인구 예측 그래프

<figure>
    <img src="/assets/img/real_estate/move_pop_estimate(1).png">
	<img src="/assets/img/real_estate/move_pop_estimate(2).png">
	<img src="/assets/img/real_estate/move_pop_estimate(3).png">
</figure>

```
# 유동인구 예측
# data_population : 서울시 우리마을가게 상권분석서비스(상권-추정유동인구).csv'
# district_list : 상권 5개 코드 리스트
def population_pred(data_population, district_list):
    # 필요한 컬럼 가저오기
    data_population = data_population.loc[:, ['기준 년코드','기준_분기_코드',' 상권_구분_코드',' 상권_구분_코드_명','상권_코드','상권_코드_명','총_유동인구_수','남성_유동인구_수','여성_유동인구_수']]
    # 컬럼 이름 변경
    data_population.columns = ['기준_년_코드','기준_분기_코드','상권_구분_코드', '상권_구분_코드_명','상권_코드','상권_코드_명','총_유동인구_수','남성_유동인구_수','여성_유동인구_수']
    # data_population['유동인구등급']  = pd.qcut(data_population['총_유동인구_수'], 5, labels=np.arange(1, 6, 1))
    
    # 상권코드 str형 전환
    # 데이터가 2014, 1분기 이런식으로 컬럼으로 떨어저 있으서 붙이기 위한 작업
    # 2014.1 <<< 이런 식으로
    # 기준년과 기준분기
    population_y = []
    for i in data_population['기준_년_코드']:
        population_y.append(str(i))

    population_q = []
    for i in data_population['기준_분기_코드']:
        population_q.append(str(i))
    # 2014.1 로 만들어 주는 과정
    population_yq = []
    for i in range(len(data_population)):
        population_yq.append(population_y[i]+'.'+population_q[i])
    # data_population 에 새로운 컬럼으로 더해줌
    data_population['기준년분기'] = population_yq
    
    # 5개의 상권 코드 리스트로 for 돌림
    for code in district_list:
        # 불러온 상권만 뽑아서 유동인구 데이터 테이블 생성
        df_com_code = data_population[data_population['상권_코드']==code]
        # x축은 년과 분기
        x = pd.DataFrame(df_com_code['기준년분기'])
        # y축은 유동인구
        y = df_com_code['총_유동인구_수']
        # 유동인구는 19년2분기까지 존재하고, 예측을 20년 4분기까지 진행
        a = pd.DataFrame(np.array(['2019.3', '2019.4', '2020.1','2020.2','2020.3','2020.4']).reshape(-1,1), columns = ['기준년분기'])
        # 이전 년도 데이터까지 예측을 위해 만듬
        b = pd.concat([x, a], axis=0)
        
        # LinearRegression 으로 예측 진행
        model_lr = make_pipeline(PolynomialFeatures(degree=9, include_bias=False),
                                 LinearRegression())
        model_lr.fit(x, y)
        # 예측값 저장
        prediction_lr = model_lr.predict(X = b)
        
        # title을 만들어 주기 위해 상권명 뽑아오기
        code_name = df_com_code['상권_코드_명'].unique().tolist()
        code_name = "".join(code_name)
        # 그래프 작성
        plt.title(f'{code_name}의 그래프')
        plt.plot(b["기준년분기"], prediction_lr, color="blue", label='예상 인구수')
        plt.scatter(df_com_code['기준년분기'], df_com_code['총_유동인구_수'], color='green', label='실제 인구수')
        plt.xticks(rotation=90)
        plt.xlabel('기준 년도 및 분기')
        plt.ylabel('총 유동인구')
        plt.legend()
        # 저장
        plt.savefig(f'savefig_population_{code}.png', bbox_inches='tight', pad_inches=0.5, edgecolor='grey')
        plt.show()

population_pred(data4, district_list)

```
## 2. 점포수 비교 그래프

<figure>
    <img src="/assets/img/real_estate/compare.png">
</figure>

```
list_=[1000744, 1000773, 1000774, 1000739, 1000743] #상권코드


#df, 업종, 상권 코드 리스트
industry_list = ['한식음식점', '양식음식점', '분식전문점', '패스트푸드점', '제과점', '커피·음료', '일식음식점',  '호프·간이주점']
def FIND_GRAPH_LSIT_TO_QCUT(df, service, business_list) : #'COLUMN 점포수
    temp_df = df[df['서비스_업종_코드_명'] == service]
    temp_df['QCUT'] = pd.qcut(temp_df['점포_수'],  5, duplicates='drop') #LABELS 값 설정시 오류
    temp_df['QCUT_명'] = 0
    temp_qcut = sorted(temp_df['QCUT'].unique()).copy()
    qcut_list = []
    for index, row in temp_df.iterrows() :
        for q_index in range(len(temp_qcut)) : 
            if row['QCUT'] == temp_qcut[q_index] :
                qcut_list.append('{}단계'.format(q_index+1))
    temp_df['QCUT_명'] = qcut_list
    
    temp_df.sort_values('QCUT_명', ascending = True, inplace= True)
    df_business = temp_df[temp_df['상권_코드'].isin(business_list)]
    df_business = df_business[['상권_코드_명', '당월_매출_금액','점포_수', 'QCUT_명']]
    
    aver_data = {'상권_코드_명': '평균', '당월_매출_금액' : temp_df['당월_매출_금액'].mean(),'점포_수' : temp_df['점포_수'].mean(), 'QCUT_명' : '3단계'}
    
    df_business = df_business.append(aver_data,ignore_index=True)
    
    df_business = df_business.set_index('상권_코드_명')
    print(len(df_business['점포_수']))
    if len(df_business['점포_수']) > 0 :
        for i in range(len(df_business['점포_수'])-1):
            clrs = ['#FFCC33','#FFCC33','#FFCC33','#FFCC33','#FFCC33','#666666']
            clrs[len(df_business['점포_수'])-1] = '#666666'
            if i==0:
                clrs[i]='#0099CC'
                df_business['점포_수'].plot.bar(color=clrs,alpha=0.5)

            elif i==1:
                clrs[i]='#0099CC'
                df_business['점포_수'].plot.bar(color=clrs,alpha=0.5)

            elif i==2:
                clrs[i]='#0099CC'
                df_business['점포_수'].plot.bar(color=clrs,alpha=0.5)

            elif i==3:
                clrs[i]
            title_text = '{} 업종 점포수'.format(service)
            plt.title(title_text, fontsize=14)
            plt.ylabel('점포수')
            plt.xlabel('상권명')
            plt.twinx()
            df_business['당월_매출_금액'].plot(color='r',legend=True)
            plt.savefig(f'점포수_비교_{list_[i]}.png')    # 그래프 png로 저장
            plt.show()
    else :
        print('해당 점포가 존재하지않습니다.')
    #business_df['당월_매출_금액'].plot(color='orange',figsize=(15,6))
    print(df_business)
    return df_business

```

<center>시스템 구현</center>

 프론트엔드 : HTML, CSS, javascript 사용
 백엔드 : django, sqllite 사용

 <figure>
    <img src="/assets/img/real_estate/show(1).png">
    <img src="/assets/img/real_estate/show(2).png">
</figure>

### 느낀점
처음으로 긴 프로젝트를 해보았다. 이 프로젝트를 하면서 data resolution 을 많이 생각하게 되었었고,
내가 배웠던것을 총 동원해서 만들어서 굉장히 뿌듯한 프로젝트였다. 팀원분들도 잘 따라와 주었고 협업을 잘하여 멋진 결과가 나왔던것 같다.

#### next
이 프로젝트를 되돌아 보며 아쉬운점이 참 많았는데 그 중 2가지를 꼽자면 데이터, 프론트 분야라 할수있었다.

##### 데이터

내가 가지고 온 데이터는 공공 데이터로써 데이터가 정밀하게 조정되어있지 않았다. 그리하여 필요한 데이터를 찾기 위해 많은 고생을 하였지만
데이터 측면에서 아쉬움이 많은 프로젝트 였던것같다.

##### 프론트 분야

마지막에 프론트 엔드를 꾸미는 일을 해서 그런지 꼼꼼히 하지 못했었다. 지도 api 를 가져와서 동적으로 만들고 싶었지만 시간 관계상 정적으로 가져왔었고 search engine 쪽을 제대로 꾸미지 못했고 비슷한 api 를 끌어써서 마무리 했던게 아쉬울 나름이다. 나중에 시간이 있다면 sarcch engine 쪽도 다뤄서 더 정확한 결과가 나오게끔 유도하고싶다.
---

