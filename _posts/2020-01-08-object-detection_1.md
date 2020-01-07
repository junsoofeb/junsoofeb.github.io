---
bg: "tf.jpg"
layout: post
title:  "Tensorflow Object Detection API (1) 소개 및 설치"
crawlertitle: "Tensorflow Object Detection API"
summary: "Object Detection"
date:   2020-01-08
categories: posts
tags: ['Object_Detection']
author: parkjunsoo
---


## Tensorflow Object Detection API (1) 소개 및 설치 방법
***

###### 아래 링크를 바탕으로 작성
###### <https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/installation.md>

#### 1. 소개

이미지에서 여러 개체를 지역화하고, 식별할 수 있는 정확한 기계 학습 모델을 만드는 것은 컴퓨터 비전의 핵심 과제이다.  
TensorFlow Object Detection API는 TensorFlow를 기반으로 구축 된 오픈 소스 프레임 워크로, 객체 탐지 ​​모델을 쉽게 구축, 교육 및 배포 할 수 있다.  

#### 2. 설치 방법 (우분투 18.04에서 아나콘다 가상환경에 설치한다고 가정)

###### [ ] 안의 내용은 선택적!
###### python과 tensorflow 버전에 주의!! 다르게 설치하면 에러 발생할 수 있음!


1) 먼저  Tensorflow Object Detection을 설치할 가상환경을 생성한다.
{% highlight js %}  

###### conda create -n tf python=3.6  
(tf라는 이름의 가상환경을 python3.6버전으로 생성)

{% endhighlight %}

2) 가상환경으로 이동하고 필요한 라이브러리를 설치한다.

{% highlight js %}  

###### conda activate tf

{% endhighlight %}

3) models를 git clone을 통해 받아온다.  
   pip install tensorflow로는 models가 포함되지 않기 때문!

{% highlight js %}  

###### git clone https://github.com/tensorflow/models [원하는 경로]

{% endhighlight %}

4) 필요한 라이브러리를 설치한다.

{% highlight js %}  

###### pip install tensorflow==1.9
###### sudo apt-get install protobuf-compiler python-pil python-lxml python-tk
###### pip install Cython
###### pip install contextlib2
###### pip install jupyter
###### pip install matplotlib
{% endhighlight %}

5) git clone으로 받아온 models에서 research 디렉토리로 이동 후,

{% highlight js %}  

###### protoc object_detection/protos/\*.proto --python_out=.
{% endhighlight %}

6) 5번과 마찬가지로 models/research에서

{% highlight js %}  

###### export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim

###### 6번 내용은 터미널을 열 때마다 수행해야 하는데,  
###### 귀찮으니 .bashrc의 맨 아래에 적어주면 좋다.  
###### 이 때 pwd는 models/research의 절대경로로 바꿔서 적는다.  
###### ex) 'home/junsoofeb/models/research' 가 'pwd'를 대체!

{% endhighlight %}

7) 제대로 깔렸는지, models/research에서 설치 확인  

{% highlight js %}  

###### python object_detection/builders/model_builder_test.py

{% endhighlight %}

이렇게 나오면 정상적으로 설치 완료!

![result](https://github.com/junsoofeb/junsoofeb.github.io/raw/master/assets/images/result.png)
