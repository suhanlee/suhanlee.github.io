---
layout: post
title:  "[2016년 6월 17일] 주간 레일즈: 스마트 파일 업데이트 체커, 테스트 가능한 커스텀 큐 어댑터 그리고 그외!"
date:   2016-06-17 20:00:00 +0900
categories: rails
---

해당 글은 Godfrey Chan/godfreykfc@gmail.com 의 허락을 얻어서 번역한 글입니다.

원본 페이지: [https://rails-weekly.ongoodbits.com/2016/06/17/smarter-file-update-checker-testable-custom-queue-adapters-and-more](https://rails-weekly.ongoodbits.com/2016/06/17/smarter-file-update-checker-testable-custom-queue-adapters-and-more)


모두 안녕하세요! [Roque](https://twitter.com/repinel) (aka Godfrey[9]) 최신 소식을 레일즈 커뮤니티로부터 가져왔습니다.


## Featured


<table>
  <tr>
      <td class="author-profile"><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3434306/twir_120.png"></td>
      <td><p><a href="http://contributors.rubyonrails.org/contributors/in-time-window/20160611-20160617">이번주 레일즈 컨트리뷰터</a></p><br>
      <div>
        이번주는 23명의 사람들이 레일즈에 컨트리뷰트 하셨습니다. 또한 4명의 첫 컨트리뷰터도 얻었습니다. 환영합니다. 계속 합시다!
      </div>
      </td>
  </tr>
</table>


## Fixed


<table>
  <tr>
    <td class="author-profile"><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3481146/5355.jpeg"></td>
    <td><p><a href="https://github.com/rails/rails/pull/25411">이름 충돌 나는 상황에서 non-HTML 템플릿 digesting 수정 </a></p><br>
    <div>
    JSON과 같이 non-HTML 템플릿을 렌더링하는 */* 리퀘스트(request) 에 대한 부정확한 템플릿 다이제스트(incorrect template digest)에 대한 수정입니다.
   HTML 템플릿이 요청된 타입 대신 캐시를 계산하는데 사용되었습니다.
    </div>
    </td>
  </tr>
  <tr>
      <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3481144/12642.png"></td>
      <td><p><a href="https://github.com/rails/rails/pull/25271">'FinderMethods#exists?'에서 RangeError 예외 방지</a></p><br>
      <div>
      값이 범위를 벗어났을 때 예외 발생 대신 불린 값을 리턴하게 됩니다.
      </div>
      </td>
  </tr>
</table>


## Improved

<table>
  <tr>
    <td class="author-profile"><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3481085/59744.png"></td>
    <td><p><a href="https://github.com/rails/rails/pull/25302">파일 수정 체커(File update checker)가 프로세스당 처음에 한번씩 동작하게 됩니다.</a></p><br>
    <div>
    체커(Checker)가 Puma와 같은 멀티스레드 기반의 웹서버에서 나이스하게 동작할 예정입니다
     자세한 내용은 <a href="https://github.com/rails/rails/pull/25302">풀 리퀘스트</a>를 확인해주세요.
    <br>
    (역주: 개발 모드에서 파일 변경이 바로 리로드가 되지 않는다는 이슈에 대한 수정입니다. 풀 리퀘스트를 확인하시면 프로세스당 boot 메서드를 수행하는 것을 알 수 있습니다.)
    </div>
    </td>
  </tr>
  <tr>
      <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3481145/76227.jpeg"></td>
      <td><p><a href="https://github.com/rails/rails/pull/25367">커스텀 큐 어댑터가 Active Job 테스트들과 동작하게 됩니다.</a></p><br>
      <div>
      만약 Active Job 에서 커스텀 큐 어댑터를 사용한다면, 당신의 테스트케이스에서 queue_adapter_for_test 를 오버라이드 해서 사용중인 테스트 헬퍼를 사용함으로써 이점을 얻을 수 있습니다.

      </div>
      </td>
    </tr>
</table>



## Wrapping Up
여기까지 주간 레일즈였습니다! 여기 나열하기 힘들정도로 정말 많은 위대한 컨트리뷰션이 있었습니다. 자유롭게 <a href="https://github.com/rails/rails/compare/master@%7B2016-06-11%7D...@%7B2016-06-17%7D">확인해보세요!</a>

다음 주에 봐요!



