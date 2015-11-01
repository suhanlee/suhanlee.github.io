---
layout: post
title:  "[Android] ActivityManager 를 이용해서 Foreground Service 만들기"
date:   2015-11-02 02:50:00 +0900
categories: android
---

일반적으로 화면을 가지고 있지 않는 서비스는 포그라운드 프로세스(Foreground Process)가 될 수 없다. 

하지만 ActivityManager는 프로세스를 관리하고, 앱별로 포그라운드, 백그라운드 제어할 수 있는 설비를 갖추고 있다.
 
아래와 같이 ActivityManagerNative의 setProcessForeground 메서드를 이용하면 가능하다.

> 참고로 ActivityManagerNative 는 hide 클래스이기 때문에 컴파일시에 참조 되지 않지만 리플렉션을 이용해서 런타임시에 참조가 가능하다.

{% highlight java %}
/*
* android.app.ActivityManagerNative.java(@hide class)
*/
IActivityManage rmAm = ActivityManagerNative.getDefault();
mAm.setProcessForeground(IBinder token, int pid, boolean isForeground)
{% endhighlight %}

{% highlight java %}
public void setProcessForeground(IBinder token, int pid,
            boolean isForeground) throws RemoteException {
        Parcel data = Parcel.obtain();
        Parcel reply = Parcel.obtain();
        data.writeInterfaceToken(IActivityManager.descriptor);
        data.writeStrongBinder(token);
        data.writeInt(pid);
        data.writeInt(isForeground ? 1 : 0);
        mRemote.transact(SET_PROCESS_FOREGROUND_TRANSACTION, data, reply, 0);
        reply.readException();
        data.recycle();
        reply.recycle();
    }
{% endhighlight %}

Binder를 이용해서 ActivityManager Service 로 콜을 하는 것을 볼 수 있다.

구현체에서 보면 알 수 있듯이 이 메서드를 사용하기 위해서는 SET_PROCESS_LIMIT 가 허용이 되어야 하는데

해당 메서드는 일반 앱에서는 허용되지 않는다. platform key로 앱 서명할 경우 사용 가능하다.
 
 `"android.permission.SET_PROCESS_LIMIT"
 Allows an application to set the maximum number of (not needed) application processes that can be running.
 Not for use by third-party applications.
 Constant Value: "android.permission.SET_PROCESS_LIMIT"`


{% highlight java %}
/*
* com.android.server.am.ActivityManagerService.java
*/
@Override
    public void setProcessForeground(IBinder token, int pid, boolean isForeground) {
        enforceCallingPermission(android.Manifest.permission.SET_PROCESS_LIMIT,
                "setProcessForeground()");
        synchronized(this) {
            boolean changed = false;

            synchronized (mPidsSelfLocked) {
                ProcessRecord pr = mPidsSelfLocked.get(pid);
                if (pr == null && isForeground) {
                    Slog.w(TAG, "setProcessForeground called on unknown pid: " + pid);
                    return;
                }
                ForegroundToken oldToken = mForegroundProcesses.get(pid);
                if (oldToken != null) {
                    oldToken.token.unlinkToDeath(oldToken, 0);
                    mForegroundProcesses.remove(pid);
                    if (pr != null) {
                        pr.forcingToForeground = null;
                    }
                    changed = true;
                }
                if (isForeground && token != null) {
                    ForegroundToken newToken = new ForegroundToken() {
                        @Override
                        public void binderDied() {
                            foregroundTokenDied(this);
                        }
                    };
                    newToken.pid = pid;
                    newToken.token = token;
                    try {
                        token.linkToDeath(newToken, 0);
                        mForegroundProcesses.put(pid, newToken);
                        pr.forcingToForeground = token;
                        changed = true;
                    } catch (RemoteException e) {
                        // If the process died while doing this, we will later
                        // do the cleanup with the process death link.
                    }
                }
            }

            if (changed) {
                updateOomAdjLocked();
            }
        }
    }
{% endhighlight %}