---
layout: post
title:  "[react-native] React Native 입문 - 2"
date:   2017-01-01 10:00:00 +0900
categories: react

---
react-native 를 처음 시작하는 사람들에게 도움이 될까 해서 작성한 글입니다.
---------------------------------------------------

이전 글에서 빌드를 했으니 이제 본격적으로 Hello World를 찍어보자.
여기서는 ios 를 기준으로 설명하겠다.

## 1. Hello World

먼저 index.ios.js 파일을 열고 아래의 코드를 복사 붙여넣기를 한다.

{% highlight js %}
import React, { Component } from 'react';
import { AppRegistry, Text } from 'react-native';

class HelloWorldApp extends Component {
  render() {
    return (
      <Text>Hello world!</Text>
    );
  }
}

AppRegistry.registerComponent('Hello', () => HelloWorldApp);
{% endhighlight %}

### (1) import
import 부분을 보면 React에 정의되어 가져오는 모듈이 React, Component 임을 알 수 있고
react-native 에 정의되어 가져오는 모듈이 AppRegistry, Text 임을 알 수 있다.
react를 하지 않고 react-native 를 입문하는 사람이라면 이 둘의 구분에 대해서 어느정도 알고 있어야 나중에 import
정의나 기존의 코드를 이해하는데 도움이 될 것이다.

### (2) HelloWorldApp
Component를 상속받아 render 메서드를 정의한다. render 메서드 안에서 return 할때 JSX 문법으로 구성이 된
 `<Text>` 태그 등이 보인다. 여기서 일단 render 메서드는 렌더링 해야 할 컴포넌트를 반환한다고 생각하고, return 할때 태그 형식으로 Component 를 반환할 수 있다고 생각하자.
 여기서 Text 도 내부적으로 보면 Component를 상속받은 컴포넌트이다.

### (3) AppRegistry

여기서 주의해야 할 것은 AppRegistry.registerComponent 의 첫번째 파라미터로 넘겨주는 'Hello' 이다.
react-native-cli 의 init 명령을 통해서 실행하게 되면 프로젝트명이 여기에 들어가는데 이것을 기준으로
ios 내부 브릿지에서 해당 모듈을 찾는다. 즉 기본 설정이 프로젝트명이다. 물론 수정가능하지만 ios 부분을 수정해야 한다.
추가적으로 HelloWorldApp이 첫번째 렌더링 될 컴포넌트인 것을 알 수 있다.

### (4) 결론

여기서 AppRegistry를 통해서 시작하는 컴포넌트를 지정하고 그 해당 컴포넌트의 render 메서드를 통해서 뷰를 렌더링 한다는 것을 알 수 있다.
또한 Text 또한 Component 이므로 render 메서드가 내부적으로 불릴 것이고 순차적으로 render 메서드가 처리될 것이다.

## 2. prop

이제 prop을 이해할 차례이다. 아래 소스를 실행해보자.

{% highlight js %}
import React, { Component } from 'react';
import { AppRegistry, Image } from 'react-native';

class Bananas extends Component {
  render() {
    let pic = {
      uri: 'https://upload.wikimedia.org/wikipedia/commons/d/de/Bananavarieties.jpg'
    };
    return (
        {% raw %}<Image source={pic} style={{width: 193, height: 110}} />{% endraw %}
    );
  }
}

AppRegistry.registerComponent('Hello', () => Bananas);
{% endhighlight %}

Image 컴포넌트에 속성으로 source 와 style 이 존재한다.
여기서 style은 react 에서 사용하는 정해져 있는 prop으로 크기나, 색깔 등 스타일을 표현하는데 사용하는데 추후 설명하겠다.

JSX문법으로 { } 안에 Javascript 코드를 실행할 수 있다.  여기서는 let 키워드로 정의한 url 필드를 가지고 있는 pic 객체를 전달하는 것을 알 수 있다.
이 경우 Image 컴포넌트 내부에서 `props` 속성을 통해서 이렇게 전달한 것을 접근할 수 있다.

이해를 위해서 다음 예제를 실행해보자.

{% highlight js %}
import React, { Component } from 'react';
import { AppRegistry, Text, View } from 'react-native';

class Greeting extends Component {
  render() {
    return (
      {% raw %}<Text>Hello {this.props.name}!</Text>{% endraw %}
    );
  }
}

class LotsOfGreetings extends Component {
  render() {
    return (
      {% raw %}<View style={{alignItems: 'center'}}>{% endraw %}
      <Greeting name='Rexxar' />
      <Greeting name='Jaina' />
      <Greeting name='Valeera' />
      </View>
    );
  }
}

