---
layout: post
title:  "[Go] Go Webserver 시작하기"
date:   2016-10-01 10:00:00 +0900
categories: go
---

간단히 Webserver 실행하기
------------------

WebServer 예제

{% highlight go %}
package main

import (
       	"fmt"
       	"net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
       	fmt.Fprintf(w, "Hello World %s", r.URL.Path)
}

func main() {
       	http.HandleFunc("/", handler)
       	http.ListenAndServe(":8080", nil)
}
{% endhighlight %}

