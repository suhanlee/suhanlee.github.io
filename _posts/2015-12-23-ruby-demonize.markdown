---
layout: post
title:  "[Ruby] Demon Process 만들기"
date:   2015-12-23 02:00:00 +0900
categories: Ruby
---

Ruby에서 Demon Process(데몬 프로세스) 만들기
-------------------

Ruby 에서 Demon Process 를 만드는 과정에 대한 글을 작성한다.
 
Demon Process는 일반적으로 백그라운드로 동작하며, 직접 사용자와의 인터럭션을 가지지 않는다.

프로세스가 Demon Process로 동작하기 위해서는 다음과 같은 과정이 필요하다.

1. 부모 프로세스가 종료할 수 있도록 포크(fork)한다.

2. 운영체제의 setsid 함수를 호출하여 어떤 터미널이나 쉘과도 분리된 새로운 세션을 생성한다.
* 세션을 생성하고 Process-Group-ID 를 설정한다.

3. 프로세스를 다시 포크(fork)하여 프로세스가 완전히 스스로 제어권을 가지는 고아(Orphan) 프로세스가 되게 한다.

4. 워킹 디렉토리를 루트 디렉토리로 변경하여 프로세스가 현재 워킹 디렉토리의 삭제를 막지 못하게 한다.

5. 표준 출력(STDIN), 표준 입력(STDOUT), 표준 오류(STDERR) 파일 핸들을 /dev/null 로 지정한다.

6. TERM 시그널(signal)을 캐치하는 핸들러를 설치하여 요구사항이 있을 때 프로세스가 종료될 수 있게 한다.

Linux/Unix에서 Ruby로 구현한 예시는 아래와 같다.

* 윈도우즈는 데몬이 아닌 서비스 개념이라 다른 방법을 찾아봐야 할 것 같다.

{% highlight ruby %}
def demonize
    fork do         
        Process.setsid
        exit if fork # 프로세스가 부모면 종료한다.
        Dir.chdir('/')
        STDIN.reopen('/dev/null')
        STDOUT.reopen('/dev/null', 'a')
        STDERR.reopen('/dev/null', 'a')
        trap('TERM') { exit }
        yield
    end
end

demonize do
    # background job
    
end
{% endhighlight %}
