---
layout: post
title:  "기본적인 텐서보드 사용법"
crawlertitle: "tensorboard"
summary: "텐서보드"
date:   2019-01-22
categories: posts
tags: 'TENSORBOARD'
author: parkjunsoo
---

#### 텐서보드(tensorboard)  

텐서플로우로 만든 그래프를 시각적으로 보여주는 툴이다.  
텐서보드는 텐서플로우가 설치될 때 같이 설치된다.  


#### 기초적인 사용법

##### 1. writer = tf.summary.FileWriter('./my_graph', sess.graph)   

 _F,W가 대문자이고, 첫 번째 파라미터는 문자열 형태임을 주의하자.  
 ./my_graph은 my_graph라는 이름의 디렉토리를 만들고,  
 그 안에 그래프에 대한 설명(description)을 저장하라는 의미이다._   
 
 _sess.graph는 Session의 graph속성이다.  
 tf.Session()객체는 그래프의 레퍼런스인 graph속성을 가진다.  
 이 정보를 인자로 받아서 my_graph디렉토리에 있는 그래프의 설명을 출력한다._  
 
                     
   
##### 2. 터미널 창에서 tensorboard --logdir my_graph 입력  
##### 3. 인터넷 브라우저 실행 후 http://localhost:6006 입력  


#### 사용 예시  

{% highlight ruby %}
 
a = tf.constant(5, name='input_a')  
b = tf.constant(3, name='input_b')  
c = tf.add(a,b, name='add_c')  
d = tf.subtract(a,b, name='sub_d')   
e = tf.multiply(a,b, name='mul_e')   
f = tf.div(a,b, name='div_f')  
g = tf.truediv(a,b, name='truediv_g')   

sess = tf.Session()  

##### 텐서보드 사용 #####

writer = tf.summary.FileWriter('./my_graph', sess.graph)  

##### 터미널 창에서 tensorboard --logdir my_graph 입력

Starting TensorBoard on port 6006 ~~~~~~~ 라는 메시지가 나온다.

##### 인터넷 브라우저 실행 후 http://localhost:6006 입력  

페이지 상단에 graphs클릭하면 시각화된 그래프가 나온다.


{% endhighlight %}
