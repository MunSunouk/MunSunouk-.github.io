---
layout: post
title:  "유튜브 조회수도 과학이다"
date:   2020-08-26
excerpt: "자극적인 제목만이 답일까?"
project: true
tag:
- Youtube 
- blog
- about
- project
comments: true
---
.. image:: assets/img/youtube.jfif
    :target: assets/img/youtube.jfif
    :height: 300px
    
<center>유튜브 조회수 제목에 영향을 줄까?</center>
     
 요즘들어 긴 시간을 소모해야하는 tv, 넷플릭스 보다 유튜브를 더 자주 보는것같다. 하루에도 10편 이상은 보는것 같은데, 초창기와는 다르게 점점 썸네일에 어그로(?)가 끌리는 편이다. 그래서 내 첫번째 프로젝트로 유튜브 제목과 조회수의 인과관계를 보려고 한다. 과연 영향을 줄까?

 ## 과정

 전체 데이터 셋에서 유튜브 조회수 예측
  1. 데이터셋
  2. 데이터 전처리 과정
  3. 모델
  
 유튜브 제목 에서 유튜브 조회수 예측
  1. 데이터셋
  2. 데이터 전처리 과정
  3. 모델
  
 댓글 데이터로 좋아요를 예측하는 알고리즘
  1. 데이터셋
  2. 데이터 전처리 과정
  
 느낀점
 
 next

<center>전체 데이터 셋에서 유튜브 조회수 예측</center>
      
## 1. 데이터 셋

<figure>
	<a href="https://drive.google.com/file/d/1PeLtTs1x2YU0euNuqNkTQp85Z8rGXtZ1/view?usp=sharing"><img src="https://drive.google.com/file/d/1PeLtTs1x2YU0euNuqNkTQp85Z8rGXtZ1/view?usp=sharing"></a>
</figure>

## 2. 데이터 전처리 과정

{% capture Data_images1 %}
	https://drive.google.com/file/d/11B1m-xTjKynpRxvmNyiKjX0vDuoiOkXe/view?usp=sharing
	https://drive.google.com/file/d/1JP-dEMVfgd0DxEjh8L-Du2NeOkFhv9Pm/view?usp=sharing
	https://drive.google.com/file/d/1M-JmBheY_IKy9EaRQEIONR1HkZ3mmBeD/view?usp=sharing
{% endcapture %}
{% include gallery images=Data_images1 cols=3 %}
    
## 3. 모델

{% capture Model_images1 %}
	https://drive.google.com/file/d/1jzrj6Wde6didf8jJh4z11oyVXaR9_qu8/view?usp=sharing
	https://drive.google.com/file/d/1Iq6xZUCw-GNGA-ej7bXUrnxpUVfDiLbO/view?usp=sharing
	https://drive.google.com/file/d/1WluFlUf_wCXtYswSMJ1PDYLkagiYoc9Q/view?usp=sharing
{% endcapture %} 
{% include gallery images=Model_images1 cols=3 %}
---

<center>댓글 데이터로 좋아요를 예측하는 알고리즘</center>

## 1. 데이터 셋

<figure>
	<a href="https://drive.google.com/file/d/18DM1VFKzlajCpjC9wvj40tZYJmRRSye2/view?usp=sharing"><img src="https://drive.google.com/file/d/18DM1VFKzlajCpjC9wvj40tZYJmRRSye2/view?usp=sharing"></a>
</figure>

## 2. 데이터 전처리 과정

{% capture Data_images2 %}
	https://drive.google.com/file/d/1PvH8nNz5rdqnAfbzjNa6jMiUmoSGjGxH/view?usp=sharing
	https://drive.google.com/file/d/1GE0A7vI-OzjCTis2K5-bu-IW5YQ7EkHb/view?usp=sharing
{% endcapture %}
{% include gallery images=Data_images1 cols=3 %}
## 3. 모델

{% capture Model_images2 %}
	https://drive.google.com/file/d/1r8nFmlX1E5e8_TX1ivNR0uYSUwzcb5J4/view?usp=sharing
	https://drive.google.com/file/d/1qNalU09VZ1BFh83KwJNOveY97BRKdaWy/view?usp=sharing
  https://drive.google.com/file/d/1xl7KeEWvd-1AnRGFzuJ3UFgBE1ny01p4/view?usp=sharing
{% endcapture %}
{% include gallery images=Model_images2 cols=3 %}

<center>전체 데이터 셋에서 유튜브 조회수 예측</center>

## 1. 데이터 셋
<figure>
	<a href="https://drive.google.com/file/d/1PeLtTs1x2YU0euNuqNkTQp85Z8rGXtZ1/view?usp=sharing"><img src="https://drive.google.com/file/d/1PeLtTs1x2YU0euNuqNkTQp85Z8rGXtZ1/view?usp=sharing"></a>
</figure>

### 느낀점
간단한 LSTM 모델로 정확한 유튜브 조회수를 예측하기는 힘들지만 나름 성과가 있었고, 
제목이 조회수에 미치는 영향이 크다는것을 이 프로젝트를 하면서 느낄수 있었다.

#### next
이 프로젝트를 되돌아 보며 아쉬운점이 참 많았는데 그 중 2가지를 꼽자면 데이터, 모델 분야라 할수있었다.
##### 데이터
<figure>
	<a href="https://drive.google.com/file/d/1P-dUNqe4HjVb8a95Fgo_jNGYz4NRjpvA/view?usp=sharing"><img src="https://drive.google.com/file/d/1P-dUNqe4HjVb8a95Fgo_jNGYz4NRjpvA/view?usp=sharing"></a>
</figure>

내가 가지고 온 데이터는 캐글에서 크리스마스날을 기준으로 가지고 온 데이터여서 시간에 대해 많은 영향을 끼쳤던것으로 예상이 되었다.
그래서 기존의 데이터에 유튜브 API 를 통한 크롤링한 데이터를 더한다면 시간에 강건성이 있고 데이터의 양이 증가하여 모델 성능에 기여했을것이라 생각된다.

##### 모델분야

당시에는 빠르게 프로젝트를 하는 입장이라 간단한 모델을 적용하였지만 돌이켜 생각해보니 BERT 모델을 써서 모델의 정확도를 높일수 있지 않았을까 하는 생가이 들었다.
또 stopword 로 tokenize 하였지만 모델 결과는 binary classfication 이기 때문에 sentencepiece 를 통해 단어를 추출하였다면 어땟을까 하는 아쉬움도 들었다.

---

재미있게 보셨다면 눌러주세요

<iframe src="https://ghbtns.com/github-btn.html?user=TaylanTatli&repo=Moon&type=star&count=true&size=large" frameborder="0" scrolling="0" width="160px" height="30px"></iframe>
