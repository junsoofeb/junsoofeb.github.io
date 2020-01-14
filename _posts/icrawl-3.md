---
bg: "c.png"
layout: post
title:  "2020-01-15_ffmpeg (동영상에서 이미지 추출하는 툴) 사용법"
crawlertitle: "icrawl usage"
summary: "crawling"
date:   2020-01-15
categories: posts
tags: ['crawling']
author: parkjunsoo
---


## ffmpeg 사용 법
**

#### 1. ffmpeg 설치

동영상에서 이미지파일을 쉽게 뽑아서 사용하기 위해 ffmpeg을 설치한다.

{% highlight js %}  

###### sudo apt-get update  
###### sudo apt-get install ffmpeg

{% endhighlight %}


#### 2. ffmpeg를 사용하여 동영상 크기를 조절하고, 이미지 추출하기


{% highlight js %}  

ffmpeg -i videos/input.mp4 -s 640X480 -c:a copy videos/output.mp4
// videos/input.mp4 파일 전체를 640X480으로 바꿔서 videos/output.mp4으로 저장한다.


ffmpeg -i videos/output.mp4 -vf fps=2 images/img%06d.jpg
// videos/output.mp4 파일을 1초에 2개 프레임 간격으로 images/img000000.jpg형식으로 저장한다

{% endhighlight %}

***

#### 3. 결과


![ff1](https://github.com/junsoofeb/junsoofeb.github.io/raw/master/assets/images/ff1.png)


![ff2](https://github.com/junsoofeb/junsoofeb.github.io/raw/master/assets/images/ff2.png)

![ff3](https://github.com/junsoofeb/junsoofeb.github.io/raw/master/assets/images/ff3.png)
