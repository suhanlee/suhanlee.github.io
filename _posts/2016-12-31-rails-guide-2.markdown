---
layout: post
title:  "[Rails] 레일즈 입문 2"
date:   2017-01-01 10:00:00 +0900
categories: rails

---

이어서 앞서 작업한 posts 게시판에서 조금 더 알아보겠습니다.
---------------------------------------------------

앞서 생성한 게시판에 글을 작성해보면 내부적으로 아래와 비슷한 로그가 찍힐 겁니다.

{% highlight bash %}
Started POST "/posts" for ::1 at 2017-01-01 17:42:09 +0900
Processing by PostsController#create as HTML
  Parameters: {"utf8"=>"✓", "authenticity_token"=>"0xn2fDQwv3iUUZU70yncXKw97pSJCGOadibAaljbQAbkLgv2r+rwA/NQE106i3o16Cfk/W9h6oNyXNxkNIXN2g==", "post"=>{"name"=>"test name", "contents"=>"contents test"}, "commit"=>"Create Post"}
   (0.1ms)  begin transaction
  SQL (0.7ms)  INSERT INTO "posts" ("name", "contents", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["name", "test name"], ["contents", "contents test"], ["created_at", "2017-01-01 08:42:09.914883"], ["updated_at", "2017-01-01 08:42:09.914883"]]
   (0.7ms)  commit transaction
Redirected to http://localhost:3000/posts/1
Completed 302 Found in 6ms (ActiveRecord: 1.5ms)
{% endhighlight %}

PostController#create 메서드 내에서 내부적으로 Insert 쿼리가 발생했음을 알 수 있습니다.
{% highlight bash %}
(0.1ms)  begin transaction
  SQL (0.7ms)  INSERT INTO "posts" ("name", "contents", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["name", "test name"], ["contents", "contents test"], ["created_at", "2017-01-01 08:42:09.914883"], ["updated_at", "2017-01-01 08:42:09.914883"]]
   (0.7ms)  commit transaction
{% endhighlight %}
참고로 파라미터로 authenticity_token 은 CSRF 취약점에 대한 예방 토큰으로 새로운 레코드를 생성해야 할 경우 입력 폼에 넣습니다.
여기서는 posts/new 라우터의 HTML 렌더링 결과물을 보면 아래와 같은 태그가 들어 있음을 알 수 있습니다.
csrf token은 매번 접속 할때마다 랜덤으로 생성 됩니다.
{% highlight html %}
  <meta name="csrf-param" content="authenticity_token" />
<meta name="csrf-token" content="Tag3+2o4q0JU3AuUjbkeXia1at2X237FD4JQvNXQQCJ6n8px8eLkOTPdjfJkG7g3Yq9gtHGy99wL+EyyuY7N/g==" />
{% endhighlight %}

위와 같이 내부적으로 DB에 CRUD하는 과정을 Rails Console 을 통해서 쉽게 수행할 수 있습니다.
Rails Console은 쉘에서 아래와 같은 명령으로 실생합니다.
{% highlight bash %}
$ rails console
{% endhighlight %}
또는 아래와 같이 생략 가능합니다.
{% highlight bash %}
$ rails c
{% endhighlight %}
수행 결과는 아래와 같이 인터렉티브한 쉘이 나타납니다.
{% highlight bash %}
Loading development environment (Rails 4.2.3)
2.2.2 :001 >
{% endhighlight %}
Rails Console은 내부적으로 루비의 인터렉티브 쉘인 irb 를 열고 Rails와 연관되는 루비 모듈들을 전부 로드 합니다.
따라서 특정 기능을 사용하기 위해서 추가적으로 require 하지 않아도 됩니다.

아래와 같이 Post 모델이 존재하는지 즉시 확인 할 수 있습니다.

{% highlight bash %}
2.2.2 :001 > Post
 => Post (call 'Post.connection' to establish a connection)
2.2.2 :002 > Post.connection
 => #<ActiveRecord::ConnectionAdapters::SQLite3Adapter:0x007fb8a091b670 @transaction_manager=#<ActiveRecord::ConnectionAdapters::TransactionManager:0x007fb8a08fb938 @stack=[], @connection=#<ActiveRecord::ConnectionAdapters::SQLite3Adapter:0x007fb8a091b670 ...>>,...
 2.2.2 :003 > Post
  => Post(id: integer, name: string, contents: text, created_at: datetime, updated_at: datetime)
{% endhighlight %}

001 에서 Post를 실행했을 때 아직 DB와 연결되어 있지 않은 상태입니다.

002 에서 Post.connection 을 통해서 DB와 연결합니다.

003 에서 다시 Post를 실행했을 때 필드명과 타입이 나타납니다. id, name, contents, created_at, updated_at 등입니다.
이러한 필드명은 앞에서 스캐폴딩을 할 때 마이그레이션 파일에서 정의되어 있습니다.

