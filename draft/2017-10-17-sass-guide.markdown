---
layout: post
title:  "SASS 이해하기"
date:   2017-10-17 10:00:00 +0900
categories: css
---

SASS 는 기본적으로 CSS 에 대한 전처리기(preprocessor)이다.
CSS 가 제공하지 못하는 여러 언어적 특징들을 제공한다.
예를 들어서 변수(variables), 중첩(nesting), 믹스인(mixin), 상속(inheritance) 과 같은 특징들이다.

흔히 CSS를 유지 보수, 확장하기 위해서 SASS를 많이 사용한다.
SASS는 http://sass-lang.com/ 를 통해서 접근할 수 있다.

루비를 이용한다면 설치하려면 `gem install sass` 를 이용해서 편리하게 설치할 수 있다.

SASS 파일은 확장자로 scss 형태로 표기 된다.
sass input.scss 와 같은 형태로 CSS로 변환된 결과를 표준출력(stdout)으로 얻을 수 있으며
sass input.scss output.css 와 같이 출력인자를 추가함으로써 파일로 출력할 수도 있다.

—watch 옵션을 추가하면 디렉토리 단위로 수행하게 할 수 있다.

sass —watch app/sass:public/stylesheets

위의 명령어는 콜론(:)으로 구분된 app/sass 폴더와 public/stylesheets 폴더 안에 있는 scss 파일의 변경 내역을 찾아서 수행한다.

➜  ruby_study sass --watch sass
>>> Sass is watching for changes. Press Ctrl-C to stop.
      write sass/variables.css
      write sass/variables.css.map
>>> New template detected: sass/test.scss
      write sass/test.css
      write sass/test.css.map
>>> Change detected to: sass/variables.scss
      write sass/variables.css
      write sass/variables.css.map

(1) 변수(Variables)
$variable 형식으로 변수를 가질 수 있다.
타입을 가지는 것이 아니기 때문에 변수라기 보다는 매크로에 가깝다.

$font-stack: Helvetica, sans-serif;

body {
	font: 100% $font-stack;
}

그러면 아래와 같이 CSS 파일이 생성된다.
body {
	font: 100% Helvetica, sans-serif;
}

(2) 중첩(Nesting)
CSS 는 셀렉터 사이에 포함하는 관계로 서술할 수 없다.
이는 중복된 코드를 작성하게 된다.

예를 들어서 아래와 같이 CSS를 서술 해보자.

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

이것을 중첩 형태로 다시 표기하면 아래와 같다.

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


(3) 파셜(Partials) and 임포트(import)

CSS 는 부분적으로 파일을 분리해서 파셜 형태로 작성 후에 불러오게끔 하는 @import 문을 지원하나 동적으로 HTTP request 요청을 하게 만드는 단점이 있다.

SASS에서 파셜 형태는 파일이름의 시작을 언더스코어(_)로 시작한다.
그리고 @import 지시어로 파셜 파일을 포함할 수 있다.

예를 들어서 아래의 @import ‘reset’ 은 _reset.scss 파일을 찾아서 포함한다. Sass 는 정적으로 파일을 하나로 합쳐서 CSS 파일을 생성해준다.

// _reset.scss
html,
body,
ul,
ol {
	margin: 0;
	padding: 0;
}

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

(4) Mixins
CSS는 CSS3 이후 아주 많은 vender prefix 가 존재한다.
믹스인은 @mixin 지시어로 CSS 그룹을 만들 수 있다.
해당 믹스인을 포함 시킬 때는 @include 지시어를 사용한다.

예를 들면 아래와 같다.
@mixin border-radius($radius) {
	-webkit-border-radius: $radius;
	-moz-border-radius: $radius;
	-ms-border-radius: $radius;
		border-radius: $radius;
}

.box { @include border-radius(10px); }

// CSS 결과
.box {
	-webkit-border-radius: 10px;
	-moz-border-radius: 10px;
	-ms-border-radius: 10px;
	border-radius: 10px;
}

(5) Extend/Inheritance
일반적인 언어에서 상속 개념과 매우 흡사하다. 해당 셀렉터의 속성을 @extend 지시어를 이용해서 사용할 수 있다.

.message {
	border: 1px solid #ccc;
	padding: 10px;
	color: #333;
}

.success {
	@extend .message;
	border-color: green;
}

// CSS 결과
.message, .success {
	border: 1px solid #cccccc;
	padding: 10px;
	color: #333;
}

.success {
	border-color: green;
}

(6) 연산자(Operators)
CSS 에서 수학 연산자 (+, -, *, /, %)가 지원 되듯이 sass 도 지원한다.

.container {
	width: 100%;
}

article[color=“main”] {
	float: left;
	width: 600px / 960px * 100%;
}

// CSS 결과
.container {
	width: 100%;
}

article[color=“main”] {
	float: left;
	width: 62.5%;
}
