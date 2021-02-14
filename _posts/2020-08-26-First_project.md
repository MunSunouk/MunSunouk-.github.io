---
layout: post
title:  "Moon Jekyll Theme"
date:   2020-08-26
excerpt: "Minimal, one column Jekyll theme for your blog."
project: true
comments: true
---

    
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

The description to show on your homepage.

#### description

The description to use for meta tags and navigation menu.

#### url

Used to generate absolute urls in `sitemap.xml`, `feed.xml`, and for generating canonical URLs in `<head>`. When developing locally either comment this out or use something like `http://localhost:4000` so all assets load properly. *Don't include a trailing `/`*.

Examples:

{% highlight yaml %}
url: http://taylantatli.me/Moon
url: http://localhost:4000
url: //cooldude.github.io
url:
{% endhighlight %}


#### logo
Your site's logo. It will show on homepage and navigation menu. Also used for twitter meta tags.

#### background
Image for background. If you don't set it, color will be used as a background.

#### Google Analytics and Webmaster Tools

Google Analytics UA and Webmaster Tool verification tags can be entered in `_config.yml`. For more information on obtaining these meta tags check [Google Webmaster Tools](http://support.google.com/webmasters/bin/answer.py?hl=en&answer=35179) and [Bing Webmaster Tools](https://ssl.bing.com/webmaster/configure/verify/ownership) support.

#### MathJax
It's enabled. But if you don't want to use it. Set it false in  `_config.yml`.

#### Disqus Comments
Set your disqus shortname in `_config.yml` to use comments.

---

### Navigation Links

To set what links appear in the top navigation edit `_data/navigation.yml`. Use the following format to set the URL and title for as many links as you'd like. *External links will open in a new window.*

{% highlight yaml %}
- title: Home
  url: /

- title: Blog
  url: /blog/

- title: Projects
  url: /projects/

- title: About
  url: /about/

- title: Moon
  url: http://taylantatli.me/Moon
{% endhighlight %}

---

## Layouts and Content

Moon Theme use [Jekyll Compress](https://github.com/penibelst/jekyll-compress-html) to compress html output. But it can cause errors if you use "linenos" (line numbers). I suggest don't use line numbers for codes, because it won't look good with this theme, also i didn't give a proper style for them. If you insist to use line numbers, just remove `layout: compress` string from layouts. It will disable compressing.

### Feature Image

You can set feature image per post. Just add `feature: some link` to your post's front matter.

```
feature: /assets/img/some-image.png
or
feaure: http://example.com/some-image.png
```    
 This also will be used for twitter card:

![Moon Twitter Card](https://cloud.githubusercontent.com/assets/754514/14509719/61c5751c-01d6-11e6-8c29-ce8ccad149bf.png)




### Comments
To show disqus comments for your post add `comments: true` to your post's front matter.

---

## Questions?

Found a bug or aren't quite sure how something works? By all means [file a GitHub Issue](https://github.com/TaylanTatli/Moon/issues/new). And if you make something cool with this theme feel free to let me know.

---

## License

This theme is free and open source software, distributed under the MIT License. So feel free to use this Jekyll theme on your site without linking back to me or including a disclaimer.
