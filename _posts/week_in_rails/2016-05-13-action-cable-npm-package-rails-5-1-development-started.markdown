---
layout: post
title:  "[2016년 5월 13일] 주간 레일즈: Action Cable NPM 패키지 및 레일즈 5.1 개발 시작"
date:   2016-05-13 20:00:00 +0900
categories: rails
---

해당 글은 Godfrey Chan/godfreykfc@gmail.com 의 허락을 얻어서 번역한 글입니다.

원본 페이지: [https://rails-weekly.ongoodbits.com/2016/05/13/action-cable-npm-package-rails-5-1-development-started](https://rails-weekly.ongoodbits.com/2016/05/13/action-cable-npm-package-rails-5-1-development-started)

![](https://goodbits-production.s3.amazonaws.com/uploads/newsletter_settings/logo/225/db659964-7ac2-48f1-8ad5-3d4907dfafd6.png)

모두 안녕. 여기 [Prathamesh](https://twitter.com/_cha1tanya/) 가 아직 RailsConf 열병에서 아직 회복 중입니다.

이번 주 레일즈 월드에서 무슨 일이 일어났는지 같이 보시죠.

## Featured

<table>
  <tr>
      <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3364636/download.png"></td>
      <td><p><a href="http://confreaks.tv/events/railsconf2016">Confreaks에서 RailsConf 2016 영상들</a></p><br>
      <div>
      RailsConf 2016 영상들이 Confreaks 에 나타나기 시작했습니다.
      <br> Jaremy Daer 의 <a href="http://confreaks.tv/videos/railsconf2016-opening-keynote-jeremy-daer">오프닝 키노트</a>를 놓치지 마세요!
      </div>
      </td>
  </tr>
  <tr>
    <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3388080/contributors.png"></td>
    <td><p><a href="http://contributors.rubyonrails.org/contributors/in-time-window/20160506-20160513">이번 주 레일즈 컨트리뷰터</a></p><br>
    <div>
    38명의 굉장한 사람들이 이번주에 레일즈에 기여(contributed) 했네요.
    <br>또한 5명의 첫 커미터가 있었습니다.
    <br>환영합니다!
    <br>만약 기여하고 싶다면 <a href="https://github.com/rails/rails/issues">이슈 트래커</a>에 눈을 떼지 마세요.
    </div>
    </td>
  </tr>
</table>



## New Stuff


<table>
    <tr>
        <td>
        <img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3364629/47848__2_.png">
        </td>
        <td><p><a href="https://github.com/rails/rails/commit/8ecc5ab1d88532a239f17c7520ed922c7579b01c">레일즈 5.1 개발이 시작되었습니다</a></p><br>
            <div>
            지난 주 레일즈 5.0 RC1이 릴리즈 되고,
            <br>마스터 브랜치는 이제 Rails 5.1 을 가리키게 됩니다.
            </div>
        </td>       
    </tr>
     <tr>
            <td>
            <img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3364622/3020626__1_.png">
            </td>
            <td><p><a href="https://github.com/rails/rails/commit/548c1d6e8b819ca4e02e6218b67107c580ee65f2">NPM에 Action Cable 패키지가 등장 </a></p><br>
                <div>
                앞으로 Action Cable은 자체적인 NPM 패키지를 가지고,
                <br>레일즈 릴리즈 시스템의 일부로 릴리즈 됩니다.
                <br>이제 레일즈 세상 밖에서도 Action Cable Javascript 에셋은 사용 가능하죠! 
                </div>
            </td>       
        </tr>
        
</table>


## Fixed

<table>
  <tr>
    <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3364621/1529387__1_.jpeg"></td>
    <td><p><a href="https://github.com/rails/rails/commit/6007e584d824225e51f47ba0684d48ea3eb8f518https://github.com/rails/rails/commit/6007e584d824225e51f47ba0684d48ea3eb8f518">JSON이 직렬화될때 수정 감지(mutuation detection) 로직이<br> 항상되는 이슈 해결</a></p><br>
    <div>
    이 이슈는 JSONB 컬럼의 changes 메서드와 연관이 있는 것으로 
    <br>컬럼값의 변경사항이 없어도 changes의 반환값이 변경사항이
    <br>존재하는 것처럼 반환하는 이슈에 대한 해결입니다.   
    </div>
    </td>
  </tr>
  <tr>
    <td>
        <img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3364631/3860146__1_.jpeg">
    </td>
    <td><p><a href="https://github.com/rails/rails/pull/24980">ActiveRecord::Attribute::Null#type_cast 추가</a></p><br>
      <div>
      데이터베이스에 해당 필드가 없는 상태에서
      <br>ActiveRecord::Base#attribute 를 기본값이 있는 형태로 사용할 때,
      <br>모델의 save 메서드를 호출 하면
       <br>NotImplementedError 가 발생하는 이슈가
       있었습니다 
       <br>이 커밋은 AciveRecord::Attribute::Null 클래스에 
       <br>type_cast를 정의하면서 해결합니다
      </div>
    </td>
  </tr>
</table>


## Improved


<table>
  <tr>
    <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3364630/5355.jpeg"></td>
    <td><p><a href="https://github.com/rails/rails/pull/24935https://github.com/rails/rails/pull/24935">Action Cable 자바스크립트 테스트</a></p><br>
    <div>
   <a href="https://github.com/javan/blade">Blade</a> 기반으로 Action Cable 테스트 스윗의 테스트 커버리지가 향상됩니다!
    </div>
    </td>
  </tr>
</table>






## Wrapping Up

이번 주 저의 보고를 끝내겠습니다. 그외 다른 많은 일이 있었으니 
<br> 자유롭게 뛰어들어서 <a href="https://github.com/rails/rails/compare/master@%7B2016-05-07%7D...@%7B2016-05-13%7D">확인해보세요!</a>


