---
bg: "c.png"
layout: post
title:  "2020-01-10_icrawl (1) 소개 및 사용법"
crawlertitle: "icrawl usage"
summary: "crawling"
date:   2020-01-10
categories: posts
tags: ['crawling']
author: parkjunsoo
---


## icrawler 소개 및 사용법
***


#### 1. 소개

icrawler는 웹 크롤러의 작은 framework이다.  
모듈화 설계로 사용 및 확장이 쉽다.  
이미지 및 비디오, 텍스트 등을 지원한다.

###### google에서 이미지 크롤링하는 데, 가장 쉬운 듯!

***

#### 2. 설치

###### 설치 전, python 3.4+ 버전인지 확인!

{% highlight js %}  

pip install icrawler

또는

conda install -c hellock icrawler

{% endhighlight %}

***

#### 3. 예시 코드

예시 코드는 아래를 참고하였습니다.
<https://pypi.org/project/icrawler/>


###### 내장 크롤러의 사용법은 매우 간단하다.

###### 아래 코드는 구글과 bing에서 '삼다수'을 검색했을때 나오는 이미지를   
###### /home/junsoofeb/cat 으로 크롤링해오는 코드이다.
###### file_idx_offset='auto' 로 줘야 크롤링해온 이미지의 저장이름이 겹치는 것을 막을 수 있다.

{% highlight js %}  

from icrawler.builtin import  BingImageCrawler, GoogleImageCrawler


google_crawler = GoogleImageCrawler(storage={'root_dir': '/home/junsoofeb/PET_bottle'})
google_crawler.crawl(keyword='삼다수', max_num=1000, file_idx_offset='auto')


bing_crawler = BingImageCrawler(downloader_threads=4,
                                storage={'root_dir': '/home/junsoofeb/PET_bottle'})
bing_crawler.crawl(keyword='삼다수',  max_num=1000, file_idx_offset='auto')


{% endhighlight %}


#### 4. 예시 코드 수행 결과


![c1](https://github.com/junsoofeb/junsoofeb.github.io/raw/master/assets/images/c1.png)

![c2](https://github.com/junsoofeb/junsoofeb.github.io/raw/master/assets/images/c2.png)
