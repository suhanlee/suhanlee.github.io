---
layout: post
title:  "Sass 기본"
date:   2017-01-02 01:00:00 +0900
categories: dev

---

Sass는 프리프로세서다.
CSS는 재미있고,

```shell
sass --watch app/sass:public/stylesheets
```

변수

```shell
SCSS SYNTAX
$font-stack:  Helvetica, sans-serif;
$primary-color: #333;

body {
    font: 100% $font-stack;
    color: $primary-color;
}
```

```shell
body {
    font: 100% Helvetica, sans-serif;
    color: #333;
}
```


Netsting

```shell
nav {
    ul {
        margin: 0;
        padding: 0;
        list-style: none;
    }

    li { display: inline-block; }

    a {
      display: block;
      padding: 6px 12px;
      text-decoration: none;
    }
}
```

```shell
nav ul {
    margin: 0;
    padding: 0;
    list-style: none;
}

nav li {
    display: inline-block;
}

nav a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
}

```

Partials

_partial.scss


Import

CSS

_reset.scss, base.scss
```SCSS SYNTAX
// _reset.scss

html,
body,
ul,
ol {
    margin: 0;
    padding: 0;
}

// base.scss

@import 'reset';

body {
    font: 100% Helvetica, sans-serif;
    background-color: #efefef;
}

.message {
    border: 1px solid #ccc;
    padding: 10px;
    color: #333;
}

.success {
    @extend .message;
    border-color: green;
}

.error {
    @extend .message;
    border-color: red;
}

.warning {
    @extend .message;
    border-color: yellow;
}


.message, .success, .error, .warning {
    border: 1px solid #cccccc;
    padding: 10px;
    color: #333;
}

.success {
    border-color: green;
}

.error {
    border-color: red;
}

.warning {
    border-color: yellow;
}

