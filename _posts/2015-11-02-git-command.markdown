---
layout: post
title:  "[Git] git command"
date:   2015-11-02 02:31:00 +0900
categories: git github
---

$ git add item

> item 을 staging 에 추가

$ git reset item

> item 을 staging 에 제외, 즉 git add 의 반대 명령어
> git rm item 은 파일을 실제 삭제하고 staging 에 반영


$ git branch -m item item2

> item branch를 item2 로 변경함

$ git branch

> 변경된 사항 확인

$ git reflog

> git 명령어 로그 보기

$ git commit -m "message"

> 한줄 짜리 commit 을 생성

$ git checkout item

> item 브랜치로 브랜치 이동

$ git checkout -b item

> item 브랜치 생성 후 바로 이동

$ git submodule update --init --recursive

> submodule을 재귀적으로 초기화