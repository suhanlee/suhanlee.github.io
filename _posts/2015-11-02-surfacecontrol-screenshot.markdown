---
layout: post
title:  "[Android] SurfaceControl 을 이용해서 화면 Screenshot"
date:   2015-11-02 03:36:00 +0900
categories: Android
---

안드로이드에서 SurfaceControl 의 screenshot 메서드를 이용해서 아래와 같이 호출 뒤 리턴값으로 Bitmap 정보를 받을 수 있다.

> SurfaceControl 은 hide 클래스이기 때문에 일반적으로 런타임시에 리플렉션으로 호출한다.

> SurfaceControl 메서드를 따라가다보면 결국 Surface Flinger 쪽의 메서드를 호출 하게 되는데 결국 `android.permission.READ_FRAME_BUFFER` 퍼미션이 필요하다.

> 위 퍼미션은 일반 third-party 앱은 가질 수 없다.

{% highlight java %}

// Take the screenshot
mScreenBitmap = SurfaceControl.screenshot((int) dims[0], (int) dims[1]);
  if (mScreenBitmap == null) {
      notifyScreenshotError(mContext, mNotificationManager);
      finisher.run();
      return;
}
...

 /**
     * Like {@link SurfaceControl#screenshot(int, int, int, int, boolean)} but
     * includes all Surfaces in the screenshot.
     *
     * @param width The desired width of the returned bitmap; the raw
     * screen will be scaled down to this size.
     * @param height The desired height of the returned bitmap; the raw
     * screen will be scaled down to this size.
     * @return Returns a Bitmap containing the screen contents, or null
     * if an error occurs. Make sure to call Bitmap.recycle() as soon as
     * possible, once its content is not needed anymore.
     */
    public static Bitmap screenshot(int width, int height) {
        // TODO: should take the display as a parameter
        IBinder displayToken = SurfaceControl.getBuiltInDisplay(
                SurfaceControl.BUILT_IN_DISPLAY_ID_MAIN);
        return nativeScreenshot(displayToken, new Rect(), width, height, 0, 0, true,
                false, Surface.ROTATION_0);
    }
{% endhighlight %}

> `SurfaceControl.BUILT_IN_DISPLAY_ID_MAIN`은 화면을 뜻한다.

nativeScreenshot 메서드는 인자로 범위를 비롯해서 레이어 범위 등을 받는데 레이어를 분리해서 캡쳐할 수도 있다.
