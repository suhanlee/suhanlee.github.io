---
layout: post
title:  "[2016년 6월 24일] 주간 레일즈: 5.0.0.rc2 릴리즈 버그픽스, 그리고 그외!"
date:   2016-06-24 20:00:00 +0900
categories: rails
---

해당 글은 Godfrey Chan/godfreykfc@gmail.com 의 허락을 얻어서 번역한 글입니다.

원본 페이지: [https://rails-weekly.ongoodbits.com/2016/06/24/5-0-0-rc2-release-bugfixes-and-more](https://rails-weekly.ongoodbits.com/2016/06/24/5-0-0-rc2-release-bugfixes-and-more)


모두 안녕하세요! [Greg](https://twitter.com/gregmolnar)이 최신 소식을 레일즈 커뮤니티로부터 가져왔습니다.


## Featured


<table>
  <tr>
      <td class="author-profile"><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3504036/railscontributors.png"></td>
      <td><p><a href="http://contributors.rubyonrails.org/contributors/in-time-window/20160617-20160624">이번주 레일즈 컨트리뷰터</a></p><br>
      <div>
        22명의 대단한 사람들이 이번 주에 레일즈 프레임웍을 앞서나가게 했습니다! 만약 같이 참여하고 싶으시면 <a href="https://github.com/rails/rails/issues">이슈 리스트</a>를 봐주세요.
      </div>
      </td>
  </tr>
  <tr>
        <td class="author-profile"><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3504013/rails-logo.jpg"></td>
        <td><p><a href="http://weblog.rubyonrails.org/2016/6/22/Rails-5-0-rc2/">레일즈 5.0.0.rc2 릴리즈!</a></p><br>
        <div>
          레일즈 5 Rc2가 많은 버그픽스와 폴리싱(polishing) 작업을 통해서 릴리즈 되었습니다. 이제 최종 릴리즈까지 한개의 단계만 남았네요!
        </div>
        </td>
    </tr>
</table>


## Fixed


<table>
  <tr>
    <td class="author-profile"><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3504997/127960.jpeg"></td>
    <td><p><a href="https://github.com/rails/rails/pull/25411">글로빙 라우트(/)와 관련있는 rails/info 라우트 수정</a></p><br>
    <div>
       /rails/info 라우트가 접근이 안되는데 글로빙-라우트(/) 뒤에 추가 되었기(append) 때문에 라우트 매치가 되지 않은 것으로 보여집니다.
       <br>
       (역주: 개발모드에서 추가되는 빌트인 라우트로 app.routes.append를 app.routes.prepend 로 처리한 것으로 보여집니다.)
    </div>
    </td>
  </tr>
  <tr>
      <td class="author-profile"><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3504987/10473586.jpeg"></td>
      <td><p><a href="https://github.com/rails/rails/pull/24773">db:structure:load, PostgreSQL Error 발생시 조용한 실패(silent failure) 수정</a></p><br>
      <div>
      SQL 에러가 났을때 db:structure:load 태스크가 조용히 실패(silent failure)합니다. 하지만 이번 커밋으로 에러를 리포트하게 됩니다.
      </div>
      </td>
  </tr>
</table>


## Improved

<table>
  <tr>
    <td class="author-profile"><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3504986/536118.jpeg"></td>
    <td><p><a href="https://github.com/rails/rails/pull/23301">db:structure:dump 향상</a></p><br>
    <div>
    불필요한 db 구조 덤프를 피하기 위해서, --skip-comments 플래그를 mysqldump 커맨드에 전달됩니다.
    </div>
    </td>
  </tr>
</table>


## Changed

<table>
  <tr>
    <td class="author-profile"><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3505068/1036970.jpeg"></td>
    <td><p><a href="https://github.com/rails/rails/pull/25469/files">datetime_field 변경</a></p><br>
    <div>
    <b>datetime_tag</b> 헬퍼가 <b>datetime-local</b> input tag 를 생성합니다.
    <br>
    (역주: 새로운 HTML5 form 표준으로 인한 변경 사항(datetime 태그는 더 이상 존재 안함)으로 수정 하는 듯 합니다.
     <br>궁금하신 분은 <a href="https://html.spec.whatwg.org/multipage/forms.html#local-date-and-time-state-(type=datetime-local)">https://html.spec.whatwg.org/multipage/forms.html#local-date-and-time-state-(type=datetime-local)</a> 를 참조)
    </div>
    </td>
  </tr>
</table>

## Wrapping Up
여기까지 주간 레일즈였습니다! 여기 나열하기 힘들정도로 정말 많은 위대한 컨트리뷰션이 있었습니다. 자유롭게 <a href="https://github.com/rails/rails/compare/master@%7B2016-06-17%7D...@%7B2016-06-24%7D">확인해보세요!</a>

다음 주에 봐요!



