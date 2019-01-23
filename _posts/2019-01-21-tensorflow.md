---
bg: "tf.jpg"
layout: post
title:  "텐서플로우 기본"
crawlertitle: "Tensorflow_basic"
summary: "텐서플로우 "
date:   2019-01-21 
categories: posts
tags: ['Tensorflow']
author: parkjunsoo
---

  

#### ++계산 그래프 소개++


***

#### 계산 그래프(computation graph)

모든 텐서플로우 프로그램에는 계산 그래프가 있다.  
계산 그래프는 계산 구조를 정의하기 위해 사용되는 특별한 형태의 방향성 그래프(directed graph)이다.  

노드(nodes) : 일반적으로 원, 박스 등으로 표현하며 그래프 안에서 데이터에 적용될 _계산_ 또는 _함수_ 를 의미한다.  
엣지(edges) : 일반적으로 화살표로 표시하며 노드들 사이에 전달되는 _실제 데이터_ 를 의미한다.  
***
#### 의존성(Dependencies)  

_노드 A가 노드 B의 계산 결과를 필요로 할때, 노드 A는 노드 B에 의존한다_ 라고 한다.

반대로, _노드들이 서로 어느 정보도 필요로 하지 않을때, 독립적이다_ 라고 한다.

##### 의존성의 전이성

_노드 A가 노드 B에 의존하고, 노드 B가 노드 C에 의존하면, 노드 A는 노드 C에 의존한다._

***
#### 텐서플로우 동작 메커니즘

1. 계산 그래프 정의한다.   
2. 그래프를 실행한다.  
3. 실행 결과로 그래프 속의 값들이 업데이트 또는 어떠한 값을 반환한다.  
    
    
    
    
 ***
#### 텐서플로우 그래프 코드


{% highlight js %}

import tensorflow as tf #텐서플로우 라이브러리 임포트하고 이름을 tf라고 한다.  

a = tf.constant(5, name='input_a')  
b = tf.constant(3, name='input_b')  

#tf.constant() 상수 생성하는 Operation // Op라고 한다.  
#하나의 텐서 값을 받아서 연결된 노드에 같은 값을 출력한다.   

c = tf.add(a,b, name='add_c')  #두 개의 입력을 받아서 더한다.  
d = tf.subtract(a,b, name='sub_d')  #두 개의 입력을 받아서 뺀다.  
e = tf.multiply(a,b, name='mul_e')  #두 개의 입력을 받아서 곱한다.  
f = tf.div(a,b, name='div_f')  
#정수형 텐서인 경우 원소간 정수 나눗셈, 실수형 텐서인 경우 원소간 실수 나눗셈  
g = tf.truediv(a,b, name='truediv_g') #요소간 실수 나눗셈  

=====================#여기까지가 그래프의 정의

sess = tf.Session()  #그래프의 실행은 sess.run(op) 이용한다.  
print('a =',sess.run(a),'  b =',sess.run(b),'\n')  
print('a + b =',sess.run(c))      
print('a - b =',sess.run(d))  
print('a * b =',sess.run(e))  
print('a % b =',sess.run(f))    
print('a % b =',sess.run(g))  


### 실행 결과 ###  

a = 5   b = 3  

a + b = 8   
a - b = 2  
a * b = 15  
a % b = 1  
a % b = 1.6666666666666667  
{% endhighlight %}


***


#### 텐서는 무엇인가?

텐서(tensor)는 단순하게 매트릭스(matrix)의 n-차원 추상화이다.   

##### 0-차원 텐서 : 스칼라(scalar)

##### 1-차원 텐서 : 벡터(vector)

##### 2-차원 텐서 : 행렬(matrix)

##### 3-차원 이상 : 텐서(tensor)  

***
#### 텐서플로우에서 텐서의 표현  

텐서플로우는 파이썬의 숫자, 불리언, 문자열, 이들의 리스트 모두를 사용할 수 있다.  
텐서플로우는 리스트를 이용(중첩)하여 모든 차원의 텐서로 변환한다.  

###### 파이썬 자료형을 사용해서 텐서 객체를 지정하는 것은 쉽고 빠르지만,  
###### 내가 사용하려는 자료형을 명확하게 표현하기에는 부족하다는 단점이 있다.  
###### 복소수와 같은 자료형을 예로 들 수 있다.  

##### 이러한 이유로 텐서 객체를 Numpy배열로 생각하는 것이 일반적이다.

{% highlight js %}

#스칼라는 값으로 표현  
t_0 = np.array(50,dtype=np.int32)   


#벡터는 리스트로 표현    
#문자열 배열에는 dtype(데이터 타입) 명시하면 안된다.  
t_1 = np.array(["apple", "banana", "peach" ])   


 #행렬은 리스트의 중첩으로 표현  
