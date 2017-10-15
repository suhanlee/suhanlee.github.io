---
layout: post
title:  "[rails] zero downtime deploy"
date:   2017-09-10 01:00:00 +0900
categories: rails-etc

---
ActiveRecord 에서 타임스탬프 설정 관련
---------------------------------------------------

Rails의 경우 ActiveRecord 를 통해서 생성한 timestamp 필드 DB에 들어가는 레코드의 timestamp 값이 기본적으로
UTC-0 기준이다. 이 경우 서버인 경우 로컬 시간으로 맞춰 줄 필요성이 있는데, config/environments/${staging} 파일에

`config.active_record.default_timezone = :local` 를 추가하면 로컬 시간으로 맞춰진다.

실제 분기 내용은 아래 rails 코드를 참조 한다.
{% highlight ruby %}
# rails/activerecord/lib/active_record/timestamp.rb
    def current_time_from_proper_timezone
      default_timezone == :utc ? Time.now.utc : Time.now
    end
{% endhighlight %}

[activerecord/lib/active_record/timestamp.rb](https://github.com/rails/rails/blob/cfb1e4dfd8813d3d5c75a15a750b3c53eebdea65/activerecord/lib/active_record/timestamp.rb)






