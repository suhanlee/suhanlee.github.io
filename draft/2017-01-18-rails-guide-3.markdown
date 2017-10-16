---
layout: post
title:  "[Rails] Rails 입문 - 3"
date:   2017-01-01 10:00:00 +0900
categories: rails-guide

---

이번에는 간단히 테스트에 대해서 알아보도록 하겠습니다.
---------------------------------------------------

테스트 케이스와 관련된 기본 사항들

setup do
    @article = articles(:one)
end

teardown do
   Rails.cache.clear
end

테스트 헬퍼


#test/test_helper.rb

module SignInHelper
  def sign_in_as(user)
    post sign_in_url(email: user.email, password: user.password)
  end
end

class ActionDispatch::IntegrationTest
  include SignInHelper
end

 d
레일즈는 테스트시에 test/fixtures 디렉토리에있는 픽스쳐들을 자동으로 읽어옵니다.

순서는 아래와 같은 순서로 진행됩니다.

1. 픽스쳐와 관련된 모든 데이터 삭제
2. 픽스쳐 데이터를 테이블로 로드
3. 필요할 경우 직접 접근할 수 있게 하기 위해서 픽스쳐 데이터를 메서드로 덤프


컨트롤러의 특정 테스트 메서드만 테스트를 할때는 이렇게 한다.

$ bin/rails test test/controllers/articles_controller_test.rb -n test_should_create_article


또한 ActiveRecord 객체 형태로 접근이 가능합니다.

# this will return the User object for the fixture named david
users(:david)

# this will return the property for david called id
users(:david).id

# one can also access methods available on the User class
david = users(:david)
david.call(david.partner)

# this will return an array containing the fixtures david and steve
users(:david, :steve)

2. 모델 테스팅

모델과 관련된 테스트들은 `test/models` 에 위치합니다

모델 테스트 스켈레톤을 아래와 같은 방식으로 생성할 수 있습니다.

$ bin/rails generate test_unit:model article title:string body:text
create  test/models/article_test.rb
create  test/fixtures/articles.yml


3. 인테그레이션 테스팅

전반적으로 어플리케이션 상호 작용을 테스트하기 위해 인테그레이션 테스트가 사용됩니다.

이와 관련된 테스트들은 test/integration 디렉토리에 파일이 위치합니다.

아래와 같은 방식으로 테스트 스켈레톤을 생성할 수 있습니다.


$ bin/rails generate integration_test user_flows
      exists  test/integration/
      create  test/integration/user_flows_test.rb


헬퍼와 관련된 URL은 다음을 참고하면 된다.

http://api.rubyonrails.org/classes/ActionDispatch/Integration/RequestHelpers.html


4. 기능 테스트

기능 테스트가 고려해야 할 사항들
* 웹 요청이 성공적인가
* 올바른 페이지로 유저가 리다이렉트 되었는가
* 유저가 올바로 인증되었는가
* 올바른 템플릿 응답을 하는가
* 유저에게 적절한 메시지가 포함된 뷰를 보여주는가?

4-1. 요청 가능한 타입

* get
* post
* patch
* put
* head
* delete

(1) get 메소드
GET 웹 요청을 날린다. 응답되는 결과는 @response 에 저장한다.
6개의 인자를 받을 수 있다.

* URI 혹은 라우트 헬퍼(e.g articles_url)
* params: 요청 파라미터
* headers: 요청 헤더
* env: 요청 환경을 수정하기 위함
* xhr: 요청이 ajax 요청인지 아닌지 true이면 ajax 요청
* as: 다른 타입으로(ex json)으로 인코딩 하기 위한 파라미터

get article_url, params: { id: 12 }, headers: { "HTTP_REFERER" => "http://example.com/home" }

patch article_url, params: { id: 12 }, xhr: true


4-2. 3가지 해시

cookies
flash
session

flash["gordon"]               flash[:gordon]
session["shmession"]          session[:shmession]
cookies["are_good_for_u"]     cookies[:are_good_for_u]

4-3. 인스턴스 변수들

@controller - 요청을 처리하는 컨트롤러
@request - 요청 객체
@response - 응답 객체



테스팅 뷰

assert_select 를 사용한다.
assert_select 'title', "Welcome to Rails Testing guide"


응답 객체가 JSON인 경우 아래와 같이 json_response 로 응답 객체를 만들고

편리하게 사용한다.


def json_response
    ActiveSupport::JSON.decode @response.body
end

assert_equal "Mike", json_response['name']