t_2 = np.array([ [True, False, False], [False, False, True], [False, True, False] ], dtype=np.bool)  
        
        
#텐서는 리스트의 다중 중첩으로 표현          
t_3 = np.array([ [[2],[4],[6]], [[8],[10],[12]], [[14],[16],[18]] ], dtype=np.int64)  

ㆍㆍㆍ

###### 문자열을 제외한 나머지 경우에 dtype을 명시하지 않아도 되지만,   
###### 어떤 객체가 TypeMismatchError를 발생하는지 쉽게 찾을 수 있도록   
###### 텐서 객체가 가질 숫자형에 대해서는 명확하게 정의하는 것이 좋다.  
        
{% endhighlight %}

***

####  Rank와 Shape

Rank는 텐서의 차원을 의미한다.
###### 여는 대괄호( '\[' )의 개수와 일치한다. 

Shape는 단어 그대로 모양을 의미한다.
###### 텐서의 각 차원에 원소가 몇 개씩 들어있는지를 표현한다. 


{% highlight js %}

Shape                         |       Rank       |    Dimension-number(차원 번호)    
[]                            |         0        |          0-D 
[D0]                          |         1        |          1-D 
[D0,D1]                       |         2        |          2-D  
[D0,D1,D2]                    |         3        |          3-D  
ㆍㆍㆍ                        |        ㆍㆍㆍ     |         ㆍㆍㆍ  
[D0,D1,D2,ㆍㆍㆍ,Dn-1]        |          n       |           n-D  


{% endhighlight %}

***

#### 스칼라가 아닌 텐서(벡터)를 입력받는 예제

{% highlight js %}

import tensorflow as tf

a = tf.constant([5,3,2])
#이전과 달리 스칼라 숫자(값)가 아닌 텐서를 입력받는 예제이다.
#하나의 노드에 1차원 텐서(값의 리스트,벡터)를 입력받았다.

#벡터나 행렬의 원소들을 연산하여 
#스칼라 값을 구하는 연산은 tf.reduce_명령 사용한다.

b = tf.reduce_sum(a)
#설정한 차원에 따라 원소들을 더한다.
    
c = tf.reduce_prod(a)
#설정한 차원에 따라 원소들을 곱한다.

d = tf.reduce_max(a)
#설정한 차원에 따라 원소들 중에서 최댓값 선택.

e = tf.reduce_min(a)
#설정한 차원에 따라 원소들 중에서 최솟값 선택.

f = tf.reduce_mean(a)
#설정한 차원에 따라 원소들을 평균낸다.

#그 밖에 아래와 같은 명령 등이 있다.  
#tf.reduce_all 설정한 축으로 이동하면서 and 논리 연산을 수행한다.  
#tf.reduce_any  설정한 축으로 이동하면서 or논리 연산을 수행한다.  

sess = tf.Session() #그래프 실행을 위함  

print('원소들의 합',sess.run(b))  
print('원소들의 곱',sess.run(c))  
print('원소들 중 최댓값',sess.run(d))  
print('원소들 중 최솟값',sess.run(e))  
print('원소들의 평균',sess.run(f))  

##### 실행결과 #####

원소들의 합 10  
원소들의 곱 30  
원소들 중 최댓값 5  
원소들 중 최솟값 2  
원소들의 평균 3  

#평균을 보면 10 / 3 = 3.333334 이지만  
#[5,3,2]가 모두 정수형이라서 정수만 반환.


{% endhighlight %}


***

#### 텐서플로우 오퍼레이션(TensorFlow Operations,Ops)

텐서플로우 오퍼레이션은 텐서 객체에 (또는 텐서 객체를 사용하여) 계산을 수행하는 _노드_ 이다.  
##### 텐서플로우 오퍼레이션은 계산 후의 결과로서 다른 Ops에서 사용될 텐서를 0개 이상을 반환한다.  
##### 모든 노드들이 다른 노드들과 꼭 연결될 필요는 없다는 점을 기억하자.

오퍼레이션을 생성하기 위해서 파이썬에서 생성자를 호출하고,  
파이썬 생성자는 오퍼레이션의 출력(0개 이상의 텐서 객체)을 다룰 수 있는 핸들(handle)을 반환한다.  

##### 텐서플로우 오퍼레이션 목록  
{% highlight js %}

tf.add: 덧셈  
tf.subtract: 뺄셈  
tf.multiply: 곱셈  
tf.div: 정수형 텐서인 경우 정수, 실수면 실수 나눗셈    
tf.truediv: 실수 나눗셈(정수 포함)
tf.pow: n-제곱(지수승)  
tf.negative: 음수 부호  

등등  
***

