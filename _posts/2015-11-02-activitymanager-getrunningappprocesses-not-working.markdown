---
layout: post
title:  "[Android M] ActivityManager getRunningAppProcesses not woking"
date:   2015-11-02 02:03:00 +0900
categories: Android
---
아래와 같이 pid 값을 가지고 현재 실행중인 프로세스 정보를 가져오려고 할때 Android M 이전 OS 에서는 다른 앱 프로세스 정보를 가져올 수 있으나
Android M OS 에서는 자신의 프로세스 외에는 정보를 가져오지 못하는 것을 확인했다.

추가적으로 기존에 getRunningAppProcess 는 4.4 이하 버전과 5.0 이후의 동작에 대해서 안드로이드 구조적인 변경사항이 있다.
이 글은 어디까지나 시스템 권한이 있을 경우에서 발생한 특정 이슈와 관련된 이야기며
특정 제조사의 5.1 바이너리에서 재현된 것을 밝힌다.

{% highlight java %}
public static String getAppPackageNameByPID(Context context, int pid) {
    ActivityManager manager = (ActivityManager) context.getSystemService(Context.ACTIVITY_SERVICE);

    if (manager != null) {
        List<ActivityManager.RunningAppProcessInfo> runningProcessList =  manager.getRunningAppProcesses();
        if (runningProcessList != null) {
            for (ActivityManager.RunningAppProcessInfo processInfo : runningProcessList) {
                if (processInfo.pid == pid) {
                    return processInfo.processName;
                }
            }
        }
    }

    return "";
}
{% endhighlight %}

해당 사항은 아래 게시물처럼 Android M Preview 에서 이슈가 나왔던 부분인 것 같다.

[https://code.google.com/p/android-developer-preview/issues/detail?id=2347]

AndroidManifest 에 `<uses-permission android:name="android.permission.REAL_GET_TASKS" />`
 퍼미션을 추가해서 이슈가 해결되었다.

또한 GET_TASKS 퍼미션은 M 에서 deprecated 되었다.

[https://code.google.com/p/android-developer-preview/issues/detail?id=2347]:https://code.google.com/p/android-developer-preview/issues/detail?id=2347