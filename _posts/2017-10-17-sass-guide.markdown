---
layout: post
title:  "SASS 간략히 이해하고 사용하기"
date:   2017-10-17 10:00:00 +0900
categories: css
---

SASS는 기본적으로 CSS 에 대한 전처리기(preprocessor)이다.
CSS 가 제공하지 못하는 여러 언어적 특징들을 제공한다.
예를 들어서 변수(variables), 중첩(nesting), 믹스인(mixin), 상속(inheritance) 과 같은 특징들이다.

> SASS안에는 SASS, SCSS 2가지 문법(Syntax) 스타일의 프리프로세서가 존재한다.

> SASS는 인덴트 기반, SCSS는 브라켓({,}) 기반의 스타일이다

> SASS 파일은 확장자가 sass, SCSS는 확장자가 scss 이다.

{% highlight sass %}
// SASS
$font-stack:    Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $font-stack
  color: $primary-color
{% endhighlight %}

{% highlight scss %}
// SCSS
$font-stack:    Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
{% endhighlight %}

복잡해지는 CSS를 그냥 쓰기에는 유지보수,확장이 용이하지 않다.
그래서 이러한 형태의 전처리기를 사용하는데 semantic-ui 와 같은 프론트엔드 기반의 라이브러리들이 이러한 전처리기를 적극적으로 활용한다.

