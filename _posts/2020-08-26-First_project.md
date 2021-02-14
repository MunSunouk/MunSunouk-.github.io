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
<center><img src="/assets/img/youtube_view/youtube.png"></center>
   
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

 전체 코드
<center>전체 데이터 셋에서 유튜브 조회수 예측</center>
      
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

```
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
import re
import time
import warnings
warnings.filterwarnings('ignore')
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.model_selection import GridSearchCV
from sklearn.linear_model import LinearRegression
from sklearn.linear_model import Lasso
from sklearn.preprocessing import PolynomialFeatures
from sklearn.pipeline import make_pipeline  
from scipy import stats
from sklearn.preprocessing import OneHotEncoder

#데이터 불러오기
us_videos = pd.read_csv(r'C:\Users\injoo\광주 인공지능 사관학교\로테이션\자연어처리&추천시스템 김준태\미니프로젝트\Dataset\cleaned_USvidoes.csv', error_bad_lines=False)
us_videos_json = pd.read_json(r"C:\Users\injoo\광주 인공지능 사관학교\로테이션\자연어처리&추천시스템 김준태\미니프로젝트 데이터\US_category_id.json")

us_videos.head(5)


plt.figure(figsize = (12,6))

plt.subplot(221)
g1 = sns.distplot(us_videos['views'])
g1.set_title("VIEWS DISTRIBUITION", fontsize=16)

plt.subplot(222)
g1 = sns.distplot(us_videos['likes'])
g1.set_title("LIKES DISTRIBUITION", fontsize=16)

plt.subplot(223)
g1 = sns.distplot(us_videos['dislikes'])
g1.set_title("DISLIKES DISTRIBUITION", fontsize=16)

plt.subplot(224)
g1 = sns.distplot(us_videos['comment_total'])
g1.set_title("COMMENTS DISTRIBUITION", fontsize=16)

plt.subplots_adjust(wspace = 0.2, hspace = 0.4,top = 0.9)

plt.show()

us_videos['likes_log'] = np.log(us_videos['likes'] + 1)
us_videos['views_log'] = np.log(us_videos['views'] + 1)
us_videos['dislikes_log'] = np.log(us_videos['dislikes'] + 1)
us_videos['comment_log'] = np.log(us_videos['comment_total'] + 1)

plt.figure(figsize = (12,6))

plt.subplot(221)
g1 = sns.distplot(us_videos['views_log'])
g1.set_title("VIEWS LOG DISTRIBUITION", fontsize=16)

plt.subplot(224)
g2 = sns.distplot(us_videos['likes_log'],color='green')
g2.set_title('LIKES LOG DISTRIBUITION', fontsize=16)

plt.subplot(223)
g3 = sns.distplot(us_videos['dislikes_log'], color='r')
g3.set_title("DISLIKES LOG DISTRIBUITION", fontsize=16)

plt.subplot(222)
g4 = sns.distplot(us_videos['comment_log'])
g4.set_title("COMMENTS LOG DISTRIBUITION", fontsize=16)

plt.subplots_adjust(wspace = 0.2, hspace = 0.4,top = 0.9)

plt.show()


us_videos.columns

us_videos = us_videos.drop(columns=['Unnamed: 0', 'video_id', 'tags', 'views', 'likes', 'dislikes', 'comment_total', 'date'])

# int형인 categoryid를 정수로 바꾸어주었다
us_videos

#  category_id가 뭔지 가져오는 코드
id_title_dict = {}
items_list =  us_videos_json["items"]
for i, key in enumerate(items_list):
    id_title_dict[int(key["id"])] = key["snippet"]["title"]


print(id_title_dict)

us_videos['category_id'] = us_videos['category_id'].apply(lambda x: id_title_dict[x])
us_videos

us_videos = pd.get_dummies(us_videos)
youtube = us_videos


from sklearn.linear_model import Lasso
views=youtube['views_log']
youtube_view2 = youtube[['likes_log','dislikes_log','comment_log']]
train,test,y_train,y_test=train_test_split(youtube_view2,views, test_size=0.2,shuffle=False)
# lasso
model_lasso = Lasso().fit(train, y_train)

print("훈련 세트 점수: {:.2f}".format(model_lasso.score(train, y_train)))
print("테스트 세트 점수: {:.2f}".format(model_lasso.score(test, y_test)))
print("사용한 특성의 수: {}".format(   np.sum( model_lasso.coef_ != 0 )   ))

from sklearn.linear_model import Lasso
columns_without_views = list(youtube.columns.values)
columns_without_views.remove('views_log')

views=youtube['views_log']
youtube_view2 = youtube[columns_without_views]

# print(views)
# print(youtube_view2)
train,test,y_train,y_test=train_test_split(youtube_view2,views, test_size=0.2,shuffle=False)
# lasso
model_lasso = Lasso().fit(train, y_train)

print("훈련 세트 점수: {:.2f}".format(model_lasso.score(train, y_train)))
print("테스트 세트 점수: {:.2f}".format(model_lasso.score(test, y_test)))
print("사용한 특성의 수: {}".format(   np.sum( model_lasso.coef_ != 0 )   ))

views=youtube['views_log']
youtube_view2 = youtube[['likes_log','dislikes_log','comment_log']]

train,test,y_train,y_test=train_test_split(youtube_view2 ,views, test_size=0.2,shuffle=False)
# linearRegression
model = LinearRegression()
model.fit(train, y_train)
print("훈련 세트 점수: {:.2f}".format(model.score(train, y_train)))
print("테스트 세트 점수: {:.2f}".format(model.score(test, y_test)))
print("사용한 특성의 수: {}".format(   np.sum( model.coef_ != 0 )   ))

views=youtube['views_log']
youtube_view2 = youtube[columns_without_views]

train,test,y_train,y_test=train_test_split(youtube_view2 ,views, test_size=0.2,shuffle=False)
# linearRegression
model = LinearRegression()
model.fit(train, y_train)
print("훈련 세트 점수: {:.2f}".format(model.score(train, y_train)))
print("테스트 세트 점수: {:.2f}".format(model.score(test, y_test)))
print("사용한 특성의 수: {}".format(   np.sum( model.coef_ != 0 )   ))

from sklearn.ensemble import RandomForestRegressor

views=youtube['views_log']
youtube_view2 = youtube[['likes_log','dislikes_log','comment_log']]
train,test,y_train,y_test=train_test_split(youtube_view2 ,views, test_size=0.2,shuffle=False)

RF = RandomForestRegressor()
RF.fit(train,y_train)
print("훈련 세트 점수: {:.2f}".format(RF.score(train, y_train)))
print("테스트 세트 점수: {:.2f}".format(RF.score(test, y_test)))

columns_without_views = list(youtube.columns.values)
columns_without_views.remove('views_log')

views=youtube['views_log']
youtube_view2 = youtube[columns_without_views]
train,test,y_train,y_test=train_test_split(youtube_view2 ,views, test_size=0.2,shuffle=False)

RF = RandomForestRegressor()
RF.fit(train,y_train)
print("훈련 세트 점수: {:.2f}".format(RF.score(train, y_train)))
print("테스트 세트 점수: {:.2f}".format(RF.score(test, y_test)))

youtube.to_csv(r'./\one_hot_encoded.csv', )

```