tf.reduce_any: 설정한 축으로 이동하면서 or논리 연산을 수행한다.  
tf.reduce_mean: 설정한 축의 평균을 구한다.  
tf.reduce_max: 설정한 축의 최댓값을 구한다.  
tf.reduce_min: 설정한 축의 최솟값을 구한다.  
tf.reduce_prod: 설정한 축의 요소를 모두 곱한 값을 구한다.  
tf.reduce_sum: 설정한 축의 요소를 모두 더한 값을 구한다.  

***
tf.abs: 절대값  
tf.sign: 부호  
tf.round: 반올림  
tf.ceil: 올림  
tf.floor: 내림  
tf.square: 제곱  
tf.sqrt: 제곱근  
tf.maximum: 두 텐서의 각 원소에서 최댓값만 반환.  
tf.minimum: 두 텐서의 각 원소에서 최솟값만 반환.  
tf.cumsum: 누적합  
tf.cumprod: 누적곱  

***

tf.matmul: 내적  
tf.matrix_inverse: 역행렬  

***
다음은 벡터와 행렬의 크기 및 차원을 바꾸는 Ops이다.  

tf.reshape: 벡터 행렬의 크기 변환  
tf.transpose: 전치 연산  
tf.expand_dims: 지정한 축으로 차원을 추가  
tf.squeeze: 벡터로 차원을 축소  

***
벡터/행렬을 나누거나 두 개 이상의 벡터/행렬을 합치는 Ops도 많이 사용된다.  

tf.slice: 특정 부분을 추출  
tf.split: 분할  
tf.concat: 합치기  
tf.tile: 복제-붙이기  
tf.stack: 합성  
tf.unstack: 분리  

***
nn 서브패키지에는 신경망에서 쓰이는 함수들도 구현되어 있다.  

tf.nn.sigmoid: 로지스틱 함수  
tf.nn.softplus: 소프트플러스 함수  
tf.nn.softsign: 소프트사인 함수  

***
등등  

{% endhighlight %}

***

#### 텐서플로우 그래프

g = tf.Graph() #G가 대문자임을 주의  

위의 코드는 그래프를 생성한다. 그래프가 생성되면 Graph.as_default()메소드를 이용해서  
오퍼레이션과 텐서 등을 추가할 수 있다.  


{% highlight js %}

with g.as_default():  
    a = tf.add(2,3)
    
{% endhighlight %}
이러한 방식이다.  


##### 텐서플로우는 라이브러리가 로딩될 때 자동으로 Graph를 생성하고 이를 디폴트로 할당한다.  
그렇기 때문에 Graph.as_default()메소드 외부에 정의된 내용은 자동으로 디폴트 그래프에 위치된다.  

***

#### 텐서플로우 세션(TensorFlow session)

세션(session)은 그래프의 실행을 책임진다.  
세션은 3개의 선택적인 파라미터(target, graph, config)를 가진다.  
보통 세션 객체는 디폴트 생성자로 생성된다.  

##### 세션이 생성되면 텐서를 계산하기 위한 run()메소드를 사용할 수 있다.  
***
#### FETCHES // session.run의 필수 파라미터

fetches는 Ops든 텐서든 어떠한 그래프 구성요소든지 받아들일 수 있다.  

a = tf.add(2, 5)  
b = tf.multiply(a, 3)  
sess = tf.Session()  
sess.run(\[a, b\]) #이와 같이 그래프 항목들의 리스트도 가능하다.

***
#### FEED 딕셔너리(dictionary) // session.run의 선택적 파라미터

feed_dict는 그래프에서 텐서 값을 override할 때 사용되며, 파이썬 딕셔너리 객체를 입력으로 받는다.  

{% highlight js %}

a = tf.add(2, 5)  
b = tf.multiply(a, 3)  
sess = tf.Session()  

replace_dict = { a : 15 } #위에서 a의 값이 7이 되지만, 15로 대체한다.   
sess.run(b, feed_dict=replace_dict) #실행 결과 45가 반환된다.  

{% endhighlight %}


***
#### 플레이스홀더(placeholder)로 입력 사용하기  

사용자로부터 값을 입력받기 위해 placeholder를 이용한다.  
플레이스홀더는 텐서 객체처럼 사용되지만, 값을 가지지 않는다.  
_대신 나중에 입력될 텐서를 위한 자리(place)를 지니고 있다(hold)._  

##### tf.placeholder는 dtype파라미터가 필수이고, shape파라미터는 선택적이다.

##### 플레이스홀더에 값을 전달하기 위해서 sess.run()에서 feed_dict파라미터를 사용한다.  


{% highlight js %}

import tensorflow as tf  
import numpy as np  

a = tf.placeholder(tf.int32, shape=[2])  
b = tf.reduce_prod(a)  
c = tf.reduce_sum(a)  
d = tf.sum(b,c) #tf.redece_sum([b,c])도 가능  

