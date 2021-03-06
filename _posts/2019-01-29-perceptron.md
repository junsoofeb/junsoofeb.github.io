---
bg: "deep.jpg"
layout: post
title:  "1장, 퍼셉트론"
crawlertitle: "perceptron"
summary: "perceptron"
date:   2019-01-29
categories: posts
tags: 'deep_learning'
author: parkjunsoo
---
#### 단층/다층 퍼셉트론

퍼셉트론은 층의 개수에 따라서 단층(single-layer) 퍼셉트론과 다층(multi-layer) 퍼셉트론으로 구분할 수 있다.  

####_이 글에서는 단층 퍼셉트론을 퍼셉트론으로, 다층 퍼셉트론을 (인공)신경망으로 부른다._  

***

#### 퍼셉트론(perceptron)

퍼셉트론 알고리즘은 신경망(딥러닝)의 기원이 되는 알고리즘이다.  
퍼셉트론의 구조를 배우는 것은 신경망과 딥러닝에 큰 도움이 된다.  

퍼셉트론은 다수의 신호를 입력으로 받아 하나의 신호를 출력한다.
여기서의 신호는 전류나 강물처럼 _흐름_ 이 있는 것을 생각하면 좋다.

다만, 실제 전류와 달리 퍼셉트론 신호는 [흐른다/안 흐른다]의 두 가지 값을 가질 수 있다.  
일반적으로 1을 신호가 흐른다. 0을 신호가 흐르지 않는다라는 의미로 사용한다.

***

![perceptron](https://github.com/junsoofeb/junsoofeb.github.io/raw/master/assets/images/perceptron.png)

이렇게 입력이 2개인 퍼셉트론을 생각해보자.

x1과 x2는 입력 신호, y는 출력 신호, w1과 w2는 가중치를 뜻한다.(weight의 w)

그림의 원을 _뉴런_  또는 _노드_ 라고 한다.  

입력 신호가 뉴런에 보내질 때에는 (w1x1, w2x2) 이렇게 각각 고유한 가중치가 곱해진다.    

뉴런에서 보내온 신호의 총합이 정해진 한계를 넘어설 때만 1을 출력한다.(이를 뉴런이 활성화한다 라고 표현하기도 한다.)

일반적으로 그 한계를 _임계값_ 이라고 하며 θ기호(세타)로 나타낸다.

지금까지를 수식으로 나타내면,

y = 0 (w1x1 + w2x2 <= θ)  
y = 1 (w1x1 + w2x2 > θ)

퍼셉트론은 복수의 입력 신호 각각에 고유한 가중치를 부여한다.  
가중치가 클수록 그 신호가 그만큼 더 중요함을 의미한다.  

***

#### 단순한 논리회로

###### AND 게이트

![perceptron](https://github.com/junsoofeb/junsoofeb.github.io/raw/master/assets/images/AND.png)

AND게이트를 퍼셉트론으로 표현해보자.  
이를 위해서 위의 진리표대로 작동하는 w1,w2, θ의 값을 정해보자.

x1과 x2 둘 다 1일 때만 가중치와 입력신호의 합이 주어진 임계값을 넘어야 한다.


사실 AND게이트를 만족하는 매개변수 조합은 무수히 많다.  
가령 (w1,w2,θ)가 (0.5, 0.5, 0.7) 또는 (0.5, 0.5, 0.8) 등등

###### NAND 게이트와 OR게이트도 (w1,w2,θ)의 값만 조절하면 같은 방식으로 표현할 수 있다.

###### NAND는 NOT AND이므로, AND 게이트를 만족하는 매개변수의 부호만 반전시키면 NAND 게이트를 만족한다.

###### 여기서 퍼셉트론의 매개변수 값을 정하는 것은 컴퓨터가 아닌 우리들이다.

###### 기계 학습에서는 이 매개변수의 값을 정하는 작업을 컴퓨터가 자동으로 하도록 한다.

###### 학습이란 적절한 매개변수 값을 정하는 작업이다.

***

#### 퍼셉트론 구현

###### python으로 AND게이트 구현


{% highlight js %}

'''python  
def AND(x1, x2):  
    w1, w2, theta = 0.5, 0.5, 0.7  
    tmp = x1,w1 + x2*w2   
    if tmp <= theta:  
        return 0  
    elif tmp > theta:  
        return 1  
'''      

{% endhighlight %}

***

#### 가중치와 편향 도입

앞에서 구현한 AND게이트는 직관적이고 쉽지만, 앞으로를 생각해서 다른 방식으로 수정하는것이 좋다.

y = 0 (w1x1 + w2x2 <= θ)  
y = 1 (w1x1 + w2x2 > θ)

이 식에서 θ를 -b로 치환하면,

y = 0 (b + w1x1 + w2x2 <= 0)  
y = 1 (b + w1x1 + w2x2 > 0)

이 된다. b는 bias(편향)이라 한다.  

이제 다시 식을 해석하면,

###### 퍼셉트론은 입력신호에 가중치를 곱한 값과 편향을 합하여, 그 값이 0을 넘으면 1을 출력하고, 그렇지 않으면 0을 출력한다.


###### 가중치와 편향 구현


{% highlight js %}

'''python  
def AND(x1, x2):  
    x = np.array([x1, x2])  
    w = np.array([0.5, 0.5])  
    b = -0.7  
    tmp = np.sum(w*x) + b  
    if tmp <= 0:  
        return 0  
    else:  
        return 1  
'''      

{% endhighlight %}


##### 가중치(w)는 입력 신호가 결과에 주는 영향력(중요도)을 조절하는 매개변수이고,  
##### 편향(b)은 뉴런이 얼마나 쉽게 활성화(결과로 1을 출력)하느냐를 조절하는 매개변수이다.

***
#### 선형과 비선형

선형 : 직선 하나로 (영역 등을) 표현할 수 있는 형태.

비 선형 : 직선 하나로는 표현할 수 없는 형태. ex) 곡선 등

단층 퍼셉트론은 직선형 영역만 표현할 수 있고, 다층 퍼셉트론은 선형, 비선형 영역 모두 표현할 수 있다.


***

#### 퍼셉트론의 한계

퍼셉트론으로 AND, NAND, OR 3가지 논리 회로를 구현할 수 있지만,

XOR게이트를 표현할 수 없다.

퍼셉트론은 _직선_ 으로 나뉜 두 영역을 만든다.  
직선으로 나뉜 한쪽 영역은 1, 다른 한쪽은 0을 출력한다.

그러나 XOR게이트는 직선 하나로는 1과 0의 구분을 할 수 없다.

그러나 직선이라는 제약을 없앤다면 가능하다.

이러한 다층 퍼셉트론(multi-layer perceptron)개념으로 XOR게이트를 만들 수 있다.

[다층 퍼셉트론의 동작방식]

3층 (0, 1, 2층) 퍼셉트론일 때,

1. 0층의 두 뉴런이 입력 신호를 받아 1층의 뉴런으로 신호를 보낸다.

2. 1층의 뉴런이 2층의 뉴런으로 신호를 보내고, 2층의 뉴런은 그 신호를 바탕으로 출력한다.


퍼셉트론은 층을 거듭 쌓으면 비선형적인 표현도 가능하고, 이론상 컴퓨터가 수행하는 처리도 모두 표현할 수 있다.

***
