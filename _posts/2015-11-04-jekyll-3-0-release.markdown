---
layout: post
title:  "[Ruby] Jekyll 3.0 release"
date:   2015-11-04 23:39:00 +0900
categories: Ruby
---

> original : [`http://jekyllrb.com/news/2015/10/26/jekyll-3-0-released/`](http://jekyllrb.com/news/2015/10/26/jekyll-3-0-released/)

아래는 주요 업데이트 된 내용들이다.

시간 나는 대로 하나씩 주제를 확인해보면서 알아봐야할 듯 하다.

- Incremental regeneration (experimental, enable with `--incremental`)
- Liquid profiler (add `--profile` to a build or serve)
- Hook plugin API (no more monkey-patching!)
- Dependencies reduced from 14 to 8, none contain C extensions. We're hoping to reduce this even more in the future.
- Changed version support: no support for Ruby 1.9.3, added basic JRuby support. Better Windows support.
- Extension-less URLs
- `site.collections` is an array of collections, thus:
    - `collection[0]` becomes `collection.label`
    - `collection[1]` becomes `collection`
- Default highlighter is now Rouge instead of Pygments
- Lots of performance improvements
- ... and lots more!

> [Code of Conduct] 페이지가 신설되었다.

> [Jekyll Talk] 가 추가되었다. 
 Jordan 에 의해 운영이 되며, Jekyll 포럼 역할을 할 것으로 보여진다.

> [full history] 를 통해서 보다 자세한 내용을 참조할 수 있다.


[Code of Conduct]: https://github.com/jekyll/jekyll/blob/master/CONDUCT.md
[Jekyll Talk]: https://talk.jekyllrb.com
[full history]: http://jekyllrb-ko.github.io/docs/history/#v3-0-0