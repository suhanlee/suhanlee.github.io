---
layout: post
title:  "[2016년 6월 4일] 주간 레일즈: 사과드립니다, Initializer 변경사항 및 그 외!"
date:   2016-06-04 20:00:00 +0900
categories: Rails
---

해당 글은 Godfrey Chan/godfreykfc@gmail.com 의 허락을 얻어서 번역한 글입니다.

원본 페이지: [https://rails-weekly.ongoodbits.com/2016/06/04/an-apology-initializer-changes-and-more](https://rails-weekly.ongoodbits.com/2016/06/04/an-apology-initializer-changes-and-more)


![](https://goodbits-production.s3.amazonaws.com/uploads/newsletter_settings/logo/225/db659964-7ac2-48f1-8ad5-3d4907dfafd6.png)

Hey team, 여기 [토드(Todd)](https://twitter.com/toddbealmear)가 다른 버전의 레일즈를 가져왔습니다! 불행히도 일정을 맞추지 못해서, 우리는 지난 주를 지나쳤습니다.

우리는 빠진 주를 다루지 않을 것이지만, 그 기간 동안의 [repo activity](https://github.com/rails/rails/compare/master@%7B2016-05-20%7D...@%7B2016-05-28%7D) 를 확인하는 것을 권장합니다.

아무튼, 이번 주 흥미진진한 내용을 보시죠!


## Featured


<table>
  <tr>
      <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3434306/twir_120.png"></td>
      <td><p><a href="http://suhanlee.github.io/2016/ruby-2-4-integer-action-mailer-rescued-and-more.html">주간 레일즈는 한국어로 번역됩니다</a></p><br>
      <div>
      첫번째로 <a href="http://suhanlee.github.io/about/">Suhan Lee</a>에게 우리 뉴스레터를<br>한국어로 번역하는 것에 감사드립니다!
      <br>그는 매주 뉴스레터를 번역하기로 한다고 하네요. <br>만약 추가로 번역 하는데 관심 있으면 <a href="https://twitter.com/chancancode">Godfrey</a>에게 연락바랍니다. 
      </div>
      </td>
  </tr>
  <tr>
    <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3388080/contributors.png"></td>
    <td><p><a href="http://contributors.rubyonrails.org/contributors/in-time-window/20160528-20160603">이번 주 레일즈 컨트리뷰터</a></p><br>
    <div>
    이번 주에 25개의 사랑스러운 컨트리뷰트 코드와 문서작업이 있었네요.<br>
    You all rock! Keep the contributions coming!
    </div>
    </td>
  </tr>
</table>



## New Stuff


<table>
    <tr>
        <td>
        <img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3434400/621238.jpeg">
        </td>
        <td><p><a href="https://github.com/rails/rails/pull/25231">Default Initializers 설정이 1개의 파일로 줄여집니다</a></p><br>
            <div>
            레일즈 5 후반에 추가된 Default Initializers 설정이<br> 더이상 각각의 파일에 있지 않게 됩니다.<br> 그 대신
            새로운 new_framework_defaults.rb 파일이 사용됩니다.
            </div>
        </td>       
    </tr>
</table>


## Improved


<table>
  <tr>
    <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3434397/3020626.png"></td>
    <td><p><a href="https://github.com/rails/rails/pull/25170">Action Cable에 WebSocket, logger 설정 옵션 추가 </a></p><br>
    <div>
   이제 액션 케이블을 사용할때 <br>WebSocket 과 logger 옵션을 설정할 수 있습니다.
    </div>
    </td>
  </tr>
  <tr>
      <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3434398/1529387.jpeg"></td>
      <td><p><a href="https://github.com/rails/rails/commit/c4cb6862babd2665a65056e205c2a5fd17a5d99d">Active Record YAML Dumps 크기 줄임.</a></p><br>
      <div>
      이 패치는 YAML로 덤프되는 모델의 크기를 줄여줍니다.<br>
      -특정 케이스에서는 말도 안되지만 80%까지 줄여줍니다!
      </div>
      </td>
  </tr>
</table>


## Fixed

<table>
  <tr>
    <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3434308/1529387.jpeg"></td>
    <td><p><a href="https://github.com/rails/rails/commit/02da8aea832485044fde1b94c021a66d37d54dec">#exists? 에서 #includes 로 체이닝 이슈 해결(Fixed)</a></p><br>
    <div>
    Sean은 #includes 에 #exists? 를 체이닝(chaining) 해서
    <br>이슈에 대해서 수정할 수 있었다고 합니다.
    <br>마치 아직 일이 많이 남아 있을 것 같이 들립니다.
    <br>그래서 저는 커밋 메시지를 보는 것을 권장합니다. 
    </div>
    </td>
  </tr>
  <tr>
      <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3434308/1529387.jpeg"></td>
      <td><p><a href="https://github.com/rails/rails/commit/c8be4574a2a35c896560ff58b26111ad6dd9d60f">여러 클래스 사이에서 ActiveRecord::Base#hash가  달라야 함</a></p><br>
      <div>
      이전에는 2개의 다른 모델 클래스에서 해시를 사용할때<br> 아이디(ID)가 동일한 경우 해시가 충돌하는 이슈가 있었습니다
      <br>지금은 master 브랜치에서 수정되었습니다.
      </div>
      </td>
    </tr>
     <tr>
          <td><img src="https://goodbits-production.s3.amazonaws.com/uploads/link/thumbnail/3434307/567626.jpeg"></td>
          <td><p><a href="https://github.com/rails/rails/pull/25194">OpenSSL Deprecation 경고 제거</a></p><br>
          <div>
          이 패치는 OpenSSL::Cipher::Cipher 이름공간(namespace)이 
          <br>OpenSSL::Cipher로 단순하게 변경됨에 따라서 발생하는
          <br>deprecation 경고를 없애기 위한 것입니다.
          </div>
          </td>
        </tr>
</table>



## Wrapping Up
위의 것 외에도 많은 것들이 있으니, 자유롭게 뛰어들어서 <a href="https://github.com/rails/rails/compare/master@%7B2016-05-14%7D...@%7B2016-05-20%7D">확인해보세요!</a>

다음 주에 봐요!



