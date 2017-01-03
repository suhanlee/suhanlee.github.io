---
layout: post
title:  "[react-native] DP, Dimensions"
date:   2016-10-16 10:00:00 +0900
categories: react
---

React-Native에서 뷰의 크기는 DP단위로 계산한다.
---------------------------------

DP단위는 단말 디바이스 종류마다 다른데 아래와 같은 방법으로 디바이스의 DP(Dip)크기를 계산 할 수 있다.

단말의 width, height 전체 크기를 알아낸 후 비율을 계산하는 식으로 뷰 크기를 정하면 대부분의 단말에서 같은 크기로 보여질 것이다. 

{% highlight javascript %}
import Dimensions from 'Dimensions'
...
    var window_width = Dimensions.get('window').width;
    var window_height = Dimensions.get('window').height;
    
    console.log("window_width:" + window_width);
    console.log("window_height:" + window_height);

    // ADB LogCat 으로 확인
    10-17 01:39:07.151 3478-3530/? I/ReactNativeJS: window_width:360
    10-17 01:39:07.151 3478-3530/? I/ReactNativeJS: window_height:592
{% endhighlight %}

이외에 Flexbox 의 `flex` 값을 정해서 뷰 간의 상대적인 크기를 정할 수도 있다.