Post는 app/models/post.rb 에 ActiveRecord::Base 클래스를 상속받은 형태로 존재합니다.
{% highlight ruby %}
class Post < ActiveRecord::Base
end
{% endhighlight %}

ActiveRecord::Base를 상속받는 것으로 ORM의 특성을 가질 수 있습니다.
내부적으로는 ActiveModel 이 프록시(Proxy)로 존재합니다만 그것은 추후 validation 단계에서 설명하겠습니다.

간단히 마지막 레코드를 가져와 보겠습니다.
{% highlight ruby %}
2.2.2 :004 > Post.last
  Post Load (0.2ms)  SELECT  "posts".* FROM "posts"  ORDER BY "posts"."id" DESC LIMIT 1
 => #<Post id: 1, name: "test name", contents: "contents test", created_at: "2017-01-01 08:42:09", updated_at: "2017-01-01 08:42:09">
{% endhighlight %}

아래처럼 레코드를 가져온 결과가 Post 객체로 반환하고 DB의 필드명을 메서드 형식(post.name, post.contents 등)으로 호출 가능합니다.
이것은 런타임에서 특정 필드가 있는지 확인 후 있다면 해당 하는 결과를 반환하는 식으로 내부적으로 루비의 missing_method 메커니즘을 통해서 동작하게 됩니다.

{% highlight ruby %}
2.2.2 :005 > Post.last.name
  Post Load (0.2ms)  SELECT  "posts".* FROM "posts"  ORDER BY "posts"."id" DESC LIMIT 1
 => "test name"
{% endhighlight %}

이제 Rails Console 에서 게시글을 작성해보겠습니다.
{% highlight ruby %}
2.2.2 :006 > post = Post.new(name: 'test2', contents: 'contents2')
 => #<Post id: nil, name: "test2", contents: "contents2", created_at: nil, updated_at: nil>
2.2.2 :007 > post.save
   (0.1ms)  begin transaction
  SQL (0.6ms)  INSERT INTO "posts" ("name", "contents", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["name", "test2"], ["contents", "contents2"], ["created_at", "2017-01-01 09:10:35.372088"], ["updated_at", "2017-01-01 09:10:35.372088"]]
   (0.8ms)  commit transaction
 => true
2.2.2 :008 > post
 => #<Post id: 2, name: "test2", contents: "contents2", created_at: "2017-01-01 09:10:35", updated_at: "2017-01-01 09:10:35">
2.2.2 :009 >
{% endhighlight %}

006 먼저 Post.new 에서 기록할 필드명을 채웁니다. 여기서는 name, contents 가 해당 필드입니다.
여기서 확인하실 사항은 id 가 nil로 되어 있는데, 이것은 아직 DB에 실제로 반영되어 있지 않은 상태라 id 값 할당이 되지 않은 상태입니다.

007 post.save 를 통해서 실제로 반영하면 DB 내부적으로 id 값이 채워집니다. Rails는 기본적으로 레코드를 생성할때 구분하기 위한 형태로 id:integer 필드를 가집니다.  이 필드는 레코드가 생성 될때마다 1씩 자동으로 증가합니다.(auto_increment)

008 post 를 통해서 id 값이 2로 증가 된 것을 볼 수 있습니다.

레코드 수정 또한 같은 패턴으로 할 수 있습니다.

{% highlight ruby %}
2.2.2 :009 > post.name = "test3"
 => "test3"
2.2.2 :010 > post.save
   (0.1ms)  begin transaction
  SQL (0.4ms)  UPDATE "posts" SET "name" = ?, "updated_at" = ? WHERE "posts"."id" = ?  [["name", "test3"], ["updated_at", "2017-01-01 09:16:12.541512"], ["id", 2]]
   (0.8ms)  commit transaction
 => true
{% endhighlight %}

009 post.name 으로 수정할 필드의 값을 변경합니다.

010 post.save 를 통해서 값을 수정합니다.

삭제의 경우도 아래와 같이 하면 됩니다.

{% highlight ruby %}
2.2.2 :011 > post.delete
  SQL (1.2ms)  DELETE FROM "posts" WHERE "posts"."id" = ?  [["id", 2]]
 => #<Post id: 2, name: "test3", contents: "contents2", created_at: "2017-01-01 09:10:35", updated_at: "2017-01-01 09:16:12">
2.2.2 :012 >
{% endhighlight %}

011 post.delete 메서드를 호출해서 삭제를 수행합니다. 내부적으로 DELETE 쿼리가 발생했음을 알 수 있습니다.

이 밖에 where이나 find_by 메서드 등을 사용해서 조건에 맞는 값을 찾거나 가져온 값에서 select, pluck 등을 이용하여 값을 가공할 수 있습니다.

더 재밌는 기능들은 좀 더 Rails를 살펴보면서 설명하도록 하겠습니다.