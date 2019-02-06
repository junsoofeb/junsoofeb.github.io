---
bg: "deep.jpg"
layout: post
title:  "4장, 오차 역전파"
crawlertitle: "BackPropagation"
summary: "backpropagation"
date:   2019-02-06
categories: posts
tags: 'deep_learning'
author: parkjunsoo
---
#### 서론  


3장에서 신경망 학습에 대해 공부했다.  

신경망 학습에서 가중치 매개변수에 대한 손실 함수의 기울기는 수치미분을 사용해서 계산했다.  
수치미분은 단순하고 구현하기도 쉽지만, 계산이 오래걸린다는 단점이 있다.

이번 장에서는 손실 함수의 기울기를 효율적으로 계산하는 [오차 역전파]를 공부한다.

오차 역전파를 이해하는 방법은 크게 2가지이다.
1. 수식을 통한 이해
2. 계산 그래프를 통한 이해

4장에서는 시각적인 이해를 위해 계산 그래프로 공부한다.

##### 결론부터 말하면,
##### 역전파는 순방향의 반대 방향으로 [부분적 미분]을 전파하는 것이다.
***

#### 계산 그래프 (computational graph)

계산 그래프는 계산 과정을 그래프로 나타낸 것이다.  
여기서의 그래프는 자료구조 중의 그래프(방향성 그래프)이며, node와 edge로 표현한다.

##### 계산 그래프를 이용한 문제 풀이 과정
1. 계산 그래프를 만든다(구성한다).
2. 계산은 그래프 왼쪽부터 오른쪽 방향으로 진행한다.

##### 출발점 -> 종착점 방향으로의 전파가 순전파(forward propagation)이고,

##### 출발점 <- 종착점 방향으로의 전파가 역전파(back propagation)이다.

***
#### 왜 계산 그래프로 계산을 하는가?

계산 그래프의 장점은 _부분적 계산_ 을 들 수 있다.

전체 문제가 아무리 복잡하더라도, 각 노드에서는 단순한 계산에 집중할 수 있어서 문제를 단순하게 접근할 수 있다.

##### 가장 큰 이유는 역전파를 통해 [미분]을 효율적으로 계산할 수 있기 때문이다.

***

#### 연쇄 법칙 (chain rule)

역전파는 연쇄 법칙을 따른 것이다.

역전파는 [부분적인 미분]을 순방향의 반대 방향으로 전파한다.  
이 [부분적 미분]을 전달하는 원리는 연쇄 법칙을 따른다.

##### [부분적 미분]은 순전파 때의 미분을 구하는 것이다.

연쇄 법칙을 이해하기 위해서 먼저 합성함수부터 알아야한다.  
합성함수란 여러개의 함수로 구성된 함수이다.

z = (x + y)^2 이 함수는
1. z = t^2

2. t = (x + y)

이렇게 두 함수로 구성된다.

연쇄법칙은 합성함수의 미분에 대한 성질이다.  
##### 연쇄 법칙의 정의 : _합성 함수의 미분은 합성함수를 구성하는 각 함수의 미분의 곱으로 나타낼 수 있다_

***

#### 역전파의 구조

[덧셈 노드의 역전파]

ex) z = x + y 를 대상으로 역전파를 생각해보자.

x에 대한 z의 미분은 1 이고, y에 대한 z의 미분도 1 이다.

역전파는 순방향의 반대 방향으로 [부분적 미분]을 전파하는 것이라 했다.  
##### 부분적 미분이 둘 다 1이기 때문에,
##### 덧셈 노드의 역전파는 입력된 값을 그대로 다음 노드에 전파한다.

[곱셈 노드의 역전파]

ex) z = xy 를 대상으로 역전파를 생각해보자.

##### x에 대한 z의 미분은 y 이고, y에 대한 z의 미분은 x 이다.

##### 곱셈 노드의 역전파는 상류의 값에 순전파 때의 입력신호를 서로 바꾸고 곱해서 하류로 보낸다.

즉, 순전파 때 입력신호가 x였다면 역전파에서는 y를 곱하고,  
순전파 때 입력신호가 y였다면 역전파에서는 x를 곱해서 하류로 보낸다.

***

#### 덧셈 계층, 곱셈 계층 구현

여기서 [계층]이란, 신경망의 기능을 나타내는 단위이다.  
덧셈 노드와 곱셈 노드를 [계층]단위로 구현해보자.

모든 계층은 forward()와 backward()라는 공통의 메소드를 갖도록 하자.  
각각 순전파와 역전파를 처리한다.

##### 덧셈 계층 구현

{% highlight js %}

