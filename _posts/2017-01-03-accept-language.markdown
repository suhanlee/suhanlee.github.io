---
layout: post
title:  "[dev] 날짜, 시간 관련 다국어 대응"
date:   2017-01-02 01:00:00 +0900
categories: dev

---
최근에 개발하면서 미약하지만 개선시켰다고 느꼈던 점을 요약해서 공유합니다.
---------------------------------------------------

과거 글로벌 버전의 앱을 만들면서 여러 언어를 지원하다 보니 클라이언트에서 처리 해야 하는 표기법 같은 것이 달라서
클라이언트가 문자열 처리를 해서 보여줘야 하는 경우가 왕왕 있었다.

예를 들자면 단말의 영어면 2시간 전을 `"2 hours ago"` 한국어면 `"2시간 전"` 이라고 표기하는 것이다.
주로 원본 데이터가 날짜나 시간과 관련된 경우가 많고 언어별로 표기하는 형식이 달라서 작업해야 하는 경우가 많았다.

문제는 이런 것이 한 두개가 아니라는 점, 귀찮기도 하고 번잡하기도 하다.

파이썬이나 루비 같은 경우 좀 더 쉽게 문자열 처리를 할 수 있기 때문이라는 것도 한몫을 한 것 같다.

이번에 내가 주로 하는 일은 서버이고 좀 더 효과적인 방법을 찾다 보니
클라이언트가 서버에게 요청하는 헤더에 브라우저가 요청하는 것처럼 "Accept-Language" 필드를 활용하는 방법으로 구현을 했다.

원리는 클라이언트가 서버에게 웹 요청을 할 때 "Accept-Language" 필드에 언어 값 예를 들어 "ko" 를 요청하면
서버가 클라이언트의 언어에 맞춰서 출력 스트링을 변경한다.

안드로이드 클라이언트의 경우 Retrofit 을 사용한다면 여러가지 방법이 있지만 OkHttpClient 빌더에 인터셉터를 추가해서 구현 가능하다.
{% highlight java %}
private final static OkHttpClient okHttpClient = new OkHttpClient.Builder()
            ...
            .addInterceptor(new Interceptor() {
                @Override
                public Response intercept(Chain chain) throws IOException {
                    Request original = chain.request();

                    Request request = original.newBuilder()
                            .header("Accept-Language",  Locale.getDefault().getLanguage())
                            .build();

                    return chain.proceed(request);
                }
            })
            .build();
{% endhighlight %}

서버의 경우 레일즈를 사용한다면 i18n 젬이 잘 적용 되어 있어서 언어별 yml 파일과 _T 함수면 간단히 사용하면 된다.