sess = tf.Session()

input_dict = { a : np.array([5, 3]}   
#플레이스홀더의 핸들(a)이 딕셔너리의 key로 사용되고,
#우리가 전달하려는 텐서 객체(np.array([5, 3]))가 딕셔너리의 value로 사용된다.

sess.run(d, feed_dict=input_dict)  


{% endhighlight %}
***

#### 변수

tf에서의 변수는 일반적인 프로그래밍 언어에서의 변수와는 조금 다르다.  
우리가 아닌 tf가 사용하는 변수라고 보면 된다. tf가 자체적으로 변경시킨다.  

##### 변수 생성

텐서와 오퍼레이션 객체는 변경할 수 없다. 
머신 러닝은 속성상 시간에 따라 변경에서는 변경되는 값을 저장할 필요가 있는데,  
tf에서는 여러 번의 Session.run() 호출 동안 유지되면서 변경 가능한 텐서 값을 보관하기 위해서 변수를 사용한다.  

##### tf.Variable()생성자를 사용해서 변수 객체를 생성할 수 있다.

{% highlight js %}

import tensorflow as tf  

my_var = tf.Variable(3)  
add = tf.add(5, my_var)  
mul = tf.multiply(8, my_var)  

{% endhighlight %}

변수의 초기값은 보통 0, 1 또는 랜덤값들로 채워진 큰 텐서들(large tensors)이다.  
이러한 값들을 쉽게 생성할 수 있도록 다양한 Ops가 마련되어 있다.

{%  highlight js %}

tf.zeros()  
tf.ones()  
tf.random_normal() #정규분포형태의 난수 텐서 생성  
tf.truncated_normal() #정규분포형태의 난수 텐서 생성하는데, 표준편차 2배 이상인 값은 제외한다.  
tf.random_uniform() #균등분포형태의 난수 텐서 생성  

이들은 텐서의 shape 파라미터를 받는다.  

zeros = tf.zeros([2,2])  
#2X2의 0행렬 생성

ones = tf.ones([6])  
#길이가 6인 1로 채워진 벡터 생성  


ㆍㆍㆍ


{% endhighlight %}

***

#### 변수 초기화

변수의 상태는 Session에 의해서 관리된다.  
이 때문에 변수를 사용하면 Session 내부에서 변수들을 초기화하는 과정이 추가로 필요하다.  
변수를 초기화하면 Session이 그 변수의 현재의 값들을 추적하기 시작한다.  

##### 보통 tf.global_variables_initializer() 또는 tf.variables_initializer()를 Session.run()에 전달하는 방식이다.  

{% highlight js %}

import tensorflow as tf  

init = tf.global_variables_initializer()  
sess = tf.Session()  
sess.run(init)

{% endhighlight %}

***

#### 변수값 변경

변수의 값을 변경하기 위해서 Variable.assign() 메소드를 사용하여 변수에 새로운 값을 부여한다.  
이 메소드는 Session에서 실행되어야 한다.  

변수의 값을 편하게 증가,감소시키기 위해서 Variable.assign_add()와 Variable.assign_sub()메소드를 사용하면 편리하다.  

{% highlight js %}

import tensorflow as tf  

my_var = tf.Variable(1)

my_var_times_two = my_var.assign(my_var * 2)  
#현재의 변수의 값에서 X2한 값을 부여한다.  
#my_var_times_two가 sess.run에서 실행될 때 마다 값이 바뀐다.  

init = tf.global_variables_initializer()  
sess = tf.Session()  
sess.run(init)  

sess.run(my_var_times_two) #output : 2  

sess.run(my_var_times_two) #output : 4  

sess.run(my_var_times_two) #output : 8  

sess.run(my_var.assign_add(1)) #output : 9  

sess.run(my_var.assign_sub(4)) #output : 5  

{% endhighlight %}

***

#### trainable // 변수의 선택적 파라미터

만약 그래프 안의 어떤 변수가 수작업으로만 변경되어야 한다면, 변수의 trainable파라미터를 False로 하면 된다.  

not_trainable = tf.Variable(0, trainable=False)  

***

#### 그래프를 name scope로 구성하기 //텐서보드에서 시각화할 때 좋다.

그래프의 규모가 커지면 복잡도가 커진다. 큰 복잡도를 다루기 위해서 name scope를 사용한다.  

기본적인 name scope사용법은 with tf.name_scope("name"): 을 사용하는 것이다.  

{% highlight js %}

import tensorflow as tf

with tf.name_scope("Scope_A"):
    a = tf.add(1, 2)  
    b = tf.multiply(a, 3)  
    
with tf.name_scope("Scope_B"):
    c = tf.add(4, 5)  
    d = tf.multiply(c, 6)  

e = tf.add(b,d)

{% endhighlight %}