AppRegistry.registerComponent('Hello', () => LotsOfGreetings);
{% endhighlight %}

### (1) LotsOfGreetings::render
LotsOfGreetings 컴포넌트에서 Greeting 컴포넌트를 참조하고 있는데 속성으로 name 에 값을 넘겨 주고 있다.

### (2) Greetings::render
Greeting 내에서는 this.props 멤버 필드를 통해서 속성 값을 접근하고 있음을 알 수 있다. 여기서는 `this.props.name` 이다.

### (3) View style에 alignItems 속성
View 의 레이아웃 속성 값을 의미하는 것으로 react-native 는 flex-box 레이아웃을 따른다.  여기서 View의 flex 는 column 이고(Y축)
alignItems 는 반대 축(여기서는 X축)의 정렬을 반영한다.  즉 X축 기준으로 가운데 정렬이 된다.


## 3. state

이제 State에 대해서 알아보자.

{% highlight js %}
import React, { Component } from 'react';
import { AppRegistry, Text, View } from 'react-native';

class Blink extends Component {
  constructor(props) {
    super(props);
    {% raw %}this.state = {showText: true};{% endraw %}

    // Toggle the state every second
    setInterval(() => {
    {% raw %}this.setState({ showText: !this.state.showText });{% endraw %}
    }, 1000);
  }

  render() {
    let display = this.state.showText ? this.props.text : ' ';
    return (
      <Text>{display}</Text>
    );
  }
}

class BlinkApp extends Component {
  render() {
    return (
      <View>
        <Blink text='I love to blink' />
        <Blink text='Yes blinking is so great' />
        <Blink text='Why did they ever take this out of HTML' />
        <Blink text='Look at me look at me look at me' />
      </View>
    );
  }
}

AppRegistry.registerComponent('Hello', () => BlinkApp);
{% endhighlight %}

prop의 경우 컴포넌트 외부에서 속성을 통해서 접근한다면, State 는 컴포넌트 내부에서 값을 다룬다.

또한 prop은 외부에서 전달되기 때문에 컴포넌트 내부에서 변경사항이 없다고 봐야 하지만, state 는 내부에서 값을 변경하며
값이 변경되는 여부에 따라서 react-natvie 내부적으로 뷰 렌더링이 다시 발생한다.

### (1) constructor
컴포넌트의 생성자이다. 컴포넌트가 생성될 때 호출이 된다. 즉 render 함수 이전에 호출이 된다.
여기서는 this.state = { showText: true } 로 속성을 초기화 하고 있다.
state 에 대한 변경은 state 참조 순서에 따른 오동작을 방지하기 위해서 초기화가 아니라면
this.setState 메서드를 통해서 값을 변경하는 것이 권장된다.

### (2) setInterval
여기서는 1초에 한번씩 this.setState 메서드를 호출하고 있다.
state 가 변경이 되면 뷰 렌더링을 다시 실행하므로 실행해보면 알겠지만 1초에 교대로 화면이 사라지는 것을 볼 수 있다.

### (3) 결론
state를 초기화 할때는 this.state 를 직접 접근하고, 읽기로 접근할 때는 this.state, 쓰기로 접근할 때는 this.setState 메서드를 활용하자.


## 4. style
style은 이미 정해져 있는 prop 이다.  color, fontweight, fontSize 등의 속성을 가진다.
아래 예제를 실행해보자.

{% highlight js %}
import React, { Component } from 'react';
import { AppRegistry, StyleSheet, Text, View } from 'react-native';

class LotsOfStyles extends Component {
  render() {
    return (
      <View>
        <Text style={styles.red}>just red</Text>
        <Text style={styles.bigblue}>just bigblue</Text>
        <Text style={[styles.bigblue, styles.red]}>bigblue, then red</Text>
        <Text style={[styles.red, styles.bigblue]}>red, then bigblue</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  bigblue: {
    color: 'blue',
    fontWeight: 'bold',
    fontSize: 30,
  },
  red: {
    color: 'red',
  },
});

AppRegistry.registerComponent('Hello', () => LotsOfStyles);
{% endhighlight %}

### (1) StyleSheet.create 메서드
StyleSheet.create 메서드를 통해서 특정 스타일 이름(bigblue, red)을 정하고 스타일 이름 안에
스타일 속성(color, fontWeight, fontSize) 등을 정의한다.

정의된 속성은 styles.red, styles.bigblue 등으로 style prop 에 값을 넣을 수 있다.

### (2) [styles.bigblue, styles.red]
style prop 에 배열 형식으로 여러 스타일 이름을 넣을 수 있다. 중복되는 경우 뒤에 있는 스타일 이름의 속성이 적용된다.