```
from scipy import stats

from collections import Counter
from nltk.tokenize import RegexpTokenizer
from stop_words import get_stop_words
import re
import nltk
from nltk.corpus import stopwords
from nltk import sent_tokenize, word_tokenize
from tensorflow.keras.preprocessing.text import Tokenizer
from tensorflow.keras.preprocessing.sequence import pad_sequences


youtube= pd.read_csv(r'C:\Users\injoo\광주 인공지능 사관학교\로테이션\자연어처리&추천시스템 김준태\미니프로젝트\Dataset\USvideos.csv', error_bad_lines=False)
youtube = youtube.drop_duplicates(['video_id'], keep='first')

only_title = youtube['title']
title_data = only_title

title_data = title_data.str.replace("[^\w]", " ")
#혹시나 공백이 있으면
title_data= title_data.replace('', np.nan)

#결측치 있으면 모두 제거
title_data = title_data.dropna(how='any')

youtube['views_label'] = np.log(youtube['views'] + 1)

# youtube['views_label'] = youtube['views']

# def label_func(x):
#     if x>=17.69:
#         return 1
#     elif x>=12.50:
#         return 2
#     elif x>=11.33:
#         return 3
#     else:
#         return 4

def label_func(x):
    if x>=12.5:
        return 1
    else:
        return 0
    
youtube['views_label'] = youtube['views_label'].apply(lambda x: label_func(x))


from nltk.corpus import stopwords 
from nltk.tokenize import word_tokenize 

stop_words = set(stopwords.words('english')) 
# stop_words = set(stop_words.words('english'))

X_train = []
for stc in train:
    token = []
    words = stc.split()
    for word in words:
        if word not in stop_words:
            token.append(word)
    X_train.append(token)

X_test = []
for stc in test:
    token = []
    words = stc.split()
    for word in words:
        if word not in stop_words:
            token.append(word)
    X_test.append(token)

train , test, y_train, y_test = train_test_split(title_data, youtube['views_label'], test_size=0.2, shuffle=False)

tokenizer = Tokenizer(7792)
tokenizer.fit_on_texts(X_train)

X_train = tokenizer.texts_to_sequences(train)
X_test = tokenizer.texts_to_sequences(test)

from tensorflow.keras.preprocessing.sequence import pad_sequences

max_len = 14
X_train = pad_sequences(X_train, maxlen=max_len)
X_test = pad_sequences(X_test, maxlen=max_len)

from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, SimpleRNN, Embedding, LSTM
model = Sequential()
model.add(Embedding(5000, 32))
model.add(LSTM(32))
model.add(Dense(1, activation='relu'))

model.compile(loss='binary_crossentropy', optimizer='adam', metrics= 'accuracy' )
# model.compile(loss='categorical_crossentropy' ,optimizer='adam' ,metrics= 'accuracy' )
model.fit(X_train, y_train, epochs=10)

sentence = input()
# 토큰화
token_stc = sentence.split()
# 정수 인코딩
encode_stc = tokenizer.texts_to_sequences([token_stc])
# 패딩
pad_stc = pad_sequences(encode_stc, maxlen = 14)

score = model.predict(pad_stc)
print(score)

sentence = input()
# 토큰화
token_stc = sentence.split()
# 정수 인코딩
encode_stc = tokenizer.texts_to_sequences([token_stc])
# 패딩
pad_stc = pad_sequences(encode_stc, maxlen = 14)

score = model.predict(pad_stc)
print(score)

```


재미있게 보셨다면 눌러주세요

<iframe src="https://ghbtns.com/github-btn.html?user=MunSunouk&repo=MunSunouk.github.io&type=star&count=true&size=large" frameborder="0" scrolling="0" width="160px" height="30px"></iframe>