class Add_layer:  

    def __init__(self):  
        pass

    def forward(self, x, y):
        out = x+y

        return out

    def backward(self, dout):
        # dout은 상류에서 넘어온 미분값
        # 덧셈 계층은 미분값이 1
        dx = dout * 1
        dy = dout * 1

        return (dx, dy)

{% endhighlight %}

##### 곱셉 계층 구현


{% highlight js %}


class Mul_layer:  

    def __init__(self):  
        self.x = None
        self.y = None

    def forward(self, x, y):
        self.x = x
        self.y = y
        out = x*y

        return out

    def backward(self, dout):
        # dout은 상류에서 넘어온 미분값  
        # 곱셈 계층에서는 x와 y 바꿔 곱한다.  
        dx = dout * self.y
        dy = dout * self.x

        return (dx, dy)

{% endhighlight %}

***

#### 활성화 함수 계층 구현

여기서의 forward()와 backward()는 np.array를 인수로 받는다고 가정한다.  

##### ReLU 계층 구현

[ReLU 함수]  

y = x (x > 0)    
y = 0 (x <= 0)

이제 역전파를 생각해보자.  

[x에 대한 y의 미분]  

 dy/dx = 1 (x > 0)  
 dy/dx = 0 (x <= 0)  

순전파에서의 입력 x가 0보다 크면, 역전파는 상류의 값을 그대로 하류로 보낸다.  
반면 순전파의 입력 x가 0보다 작으면, 역전파는 하류로 신호를 보내지 않는다. (정확히는 0 을 보낸다.)  

{% highlight js %}

class ReLU:
    def __init__(self):
        self.mask = None

    def forward(self, x):
        self.mask = (x <= 0)
        out = x.copy()
        out[self.mask] = 0

        return out

    def backward(self, dout)
        dout[self.mask] = 0
        dx = dout

        return dx

{% endhighlight %}

렐루 클래스는 mask라는 인스턴스 변수를 갖는다.  
mask는 True/False로 구성된 넘파이 배열이다.  
순전파의 입력x가 0보다 크면 False, 0이하면 True이다.  

##### sigmoid 계층 구현

[sigmoid 함수]  

y = 1 / 1 + e^-x

시그모이드 함수를 계산 그래프로 표현해보자.  
입력 => 출력 으로 표현하였다.  
1. x 와 -1의 곱셈연산 => -x  

2. -x에 exp연산(y = e^x연산 수행) => e^-x

3. e^-x 와 1의 덧셈연산 => 1+ e^-x

4. 1+e^-x에 / 연산 (/은 y = 1/x 연산 수행한다.) => 1 / 1 + e^-x

이제 역전파를 생각해보자.  
역전파는 당연히 순전파의 역순으로 진행된다.  

1. / 연산(y = 1/x)을 미분하면 dy/dx = -1 / x^2이고, x를 y로 치환하면 -y^2이다.

2. 덧셈 노드는 상류의 값을 그대로 하류로 보낸다.

3. exp노드를 미분하면 그대로 dy/dx =  e^x이다.

4. 곱셈 노드는 순전파 때의 값을 서로 바꾸어서 곱하고 전파한다.

즉,  

1단계에서 역전파 떄는 상류의 전달값에 -y^2(순전파 출력의 제곱에 -붙인 값)을 곱해서 하류로 보낸다.  

3단계에서는 상류의 값에 순전파때의 출력을 곱해서 하류로 전달한다.    
여기서는 e^-x가 순전파때의 출력이었다.

4단계에서는 x 방향의 역전파는 -1을 곱하고, -1 방향의 역전파는 x를  곱한다.

최종 출력을 계산해보면,   

dL/dyㆍy^2ㆍe^-x가 나온다. L은 역전파의 초기값이다.  

이 결과를 보면 순전파의 입력x와 출력y 만으로 계산 가능함을 알 수 있다.  

##### 또한 y = 1 / 1 + e^-x 이므로 y를 이 값으로 치환하면, sigmoid의 역전파는 순전파의 출력y만으로 계산할 수 있다.

y만으로 표현한 시그모이드의 역전파는  

dy/dx = dL/dyㆍy(1-y)이다.

이를 구현해보자.  

{% highlight js %}

class sigmoid:
    def __init__(self):
        self.out = None

    def forward(self, x):
        out = 1 / 1(1 + np.exp(-x))
        self.out = out

        return out

    def backward(self, dout)
        dx = dout * (1.0 - self.out) * self.out

        return dx

{% endhighlight %}

***

#### 행렬의 내적(Affine 변환)과 softmax with loss 구현

행렬의 내적(행렬의 곱셈)을 기하학에서는 어파인(affine)변환이라 한다.  
그래서 Affine 계층이라는 이름으로 행렬의 내적을 구현한다.