[http://sass-lang.com/](http://sass-lang.com) 를 통해서 상세한 정보를 얻을 수 있고 아래와 같이 설치한다. 

> SASS/SCSS 라이브러리는 루비로 작성된 라이브러리라서 루비젬을 이용해서 설치할 수 있다.

{% highlight bash %}
$ gem install sass
{% endhighlight %}

다른 방식(ex application)으로 설치 하는 것에 대해서는 공식 사이트를 참고하면 된다.
[http://sass-lang.com/install](http://sass-lang.com/install)

커맨드라인에서 SASS 전처리기를 수행하려면 아래와 같이 수행한다.
{% highlight bash %}
$ sass input.scss // ex: sass [SASS 파일]
{% endhighlight %}

CSS로 변환된 결과를 표준출력(stdout) 즉 모니터 화면으로 얻을 수 있으며
{% highlight bash %}
$ sass input.scss output.css
{% endhighlight %}

와 같이 css 형식으로 출력인자를 추가함으로써 파일로 출력할 수도 있다.

아래와 같이 —watch 옵션을 추가하면 디렉토리 단위로 파일 변경사항을 감시해서 전처리를 수행하게 할 수 있다.

{% highlight bash %}
$ sass —watch app/sass:public/stylesheets
{% endhighlight %}

위의 명령어는 콜론(:)으로 구분된 app/sass 폴더와 public/stylesheets 폴더 안에 있는 scss 파일의 변경 내역을 찾아서 수행한다.

{% highlight bash %}
➜  $ sass --watch sass
>>> Sass is watching for changes. Press Ctrl-C to stop.
      write sass/variables.css
      write sass/variables.css.map
>>> New template detected: sass/test.scss
      write sass/test.css
      write sass/test.css.map
>>> Change detected to: sass/variables.scss
      write sass/variables.css
      write sass/variables.css.map
{% endhighlight %}

디렉토리 지정 후 폴더내에 sass 파일들(sass, scss)이 추가 되거나 변경되면 그 즉시 다시 전처리가 실행된다.

현재 SASS 스타일 보다 SCSS 스타일이 많이 쓰이고 있어서 문법 설명은 SCSS 기준으로 설명하겠다.

### (1) 변수(Variables)

$variable 와 같은 형태로 앞에 $를 이용해서 변수를 정의할 수 있다.

{% highlight scss %}
$font-stack: Helvetica, sans-serif;

body {
	font: 100% $font-stack;
}
{% endhighlight %}

위의 코드를 전처리하면 아래와 같이 CSS 파일이 생성되는데 출력물을 보고 있으면 변수라기 보다는 치환 매크로에 가깝다.

{% highlight css %}
body {
	font: 100% Helvetica, sans-serif;
}
{% endhighlight %}

### (2) 중첩(Nesting)

CSS 는 셀렉터 사이에 포함하는 관계로 서술할 수 없다.
이는 중복된 코드를 작성하게 된다.

예를 들어서 아래와 같이 CSS를 서술 해보자.

{% highlight css %}
// CSS
nav ul {
	margin: 0;
	padding: 0;
	list-style: none;
}

nav li {
	display: inline-block;
}
nav a {
	padding: 6px 12px;
	text-decoration: none;
}
{% endhighlight %}

이것을 중첩 형태로 SCSS 식으로 다시 표기하면 아래와 같다.

{% highlight scss %}
// SCSS
nav {
	ul {
	    margin: 0;
	    padding: 0;
	    list-style: none;
	}

	li {
	    display: inline-block;
	}

	a {
	    padding: 6px; 12px;
	    text-decoration: none;
	}
}

{% endhighlight %}

중첩된 식으로 나열하니 구조적으로 보기가 쉬워졌다.

### (3) 파셜(Partials) and 임포트(import)

CSS 는 부분적으로 파일을 분리해서 파셜 형태로 작성 후에 불러오게끔 하는 @import 문을 지원하나 동적으로 HTTP request 요청을 하게 만드는 단점이 있다.

이러한 부분은 SASS의 파셜로 처리하면 CSS 를 생성하는 시점에서 합쳐서 처리할 수 있다.

> SASS의 파셜 형태는 파일이름의 시작을 언더스코어(_)로 시작한다.

> @import 지시어로 파셜 파일을 포함할 수 있다.

예를 들어서 아래의 @import ‘reset’ 은 _reset.scss 파일을 찾아서 포함한다. 전처리를 할 때 파일을 하나로 합쳐서 CSS 파일을 생성해준다.

{% highlight scss %}
// _reset.scss
html,
body,
ul,
ol {
	margin: 0;
	padding: 0;
}
{% endhighlight %}

{% highlight scss %}
// base.scss
@import ‘reset’;

body {
	font: 100% Helvetica, sans-serif;
	background-color: #efefef;
}

// css 결과
html, body, ul, ol {
	margin: 0;
	padding: 0;
}

body {
	font: 100% Helvetica, sans-serif;
	background-color: #efefef;
}
{% endhighlight %}

### (4) 믹스인(Mixins)

CSS는 CSS3 이후 아주 많은 크롬 파이어폭스 브라우저 등의 벤더(vender-prefix) 가 존재한다.
믹스인은 @mixin 지시어로 CSS 그룹을 만들 수 있는데 이런 벤더 프리픽스를 모아서 처리할 때 유용하다.

믹스인을 정의하고 나서 해당 믹스인을 포함 시킬 때는 @include 지시어를 사용한다.

예를 들면 아래와 같다.

{% highlight scss %}
@mixin border-radius($radius) {
	-webkit-border-radius: $radius;
	-moz-border-radius: $radius;
	-ms-border-radius: $radius;
		border-radius: $radius;
}

.box { @include border-radius(10px); }
{% endhighlight %}

{% highlight scss %}
// CSS 결과
.box {
	-webkit-border-radius: 10px;
	-moz-border-radius: 10px;
	-ms-border-radius: 10px;
	border-radius: 10px;
}
{% endhighlight %}

여기서는 변수를 활용해서 border-radius 값을 일괄 치환했다.

### (5) 확장/상속(Extend/Inheritance)

일반적인 언어에서 상속 개념과 매우 흡사하다. 특정 셀렉터의 속성을 @extend 지시어를 이용해서 상속할 수 있다.

{% highlight scss %}
// SCSS
.message {
	border: 1px solid #ccc;
	padding: 10px;
	color: #333;
}

.success {
	@extend .message;
	border-color: green;
}

{% endhighlight %}

전처리 결과는 아래와 같다.

{% highlight scss %}
// CSS 결과
.message, .success {
	border: 1px solid #cccccc;
	padding: 10px;
	color: #333;
}

.success {
	border-color: green;
}
{% endhighlight %}

### (6) 연산자(Operators)

CSS 에서 수학 연산자 (+, -, *, /, %)가 지원 되듯이 sass 도 지원한다.

{% highlight scss %}
// SCSS
.container {
	width: 100%;
}

article[color=“main”] {
	float: left;
	width: 600px / 960px * 100%;
}
{% endhighlight %}

{% highlight scss %}
// CSS 결과
.container {
	width: 100%;
}

article[color=“main”] {
	float: left;
	width: 62.5%;
}
{% endhighlight %}

CSS 파일로 변환하면서 수치가 계산되었다는 것이 보인다.
