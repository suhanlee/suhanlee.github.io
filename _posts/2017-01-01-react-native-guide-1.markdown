---
layout: post
title:  "[react-native] React Native 입문 - 1"
date:   2017-01-01 10:00:00 +0900
categories: react

---
react-native 를 처음 시작하는 사람들에게 도움이 될까 해서 작성한 글입니다.
---------------------------------------------------

## 1. 개발환경
먼저 node와 watchman이 설치 되어 있는지 확인한다.
필자의 경우 [nvm](https://github.com/creationix/nvm)을 주로 이용한다.

{% highlight bash %}
$ brew install node
$ brew install watchman
{% endhighlight %}

nvm을 사용해서 설치하려고 하는 경우는 아래와 같이 설치한다.

{% highlight bash %}
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash
$ nvm install node
{% endhighlight %}
노드가 설치 되어 있다면 react-native-cli 를 설치가 필요하다.
프로젝트를 생성하거나 ios, android 앱으로 실행시키기 위해서 필요하다.

{% highlight bash %}
$ npm install -g react-native-cli
{% endhighlight %}

## 2. 개발 에디터
페이스북이 권장하는 아톰 에디터 + nuclide 를 권장하고 싶다.

코드 autocomplete 등이 지원되고 syntax highlight 등이 깔끔하게 지원된다.

설치 방법은 [atom 에디터](https://atom.io/)를 먼저 설치하고 atom 에디터 내에서 atom package manager에 nuclide 패키지를 설치하면 된다.

관련된 정보는 [https://nuclide.io/docs/quick-start/getting-started/#installation](https://nuclide.io/docs/quick-start/getting-started/#installation) url을 참조하자.

## 3. 프로젝트 생성 및 빌드
그럼 이제 직접 프로젝트를 생성해볼 차례다.
쉘에서 react-native init [Project명] 으로 프로젝트를 생성한다.

모듈명이니 아무래도 프로젝트 이름의 첫글자는 대문자를 권장하고 싶다.

{% highlight bash %}
➜  react_study react-native init Hello
This will walk you through creating a new React Native project in /Users/suhanlee/react_study/Hello
Installing react-native...
Consider installing yarn to make this faster: https://yarnpkg.com
npm WARN deprecated node-uuid@1.4.7: use uuid module instead
hello@0.0.1 /Users/suhanlee/react_study/hello
{% endhighlight %}

생성된 프로젝트의 디렉토리 구조는 아래와 같다.

{% highlight bash %}
➜  Hello tree -L 1
.
├── __tests__
├── android
├── index.android.js
├── index.ios.js
├── ios
├── node_modules
└── package.json

4 directories, 3 files
{% endhighlight %}

여기서 android, ios 는 android, ios 용으로 생성된 프로젝트를 나타낸다.

react-native 는 말그대로 native 이기 때문에 플랫폼별 프로젝트를 빌드 한다.
index.android.js, index.ios.js 파일은 각 플랫폼별 시작 진입점에 해당하는 파일을 나타낸다.

이제 한번 실행해보자. ios 버전으로 실행하려면 아래와 같이 `run-ios` 명령 옵션을 준다.
{% highlight bash %}
$ react-native run-ios
{% endhighlight %}

안드로이드 빌드라면 `run-android` 이다.

{% highlight bash %}
$ react-native run-android
{% endhighlight %}

빌드가 문제가 없다면 먼저 `React Packager`가 뜨고 난 후 ios 라면 시뮬레이터가 뜰 것이고 안드로이드라면 에뮬레이터 혹은 디바이스가 뜰 것이다.

참고로 실행시켰는데 appRegistry에서 프로젝트 모듈을 찾을 수 없다는 에러가 나타나면 기존에 다른 프로젝트로 인한 `React Packager` 프로세스가
있는지 확인한 후 프로세스가 존재하면 제거 한 후 다시 시도 하면 된다.

## 4. Debug, Release

### Debug 모드
react-native 는 Debug 모드에서는 npm start 를 통해서 동작하는 데몬이 각 타겟에 bundle.js 파일을 업데이트 시켜 준다.
즉 코드 변경시에 자동으로 업데이트 하며, 앱의 변경 사항이 있다고 해서 네이티브 앱을 다시 빌드하지 않는다.

iOS 시뮬레이터에서 CMD+R 키를 눌러서 변경 사항을 강제로 업데이트 할 수 있다.
개인적으로는 CMD+D 를 눌러서 Development 옵션을 열고 `Enable Live Reload` 옵션을 사용하도록 권장한다.
그러면 에디터에서 수정한 내용이 즉각 반영된다.

참고로 안드로이드 단말에서 Development 옵션을 키려면 단말을 좌우로 흔들면 나타난다.
동일하게 `Enable Live Reload` 옵션을 켜서 반영할 수 있다.

### Release 모드
Release 모드에서는 최종적으로 bundle.js를 패키징하며 앱 내에 탑재한다.
앱을 개발/테스트가 아니라 최종적으로 배포할 시에 적용하면 되며 각 ios, android 빌드 방법을 따른다.



