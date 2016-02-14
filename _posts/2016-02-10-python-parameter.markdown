---
layout: post
title:  "[Python] Pyhon의 기본 매개변수 초기화에 대해서" 
date:   2016-02-10 02:00:00 +0900
categories: Python
---

Python의 기본 매개변수 초기화에 대해서
-------------------

한 가지 예제를 통해서 파이썬의 매개변수 초기화 시점에 대해서 생각해보자.

{% highlight python %}

def buggy(arg, result=[]):
    result.append(arg)
    print(result)

{% endhighlight %}

위의 buggy 라는 함수를 한번 실행해 보자 buggy('a'), buggy('b') 라는 식으로 말이다.

기본 매개변수(result=[])에 대해서 다른 언어를 통해서 알고 있다면

buggy('a') -> ['a'], buggy('b') -> ['b'] 로 생각할 것이다.

결과는 아래와 같다.

{% highlight python %}

>> buggy('a')

['a']

>> buggy('b')

['a', 'b']

{% endhighlight %}

어떻게 된 일일까? 기본 매개변수의 초기화 되는 시점에 대해서 함수를 호출(call)할 시점이 아니라
 
정의 되는 시점에서 result = [] 이 계산되기 때문이다.

그럼 여기서 추가적으로 이런 것을 고려해서 다른 예제를 하나 보자.

{% highlight python %}

def nonbuggy(arg, result=None):
    if result is None:
        result = []
    result.append(arg)
    print(result)
    
{% endhighlight %}

nonbuggy('a'), nonbuggy('b') 를 부르면 결과가 어떻게 될까?

결과는 아래와 같다.

{% highlight python %}

>>> nonbuggy('a')

['a']

>>> nonbuggy('b')

['b']

{% endhighlight %}

어라 nonbuggy 는 첫번째 호출될 때 result 가 None 일 시점에서 result 값이 리스트([])로 변경이 되면

당연히 두번째부터는 result 가 리스트 상태인것이 아닌가?

result 에 빈 리스트가 대입되는 전 후로 id 값을 한번 찍어보면 재밌는 결과를 알 수 있다.

{% highlight python %}

def nonbuggy(arg, result=None):
    if result is None:
        print(id(result))  #  4482439176
        result = []
        print(id(result))  #  4481633000
    result.append(arg)
    print(result)
    
{% endhighlight %}

실행해보면 대입 전후로 result 의 참조값이 바뀌는 것을 알 수 있다.

매개변수의 result 참조가 아니게 된다.
