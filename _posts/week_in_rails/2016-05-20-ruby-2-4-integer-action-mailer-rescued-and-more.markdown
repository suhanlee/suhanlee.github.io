---
layout: post
title:  "[2016년 5월 20일] 주간 레일즈: Ruby 2.4 Integer, Action Mailer rescued and more!"
date:   2016-05-30 20:00:00 +0900
categories: rails
---

해당 글은 Godfrey Chan/godfreykfc@gmail.com 의 허락을 얻어서 번역한 글입니다.

원본 페이지: [https://rails-weekly.ongoodbits.com/2016/05/20/ruby-2-4-integer-action-mailer-rescued-and-more](https://rails-weekly.ongoodbits.com/2016/05/20/ruby-2-4-integer-action-mailer-rescued-and-more)


![](https://goodbits-production.s3.amazonaws.com/uploads/newsletter_settings/logo/225/db659964-7ac2-48f1-8ad5-3d4907dfafd6.png)

쉿, 조용히! 레일즈 5.0이 이제 얼마 안남았습니다.

레일즈 메인테이너, 컴퓨터 사이언티스트들의 커밋 DNA가 오래전부터 더 나은 웹 프레임웍을 만들기 위해서 머지 되고 있습니다. 마치 쥬라기 공원처럼.

여기 [Kasper Timm Hansen](https://twitter.com/kaspth)와 함께 자발적으로 봉사하는 공동 편집자인 [Jeff Goldblum](https://www.youtube.com/watch?v=lpuS7_NPv6U)에 이야기에 의하면 레일즈 팀은 할수 있는 것과 할수 없는 것에 대해서 계속 고민하고 있다고 합니다

*역자 주: 이번주 소개는 캐스퍼의 쥬라기 공원 조크인 것 같군요.

## Featured

<table>
  <tr>
    <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3388080/contributors.png"></td>
    <td><p><a href="http://contributors.rubyonrails.org/contributors/in-time-window/20160514-20160520">이번 주 레일즈 컨트리뷰터</a></p><br>
    <div>
    Contributors! Contributors! Contributors!<br>
    62개의 커밋, 22명의 사람들이 뉴스레터팀을 기쁘게 해주었네요.❤
    </div>
    </td>
  </tr>
  <tr>
    <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3388083/199.jpeg"></td>
    <td><p><a href="https://github.com/rails/rails/pull/25056">Fixnum + Bignum = Integer</a></p>
    <div>
    루비 2.4부터 Fixnum 과 Bignum이 Integer 로 통합 됩니다<br>
    걱정 안하셔도 되는게, 레일즈는 이전, 이후 버전에 대해서 <br>호환성을 전방위적으로 이미 준비되어 있습니다. Int' no Fix' too Big', folks.
    </div>
    </td>
  </tr>
  <tr>
    <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3388136/199-upside-down.jpeg"></td>
    <td><p><a href="https://github.com/rails/rails/pull/25018">Action Mailer: rescue_from and more</a></p>
    <div>
    액션 메일러는 이제 rescue_from 을 통해서<br> rescue 를 자체적으로 할 수 있습니다.<br>
    이것은 Active Job을 통해서 수행(delivering)할 때<br> 발생하는 에러에 대해서 핸들링 가능합니다.<br>
    세번째로, pull request 로 변경된 철저히 작성된 문서를 통해서<br> 예외상황에서 rescue_from 을 어떻게 사용할 수 있는지 확인할 수 있으니 읽어보세요.
    </div>
    </td>
  </tr>

</table>


## Fixed

<table>
  <tr>
    <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3388132/370323.jpeg"></td>
    <td><p><a href="https://github.com/rails/rails/pull/25009">Support nested calls to #suppress</a></p><br>
    <div>
    같은 클래스 안의 다른 suppression 상태에서<br> suppress를 부르는 경우 이전에는 실행이 중지 되었습니다<br>
    이제는 메서드가 이름을 취급할때 크게 주의하지 않기 때문에 괜찮습니다.
    </div>
    </td>
  </tr>
</table>

## Improved

<table>
  <tr>
    <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3388131/10308.jpeg"></td>
    <td><p><a href="https://github.com/rails/rails/pull/24203">Relation blocked Enumerable count</a></p><br>
    <div>
    레일즈 5.1의 첫 번째 피쳐중의 하나가<br> 적합하게 등장(forward)했네요.
    <br>
    문자 그대로 Relation 단계의 <b>count</b> 를<br> 만약 block 이 있을 경우 Enumerable 에게 전달합니다.<br><br>
    </div>
    </td>
  </tr>
  <tr>
      <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3388133/216.jpeg"></td>
      <td><p><a href="https://github.com/rails/rails/pull/24930">Time#all_day meet Date#all_day</a></p><br>
      <div>
      레일즈는 오랫동안 <b>Time#all_day</b>에 대해서 <br> 과거 시대의 지평을 확장해왔습니다. <br>이제 Date 도 동일하게 사용할 수 있습니다.
      <br>Though personally, dating for a whole day sounds pretty extreme,<br> but kids these days ¯_(ツ)_/¯
      </div>
      </td>
  </tr>
</table>

## Wrapping Up
위의 것 외에도 많은 것들이 있으니, 자유롭게 뛰어들어서 <a href="https://github.com/rails/rails/compare/master@%7B2016-05-14%7D...@%7B2016-05-20%7D">확인해보세요!</a>



