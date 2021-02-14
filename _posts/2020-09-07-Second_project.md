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

 데이터 수집

  1. 공공데이터(상권매출, 상권영역, 행정동코드)
  2. 크롤링(네이버 부동산, 다음 부동산 매물정보)
  
 데이터 처리

  1. 사용자의 입력을 기준으로 매출을 정렬 
  2. 인공지능으로 추정 매출을 구한다.
  3. 매출을 기준으로 해당 위치의 x.y 좌표 찾는다
  4. 해당 위치의 좌표를 위도와 경도로 변환한다
  5. 해당 위치를 크롤링한다
  
 시스템 구현

  1. 홈페이지 검색란에 검색을 하면 검색어를 중점으로하여 데이터를 필터링을 하여 데이터 처리를 하고 정제된 데이터를 지도상에 표기한다.



  
 느낀점
 
 개발 일정

 역할분담

 next

 결과물


<center>데이터 수집</center>
      
## 1. 데이터 셋

<figure>
    <img src="/assets/img/dataset1.png">
</figure>

## 2. 데이터 전처리 과정

<figure>
	<img src="/assets/img/youtube_view/data_preprocess1.png">
	<img src="/assets/img/youtube_view/data_preprocess2.png">
	<img src="/assets/img/youtube_view/data_preprocess3.png">
</figure>
    
## 3. 모델

<figure class="third">
	<img src="/assets/img/youtube_view/model_result1.png">
	<img src="/assets/img/youtube_view/model_result2.png">
	<img src="/assets/img/youtube_view/model_result3.png">
</figure>
---

<center>댓글 데이터로 좋아요를 예측하는 알고리즘</center>

## 1. 데이터 셋

<figure>
    <img src="/assets/img/youtube_view/dataset1.png">
</figure>

## 2. 데이터 전처리 과정

<figure class="second">
	<img src="/assets/img/youtube_view/data_preprocess4.png">
	<img src="/assets/img/youtube_view/data_eda2.png">
</figure>
## 3. 모델

<figure class="third">
	<img src="/assets/img/youtube_view/modeling2.png">
	<img src="/assets/img/youtube_view/model_test1.png">
	<img src="/assets/img/youtube_view/model_test2.png">
</figure>
---

<center>전체 데이터 셋에서 유튜브 조회수 예측</center>


## 1. 데이터 셋
<figure class="third">
	<img src="/assets/img/youtube_view/dataset1.png">
	<img src="/assets/img/youtube_view/like_destribution.png">
	<img src="/assets/img/youtube_view/like_destribution_result.png">
</figure>


### 느낀점
간단한 LSTM 모델로 정확한 유튜브 조회수를 예측하기는 힘들지만 나름 성과가 있었고, 
제목이 조회수에 미치는 영향이 크다는것을 이 프로젝트를 하면서 느낄수 있었다.

#### next
이 프로젝트를 되돌아 보며 아쉬운점이 참 많았는데 그 중 2가지를 꼽자면 데이터, 모델 분야라 할수있었다.

##### 데이터
<figure>
    <img src="/assets/img/youtube_view/dataset+api.png">
</figure>

내가 가지고 온 데이터는 캐글에서 크리스마스날을 기준으로 가지고 온 데이터여서 시간에 대해 많은 영향을 끼쳤던것으로 예상이 되었다.
그래서 기존의 데이터에 유튜브 API 를 통한 크롤링한 데이터를 더한다면 시간에 강건성이 있고 데이터의 양이 증가하여 모델 성능에 기여했을것이라 생각된다.

##### 모델분야

당시에는 빠르게 프로젝트를 하는 입장이라 간단한 모델을 적용하였지만 돌이켜 생각해보니 BERT 모델을 써서 모델의 정확도를 높일수 있지 않았을까 하는 생가이 들었다.
또 stopword 로 tokenize 하였지만 모델 결과는 binary classfication 이기 때문에 sentencepiece 를 통해 단어를 추출하였다면 어땟을까 하는 아쉬움도 들었다.

---

### 전체코드
