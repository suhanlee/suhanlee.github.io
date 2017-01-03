---
layout: post
title:  "[react] Atom + Nuclide 사용시 CMD + Click 이 먹히지 않는 경우"
date:   2016-10-15 10:00:00 +0900
categories: react
---

Nuclide 사용시 CMD + Click이 먹히지 않는 경우(소스 찾아가기가 안되는 경우)
---------------------------------------------------

최근 React-Native 를 사용해서 개발을 시작하다가 페이스북이 만든 [Nuclide](https://nuclide.io) 를 사용하게 되었다.
사용하면서 Nuclide 는 정말 좋은 플러그인이라는 것을 새삼 느끼고 있었는데
소스 찾아가기(맥에서 CMD+Click)를 비롯한 Flow 동작이 먹히지 않는 것 아닌가.

`뭐가 원인이야!` 하고 삽질을 하다가 결국 알아낸 것은
리액트 프로젝트가 설치된 폴더의 .flowconfig 안의 버전이
설치된 flow 버전과 일치해야 한다는 점이었다.

아래와 같이 flow 버전을 확인하고
{% highlight bash %}
$ brew info flow
flow: stable 0.33.0 (bottled), HEAD
Static type checker for JavaScript
https://flowtype.org/
/usr/local/Cellar/flow/0.24.1 (7 files, 6M)
  Poured from bottle on 2016-08-04 at 00:41:39
/usr/local/Cellar/flow/0.33.0 (7 files, 4.9M) *
  Poured from bottle on 2016-10-15 at 19:48:10
From: https://github.com/Homebrew/homebrew-core/blob/master/Formula/flow.rb
==> Dependencies
Build: ocaml ✘, ocamlbuild ✘
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d

zsh completion has been installed to:
  /usr/local/share/zsh/site-functions
{% endhighlight %}

.flowconfig 파일 안의 버전을 동일하게 맞춘다.(나의 경우는 0.33)
{% highlight bash %}
[version]
^0.33.0
{% endhighlight %}
그리고 Atom Editor 를 재시작하면 오른쪽 하단에 파란색 아이콘이 뜨면서 waiting for diagnosis 가 잠깐 뜨고
정상 동작할 것이다.
