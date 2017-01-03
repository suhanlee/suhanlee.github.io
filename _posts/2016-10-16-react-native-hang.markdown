---
layout: post
title:  "[react-native] react-native 가 hang이 걸리는 경우"
date:   2016-10-16 10:00:00 +0900
categories: react
---

React-Native CLI 명령이 갑자기 Hang이 걸리는 경우가 있다.
-------------------------------


그럴 경우 watchman 이 행이 걸려 있다면 아래와 같이 watchman 을 제거하고 다시 설치해보기를 권장한다

{% highlight bash %}
$ rm -rf /usr/local/var/run/watchman/ && brew uninstall watchman && brew install watchman
{% endhighlight %}

관련된 이슈는 [https://github.com/facebook/react-native/issues/9943](https://github.com/facebook/react-native/issues/9943) 에 있다.