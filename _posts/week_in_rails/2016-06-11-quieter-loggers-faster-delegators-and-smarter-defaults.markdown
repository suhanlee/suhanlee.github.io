---
layout: post
title:  "[2016년 6월 11일] 주간 레일즈: quieter loggers, faster delegators, and smarter defaults!"
date:   2016-06-11 20:00:00 +0900
categories: rails
---

해당 글은 Godfrey Chan/godfreykfc@gmail.com 의 허락을 얻어서 번역한 글입니다.

원본 페이지: [https://rails-weekly.ongoodbits.com/2016/06/11/quieter-loggers-faster-delegators-and-smarter-defaults](https://rails-weekly.ongoodbits.com/2016/06/11/quieter-loggers-faster-delegators-and-smarter-defaults)


Ahoy hoy! [Tim](https://twitter.com/imtayadeway) (aka Godfrey[9]) 여기 최신 소식을 레일즈 커뮤니티에 가져왔습니다.

 여전히 다가오는 레일즈5 런칭을 위해 지속적으로 정제(refine) 활동의 또 다른 한주였습니다.
  Don't touch that dial - it's gonna be a helluva show!


## Featured


<table>
  <tr>
      <td class="author-profile"><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3434306/twir_120.png"></td>
      <td><p><a href="http://contributors.rubyonrails.org/contributors/in-time-window/20160604-20160610">이번주 레일즈 컨트리뷰터</a></p><br>
      <div>
        이번주는 26명의 놀라운 사람들을 보았네요. 그 중 3명은 레일즈에 첫 커밋 머지(merged)를 했습니다. 모두에게 큰 감사드립니다!
        다음 주에 참여하는 자신을 생각한다면, 현재 최신 이슈들을 살펴보지 않을까요? 문서화 작업에 기여하는 것도 중요한 시작일 수 있죠!
      </div>
      </td>
  </tr>
  <tr>
    <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3457950/maxresdefault.jpg"></td>
    <td><p><a href="http://contributors.rubyonrails.org/contributors/in-time-window/20160528-20160603">Rails 5: 투어(Tour)</a></p><br>
    <div>
    지난 주 소식 중 레일즈를 누구도 아닌 <a href="https://twitter.com/dhh">DHH</a>가 살펴보는 것이 있었습니다.
     이 비디오는 신규로 추가되는 기능 중에서 가장 강력한 기능의 튜토리얼(블로그를 20분안에 만들기)의 휠윈드 투어(whirlwind tour)를 다룹니다.
      또한 기존 루비온 레일즈 어플리케이션에 신규 기능을 추가하려는 사람들을 위해 도움이 될 것입니다.
    </div>
    </td>
  </tr>
</table>


## Fixed


<table>
  <tr>
    <td class="author-profile"><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3457536/43621.png"></td>
    <td><p><a href="https://github.com/rails/rails/pull/25079">before_action 에서 #send_file 메서드를 사용할 때 콜백 주기(callback cycle)을 종료할 수 있게 수정  </a></p><br>
    <div>
   최근 #send_file 메서드를 before_action 에서 사용할 때 리퀘스트 주기(request cycle)을 종료하는 것이 실패하는 경우가 있었습니다.
   이것은 콜백 종료자(callback terminator)가 @_response_body 를 확인할 때, #send_file 메서드에서 더 이상 설정되어 있지 않기 때문입니다.
   이것은 #performed? 메서드를 대신해서 사용하는 것으로 수정되었습니다.
    </div>
    </td>
  </tr>
  <tr>
      <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3457539/84159.jpeg"></td>
      <td><p><a href="https://github.com/rails/rails/pull/25341">모든 로그에 대해서 #silence로 전달(Broadcasting) 조절</a></p><br>
      <div>
      만약 동시에 여러 로거에 메시지를 전달(Broadcasting)하고 있다면, 발송후에도, 모두다 침묵(silence)하지 않음을 발견할 수 있을 겁니다.
       이 수정은 #silence 메서드를 위임(delegate)하는 것으로 수정되었습니다.
      </div>
      </td>
  </tr>
</table>


## Improved

<table>
  <tr>
    <td class="author-profile"><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3457537/19192189.png"></td>
    <td><p><a href="https://github.com/rails/rails/pull/25352">델리게이션(Delegation)이 Kernel#caller_locations 메서드로 빨라졌습니다.</a></p><br>
    <div>
    Kernel#caller_locations 는 MRI 2.0 에서 소개되었습니다 그리고 Module#delegate 메서드 안에서 Kernel#caller 메서드보다 10% 근접으로 빠릅니다.
    이것으로 저자는 부트 타임이 빨라진 결과를 얻었다고 합니다.
    </div>
    </td>
  </tr>
  <tr>
      <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3457538/621238.jpeg"></td>
      <td><p><a href="https://github.com/rails/rails/pull/25235">프레임웍 기본설정 파일들 대청소(spring clean)</a></p><br>
      <div>
      이번 주 새로운 프레임웍 기본설정 파일들이 쌓여 있었고, 기존 앱에서 새로운 설정값으로 업그레이드를 하는 좋은 조언을 비롯한 문서 업데이트가 있었습니다.
      이 변경사항은 또한 여러 곳에 있는 업데이트 플래그를 업그레이드 하는 과정에서 더 나은 메시지를 제공하기 위한 용도로 활용하기 위함입니다
      </div>
      </td>
    </tr>
</table>



## Wrapping Up
여기까지 주간 레일즈였습니다! 여기 나열하기 힘들정도로 정말 많은 위대한 컨트리뷰션이 있었습니다. 자유롭게 <a href="https://github.com/rails/rails/compare/master@%7B2016-06-04%7D...@%7B2016-06-10%7D">확인해보세요!</a>

다음 주에 봐요!