[Affine 구현]  
Y = WX + B를 계산그래프로 표현해보자.

1. W와 X의 dot연산(행렬곱셈 수행) => WX

2. WX와 B의 덧셈 연산 => WX + B == Y

##### 여기서 주의할 점은, X,W,B가 행렬(다차원 배열)이라는 점이다.

##### 이전까지의 그래프에서는 노드사이에 스칼라값이 오고 갔다.

##### 행렬이기 때문에 Shape를 서로 맞춰야 한다!!

##### dot연산은 행렬의 곱셈일 뿐, 스칼라의 곱셈연산과 동일하다.

##### 다만 Shape를 맞춰야하므로 바꾸어 곱할 때 [전치행렬]로 만들어 곱한다.

{% highlight js %}

class Affine:
    def __init__(sefl, W, b):
        self.W = W
        self.b = b
        self.x = None
        self.dW = None
        self.db = None

    def forward(self, x):
        self.x = x
        out = np.dot(x, self.W) + self.b

        return out

    def backward(self, dout):
        dx = np.dot(dout, self.W.T)
        self.dW = np.dot(self.x.T, dout)
        self.db = np.sum(dout, axis=0)

        reuturn dx

{% endhighlight %}

[softmax with loss 구현]

마지막으로 [출력층]에서 사용하는 소프트맥스 함수에 관해 공부하자.  

신경망을 학습시킬 때는 출력층에서 소프트맥스 함수를 사용한다.  
그러나 추론할 때에는 일반적으로 출력층에서 소프트맥스 함수를 사용하지 않는다.

여기서는 손실 함수인 CEE도 포함하여, softmax_with_loss라는 이름으로 구현하자.

softmax_with_loss 계층은 도출과정이 다소 복잡하여 간소화한 버전으로 구현한 결과만 제시한다.

먼저 순전파이다.  
여기서는 3 클래스 분류로 가정했다.

1. 이전의 계층에서 3개의 입력을 받는다. (a1, a2, a3)  

2. softmax계층에서는 입력(a1, a2, a3)을 정규화해서 y1, y2, y3을 출력한다.

3. CEE계층은 softmax의 출력(y1, y2, y3)과 정답 레이블(t1, t2, t3)을 입력받고 그것들로부터 손실(L)을 출력한다.

역전파의 결과는

(y1-t1, y2-t2, y3-t3)라는 깔끔한 결과가 나온다.  

이것은 [소프트맥스의 출력과 정답 레이블의 차] == 오차이다.   

#### 모든 신경망의 역전파에서는 이 오차가 앞 계층에 전달되는 것이다.

{% highlight js %}

class SoftmaxWithLoss:
    def __init__(self):
        self.loss = None  
        self.y = None  
        self.t = None

    def forward(self, x, t):
        self.t = t  
        self.y = softmax(x) # softmax()와 CEE는 아래에 구현함  
        self.loss = cross_entropy_error(self.y, self.t)

        return self.loss

    def backward(self, dout=1):
        batch_size = self.t.shape[0]
        dx = (self.y - self.t) / batch_size

        return dx

def softmax(a):
    c = np.max(a)
    exp_a = np.exp(a - c) #overflow방지
    sum_exp_a = np.sum(exp_a)
    y = exp_a / sum_exp_a

    return y

def cross_entropy_error(y, t): #y와 t는 넘파이 배열  
    delta = 1e-7

    return -np.sum(t * np.log(y + delta))

{% endhighlight %}

***

#### 정리

신경망 학습 순서를 다시 복습해보자.

전제 : 신경망에는 조절가능한 가중치와 편향이 있고, 그것들을 훈련데이터에 맞게 조절하는 과정을 학습이라 한다.

1단계 - 미니 배치

2단계 - 기울기 구하기

3단계 - 매개변수 갱신

4단계 - 반복

##### 3장까지는 기울기를 구할때 [수치미분]을 사용했다.
##### 수치미분은 구현하기 쉽지만 계산이 느리다는 단점이 있다.
##### 이런 단점을 극복할 수 있는 [오차 역전파]를 배웠다.

수치미분은 아예 사용되지 않는것이 아니다.

오차 역전파의 구현이 다소 복잡하기 때문에,   
오차역전파로 구한 기울기가 정확한지  
[검증하는 용도로 수치미분으로 구한 기울기가 사용]된다.

올바르게 구현했다면, 수치미분과 오차역전파 결과가 둘 다 0에 가깝게 나올 것이다.  

딱 0이 되는 것은 드물다.  
컴퓨터의 게산자체가 부동소수점 등 미세한 오차를 포함하기때문!
