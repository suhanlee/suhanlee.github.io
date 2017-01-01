---
layout: post
title:  "[Rails] Rails 입문 - 1"
date:   2016-12-31 10:00:00 +0900
categories: rails
---

주위에 Rails를 처음 공부하는 사람들을 위해서 도움이 되고자 본 문서를 작성하고자 합니다.
---------------------------------------------------

Rails 가 가지고 있는 강력한 스캐폴딩 기능을 통해서 CRUD가 가능한 게시판을 만들어 보는 것으로
초심자들에게 충분히 매력을 느낄 수 있다고 생각하기에 이 글을 작성합니다.

## 1. Rails를 시작하기 위해 설정해야 하는 것들

Rails는 Ruby 언어로 작성되어 있는 루비 라이브러리(gem)입니다.
따라서 Rails를 시작할 때 사용하는 Ruby 언어의 버전 또한 고려해야 합니다.

- Ruby 버전을 선택합니다.(Rails 5.0 기준으로, 권장은 2.3 이상)

    Ruby 버전은 [RVM(http://rvm.io)](http://rvm.io)이나 [rbenv(https://github.com/rbenv/rbenv)](https://github.com/rbenv/rbenv) 등의 버전 관리 유틸을 사용하면 선택할 수 있습니다.

    예를 들어서 RVM이 설치가 되어 있다면 2.3.0으로 변경시에는 아래와 같은 명령을 사용합니다.
    {% highlight bash %}
    $ rvm use 2.3.0
    {% endhighlight %}

- Ruby 버전을 선택 후 Rails Gem 설치가 필요합니다.

    Rails는 gem install rails 명령어로 설치합니다
    {% highlight bash %}
    $ gem install rails
    {% endhighlight %}



## 2. Rails로 CRUD 이해를 위해서 간단한 게시판 만들기

CRUD는 Create, Read, Update, Delete 의 앞글자만 따서 만든 용어로
생성, 읽기, 수정, 삭제 의 기능 자체를 이야기 합니다. 흔히 DB에 입출력 하는 작업들을 일컫습니다.

Rails의 매력을 느끼기 위해서는 Rails로 간단한 게시판을 만들면서 CRUD 동작을 체험해보는 것이 효과적이라고 생각합니다
아래의 명령을 통해서 blog라는 이름으로 프로젝트를 생성합니다.

{% highlight bash %}
$ rails new blog
{% endhighlight %}

프로젝트를 생성하면 아래와 같이 생성 되어 있을 것입니다.

디렉토리 구조를 확인하기 위해서는 현재 폴더에서 tree 라는 명령을 쓰는 것이 효과적입니다.
혹시 tree가 깔려 있지 않다면 설치를 해야 하는데, 맥(Mac)의 경우 Homebrew가 설치 되어 있다면 아래 명령을 통해서 설치가 가능합니다.

{% highlight bash %}
$ brew install tree
{% endhighlight %}

{% highlight bash %}
➜  blog git:(master) ✗ tree
.
├── Gemfile
├── README.rdoc
├── Rakefile
├── app
│   ├── assets
│   │   ├── images
│   │   ├── javascripts
│   │   │   └── application.js
│   │   └── stylesheets
│   │       └── application.css
│   ├── controllers
│   │   ├── application_controller.rb
│   │   └── concerns
│   ├── helpers
│   │   └── application_helper.rb
│   ├── mailers
│   ├── models
│   │   └── concerns
│   └── views
│       └── layouts
│           └── application.html.erb
├── bin
│   ├── bundle
│   ├── rails
│   ├── rake
│   └── setup
├── config
│   ├── application.rb
│   ├── boot.rb
│   ├── database.yml
│   ├── environment.rb
│   ├── environments
│   │   ├── development.rb
│   │   ├── production.rb
│   │   └── test.rb
│   ├── initializers
│   │   ├── assets.rb
│   │   ├── backtrace_silencers.rb
│   │   ├── cookies_serializer.rb
│   │   ├── filter_parameter_logging.rb
│   │   ├── inflections.rb
│   │   ├── mime_types.rb
│   │   ├── session_store.rb
│   │   └── wrap_parameters.rb
│   ├── locales
│   │   └── en.yml
│   ├── routes.rb
│   └── secrets.yml
├── config.ru
├── db
│   └── seeds.rb
├── lib
│   ├── assets
│   └── tasks
├── log
├── public
│   ├── 404.html
│   ├── 422.html
│   ├── 500.html
│   ├── favicon.ico
│   └── robots.txt
├── test
│   ├── controllers
│   ├── fixtures
│   ├── helpers
│   ├── integration
│   ├── mailers
│   ├── models
│   └── test_helper.rb
├── tmp
│   └── cache
│       └── assets
└── vendor
    └── assets
        ├── javascripts
        └── stylesheets

38 directories, 38 files
{% endhighlight %}

그럼 이제 간단히 스캐폴딩(scaffold)을 통해서 이름(name)과 내용(contents)이 있는 게시판 형태로 만들어 보겠습니다.
스캐폴딩이 해주는 것은 Rails 가 사용하는 관습(Convention)을 이용해서 잘 엮어 놓은 기본 코드 템플릿을 만들어줍니다.

스캐폴딩을 통해서 기본적으로 만들어 놓은 코드 위에서 작업을 하면 밑바닥부터 작성하는 것보다 시간, 노력적인 측면에서 효과적입니다.

아래와 같이 명령을 쉘에서 입력합니다.

{% highlight bash %}
$ rails generate scaffold Post name:string contents:text
{% endhighlight %}

혹은 아래와 같이 generate 부분은 g로 생략이 가능합니다.

{% highlight bash %}
$ rails g scaffold Post name:string contents:text
{% endhighlight %}

위의 명령을 통해서 얻게 되는 결과는 아래와 같습니다.
{% highlight bash %}
➜  blog git:(master) ✗ rails g scaffold Post name:string contents:text
      invoke  active_record
      create    db/migrate/20161229121245_create_posts.rb
      create    app/models/post.rb
      invoke    test_unit
      create      test/models/post_test.rb
      create      test/fixtures/posts.yml
      invoke  resource_route
       route    resources :posts
      invoke  scaffold_controller
      create    app/controllers/posts_controller.rb
      invoke    erb
      create      app/views/posts
      create      app/views/posts/index.html.erb
      create      app/views/posts/edit.html.erb
      create      app/views/posts/show.html.erb
      create      app/views/posts/new.html.erb
      create      app/views/posts/_form.html.erb
      invoke    test_unit
      create      test/controllers/posts_controller_test.rb
      invoke    helper
      create      app/helpers/posts_helper.rb
      invoke      test_unit
      invoke    jbuilder
      create      app/views/posts/index.json.jbuilder
      create      app/views/posts/show.json.jbuilder
      invoke  assets
      invoke    coffee
      create      app/assets/javascripts/posts.coffee
      invoke    scss
      create      app/assets/stylesheets/posts.scss
      invoke  scss
      create    app/assets/stylesheets/scaffolds.scss
{% endhighlight %}

위의 결과로 여러가지 파일들이 만들어졌습니다.  몇가지 파트로 나눠서 설명을 하겠습니다.

{% highlight bash %}
 invoke  active_record
      create    db/migrate/20161229121245_create_posts.rb
      create    app/models/post.rb
      invoke    test_unit
      create      test/models/post_test.rb
      create      test/fixtures/posts.yml
{% endhighlight %}

(1) invoke active_record

rails는 active_record 라는 라이브러리를 포함하고 있는데 이것은 DB와 직접적인 연결을 감싸서 구현해주는 ORM입니다.

ORM은 Object Relational Mapping 의 약자로 객체 관계 매핑을 의미합니다.
기존의 DB에 질의하던 언어 SQL 방식이 아니라 OOP의 객체 방식으로 DB에 대신 질의할 수 있고
결과값 또한 객체로 가져 오는 형태를 의미합니다.

{% highlight bash %}

{% endhighlight %}

생성된 db/migrate/20161229121245_create_posts.rb 은 DB에 반영할 마이그레이션 파일입니다.
여기서는 posts 라는 테이블 안에 name 이라는 이름의 string, contents 이름의 text 필드가 존재합니다.
그 다음에 나오는 app/models/post.rb 는 ActiveModel 을 상속받은 클래스로 앞서 ActiveRecord 와 연결하는
OOP 객체 클래스입니다.

위의 명령은 결과적으로 name, contents 라는 필드를 가지는 Post 모델, 마이그레이션 코드를 만듭니다


> Rails는 MVC 구조가 명확한 프레임웍입니다.

> MVC가 뭔지 모르시는 분들을 위해서 MVC에 대해서 간단히 설명하겠습니다. MVC는 Model View Controller 의 약자로
Model, View, Controller 3개의 역할을 나누고 각 역할사이의 관계를 정립합니다.

> 예를 들어 사용자가 웹브라우저를 열고 /test 라는 url로 접속을 요청하면 이 요청을 Controller가 받습니다.
> Controller 내에서 Model 에 접근해서 데이터를 가져온 후에 View에 Model 데이터를 반영합니다.
> 최종적으로 반영된 결과값(View 에서 렌더링된 결과값)을 웹브라우저에게 반환하고, 사용자는 웹사이트를 보게 됩니다.

> Rails는 MVC 위에서 Naming Convention으로 이루어진 암묵적인 규칙을 가지고 있고 이것을 적극적으로 활용합니다.
> 스캐폴딩을 할때 정하는 모델명이 Post 이면 Controller의 이름은 PostsController 가 되고
> PostController 의 인스턴스 메서드의 이름이 뷰와 1:1로 맵핑 되는 형태로 나타납니다.(index -> index.html.erb)
> 이러한 암묵적인 부분들은 보일 때마다 추가적으로 언급하겠습니다.


다음으로 route 부분을 살펴 보겠습니다.

(2) invoke resource_route

{% highlight bash %}
invoke  resource_route
       route    resources :posts
{% endhighlight %}

Rails는 가장 상위 디렉토리 routes.rb 파일 안에 컨트롤러와 매핑되는 라우팅 정보가 포함이 되어 있습니다.
결과적으로 resources :posts 는 레일즈가 정한 기본적인 CRUD 형태의 라우터를 추가합니다.

`rake routes` 명령을 통해서 추가된 라우터 정보를 확인 가능합니다.

{% highlight bash %}
$ rake routes
   Prefix Verb   URI Pattern               Controller#Action
    posts GET    /posts(.:format)          posts#index
          POST   /posts(.:format)          posts#create
 new_post GET    /posts/new(.:format)      posts#new
edit_post GET    /posts/:id/edit(.:format) posts#edit
     post GET    /posts/:id(.:format)      posts#show
          PATCH  /posts/:id(.:format)      posts#update
          PUT    /posts/:id(.:format)      posts#update
          DELETE /posts/:id(.:format)      posts#destroy
{% endhighlight %}

Prefix 에 나타난 부분은 뷰에서 접근할 때 매칭 되는 이름을 나타내며
Verb 는 HTTP Method, URI Pattern 에서 format은 html, json 등의 mime-type
Controller#Action 은 연결되는 컨트롤러의 메서드를 말합니다.
뒤에 컨트롤러와 뷰를 설명하면서 추가 설명하도록 하겠습니다.

다음으로 controller와 view 부분을 살펴 보겠습니다.

(3) invoke scaffold_controller

{% highlight bash %}
invoke  scaffold_controller
      create    app/controllers/posts_controller.rb
      invoke    erb
      create      app/views/posts
      create      app/views/posts/index.html.erb
      create      app/views/posts/edit.html.erb
      create      app/views/posts/show.html.erb
      create      app/views/posts/new.html.erb
      create      app/views/posts/_form.html.erb
{% endhighlight %}

app/controller/posts_controller.rb 에 PostsController 라는 이름으로 Controller 클래스가 위치합니다.
그리고 index, edit, show, new 의 인스턴스 메서드를 가지고 있습니다.
이 메서드들은 이름 규칙에 따라서 app/views/posts/index, edit, show, new 의 view 들과 매칭이 됩니다.

{% highlight bash %}
class PostsController < ApplicationController
  before_action :set_post, only: [:show, :edit, :update, :destroy]

  # GET /posts
  # GET /posts.json
  def index
    @posts = Post.all
  end

  # GET /posts/1
  # GET /posts/1.json
  def show
  end

  # GET /posts/new
  def new
    @post = Post.new
  end

  # GET /posts/1/edit
  def edit
  end
{% endhighlight %}

여기서 PostsController 안에 있는 index 메서드에서 사용하는 뷰는 암묵적으로 app/views/posts/index.html.erb
와 자동으로 연결됩니다.  이러한 부분은 Rails가 정한 이름 규칙(Naming Convention)에 의거해서 암묵적으로 동작하는 부분들입니다.


## 3. 마이그레이션 코드를 실제 DB에 반영하기

이제 마이그레이션 코드를 이용해서 실제 DB에 반영합니다.
아래와 같은 명령어로 DB 에 실질적으로 반영할 수 있습니다.

{% highlight bash %}
➜  blog git:(master) ✗ rake db:migrate
== 20161229121245 CreatePosts: migrating ======================================
-- create_table(:posts)
   -> 0.0011s
== 20161229121245 CreatePosts: migrated (0.0011s) =============================
{% endhighlight %}

Rails는 기본 DB가 sqlite로 되어 있습니다. 위 명령은 Environment 에 따라서 {Environment}.sqlite 형태로 파일을 생성하고
posts 테이블 변동 사항(= 여기서는 테이블 생성)을 실제 반영합니다.

## 4. 서버 확인

자 이제 실제로 반영이 되어 있는지 확인할 차례입니다.

{% highlight bash %}
$ rails server
{% endhighlight %}

혹은 아래와 같이 단축(server -> s)해서 명령을 실행합니다.

{% highlight bash %}
$ rails s
=> Booting WEBrick
=> Rails 4.2.3 application starting in development on http://localhost:3000
=> Run `rails server -h` for more startup options
=> Ctrl-C to shutdown server
[2016-12-29 21:57:14] INFO  WEBrick 1.3.1
[2016-12-29 21:57:14] INFO  ruby 2.0.0 (2015-12-16) [universal.x86_64-darwin16]
[2016-12-29 21:57:14] INFO  WEBrick::HTTPServer#start: pid=22738 port=3000
{% endhighlight %}

Rails는 환경 변수 Environment 가 설정되어 있지 않으면 기본적으로 development로 인식합니다.
여기서는 Webrick 이라는 루비 내장 웹서버가 동작하는 것으로 알 수 있습니다.
만약 production 이라면 기본적으로 puma 웹 서버가 동작하게 됩니다.

이제 웹브라우저를 열고 http://localhost:3000/posts 로 접근하면 게시판 화면이 나타납니다.

기본적으로 CRUD가 반영이 된 게시판에서 게시글을 입력, 수정, 삭제 등을 할 수 있습니다.
로그를 확인하면서 열심히 CRUD를 하시길 권장합니다. :)

좀 더 재미있는 이야기들은 추후 계속 업데이트하면서 다루도록 하겠습니다.

