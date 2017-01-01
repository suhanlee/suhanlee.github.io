---
layout: post
title:  "[react-native] React Native 입문 - 3"
date:   2017-01-02 00:00:00 +0900
categories: react

---
react-native 를 처음 시작하는 사람들에게 도움이 될까 해서 작성한 글입니다.
---------------------------------------------------

이번 글에는 컴포넌트의 크기와 레이아웃에 대해서 다루겠다.

## 1. 컴포넌트의 크기

View 컴포넌트를 렌더링 할때 style 속성으로 width, height 속성을 지원한다.
여기서 속성의 단위는 Dip(density-independent pixels)이다. 픽셀이 아니다.
아래 예제를 한번 확인 해보자.

{% highlight js %}
 import React, { Component } from 'react';
 import { AppRegistry, View } from 'react-native';

 class FixedDimensionsBasics extends Component {
   render() {
     return (
       <View>
         {% raw %}<View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />{% endraw %}
         {% raw %}<View style={{width: 100, height: 100, backgroundColor: 'skyblue'}} />{% endraw %}
         {% raw %}<View style={{width: 150, height: 150, backgroundColor: 'steelblue'}} />{% endraw %}
       </View>
     );
   }
 };

 AppRegistry.registerComponent('Hello', () => FixedDimensionsBasics);
{% endhighlight %}

View 컴포넌트의 style prop 속성에 width, height 을 넣는데 각각 dp 단위로 넣고 있다.

그럼 단말의 가로, 세로 dp를 알아야 비율을 맞출 수 있을 텐데 라는 생각이 들 것이다.
물론 단말의 가로, 세로 dp를 알 수 있으나 flex-box 레이아웃 방법을 권장한다.
차근 차근 flex에 대해서 알아볼 것이다.

## 2. flex

다음 예제를 통해서 flex 에 대해서 감을 잡자.

{% highlight js %}
import React, { Component } from 'react';
import { AppRegistry, View } from 'react-native';

class FlexDimensionsBasics extends Component {
  render() {
    return (
      // Try removing the `flex: 1` on the parent View.
      // The parent will not have dimensions, so the children can't expand.
      // What if you add `height: 300` instead of `flex: 1`?
      {% raw %}<View style={{flex: 1}}>{% endraw %}
        {% raw %}<View style={{flex: 1, backgroundColor: 'powderblue'}} />{% endraw %}
        {% raw %}<View style={{flex: 2, backgroundColor: 'skyblue'}} />{% endraw %}
        {% raw %}<View style={{flex: 3, backgroundColor: 'steelblue'}} />{% endraw %}
      </View>
    );
  }
};

AppRegistry.registerComponent('Hello', () => FlexDimensionsBasics);
{% endhighlight %}

### (1) flex 부모
flex 속성은 부모 뷰 컴포넌트에서 먼저 속성을 정의한다. 그리고 자식 뷰 컴포넌트에서 확장(expand)을 하는 구조이다.
여기서는 부모가 flex : 1 로 속성을 정의하고, 자식 뷰 컴포넌트에서 flex 속성 1, 2, 3 으로 나눠서 확장하는 것을 알 수 있다.

### (2) flex 1, 2, 3
flex 속성이 가지는 전체에서 차지하는 비율이다. flexDirection 속성을 주지 않았을 때
기본이 column 값을 가지게 되고 아래로 확장한다.

## 3. flexDirection

다음은 flexDirection 과 관련되는 내용이다.

{% highlight js %}
 import React, { Component } from 'react';
 import { AppRegistry, View } from 'react-native';

 class FlexDirectionBasics extends Component {
   render() {
     return (
       // Try setting `flexDirection` to `column`.
       {% raw %}<View style={{flex: 1, flexDirection: 'row'}}>{% endraw %}
         {% raw %}<View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />{% endraw %}
         {% raw %}<View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />{% endraw %}
         {% raw %}<View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />{% endraw %}
       </View>
     );
   }
 };

 AppRegistry.registerComponent('Hello', () => FlexDirectionBasics);
{% endhighlight %}

### (1) flexDirection: 'row'
flexDirection을 row 로 가질 경우 자식 뷰 컴포넌트의 확장이 가로 방향으로 확장한다.
이번 예제의 경우 자식 뷰 컴포넌트는 width, height 속성을 지정함으로써 크기가 고정임을 알 수 있다.

## 4. Justify Content

부모 뷰 컴포넌트에서 justifyContent 속성을 정의하여 자식 뷰 컴포넌트의 배치에 영향을 줄 수 있다.

{% highlight js %}
 import React, { Component } from 'react';
 import { AppRegistry, View } from 'react-native';

 class JustifyContentBasics extends Component {
  render() {
    return (
      // Try setting `justifyContent` to `center`.
      // Try setting `flexDirection` to `row`.
      {% raw %}<View style={{
        flex: 1,
        flexDirection: 'column',
        justifyContent: 'space-between',
      }}>{% endraw %}
        {% raw %}<View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />{% endraw %}
        {% raw %}<View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />{% endraw %}
        {% raw %}<View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />{% endraw %}
      </View>
    );
  }
};

AppRegistry.registerComponent('Hello', () => JustifyContentBasics);
{% endhighlight %}


### (1) justifyContent: 'space-between'

부모 뷰 컴포넌트에서 justifyContent 속성으로 `flex-start`, `center`, `flex-end`, `space-around`, `space-between` 등을 가질 수 있다.  자식 뷰 컴포넌트의 간격에 영향을 준다.

## 5. Align Items

{% highlight js %}
import React, { Component } from 'react';
import { AppRegistry, View } from 'react-native';

class AlignItemsBasics extends Component {
  render() {
    return (
      // Try setting `alignItems` to 'flex-start'
      // Try setting `justifyContent` to `flex-end`.
      // Try setting `flexDirection` to `row`.
      {% raw %}<View style={{
        flex: 1,
        flexDirection: 'column',
        justifyContent: 'center',
        alignItems: 'center',
      }}>{% endraw %}
        {% raw %}<View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />{% endraw %}
        {% raw %}<View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />{% endraw %}
        {% raw %}<View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />{% endraw %}
      </View>
    );
  }
};
AppRegistry.registerComponent('Hello', () => AlignItemsBasics);
{% endhighlight %}

alignItems 속성은 자식 뷰 컴포넌트의 보조 축(secondary axis)의 정렬 기준을 결정한다.

예를 들어서 flexDirection 이 `column` 이면 세로 방향이 주축(primary axis)이고 가로 방향이 보조 축(secondary axis)이다. 여기서는 가로 방향에 대해서 가운데 정렬을 하게 된다.

사용 가능한 값으로 `flex-start`, `center`, `flex-end` `stretch` 가 있다.