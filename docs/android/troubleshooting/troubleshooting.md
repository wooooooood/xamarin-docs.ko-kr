---
title: "문제 해결 팁"
ms.topic: article
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 145c8507ca5ebea6197fa8827b93f58fbc9bb078
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="troubleshooting-tips"></a>문제 해결 팁


## <a name="getting-diagnostic-information"></a>진단 정보 가져오기

Xamarin.Android에 다양 한 버그를 추적 하는 때를 찾을 몇 개의 위치가 있습니다.
여기에는 다음이 포함됩니다.

1.  진단 MSBuild 출력 합니다.
2.  장치 배포 로그 합니다.
3.  Android 디버그 로그 출력입니다.


<a name="Diagnostic_MSBuild_Output" />

## <a name="diagnostic-msbuild-output"></a>진단 MSBuild 출력

진단 MSBuild 패키지 빌드에 관련 된 추가 정보를 포함할 수 있습니다 및 일부 패키지 배포 정보를 포함 될 수 있습니다.

Visual Studio 내에서 진단 MSBuild 출력을 사용하려면:

1.  클릭 **도구 > 옵션...**
2.  왼쪽 트리 뷰에서 선택 **프로젝트 및 솔루션 > 빌드 및 실행**
3.  오른쪽 패널에 MSBuild 빌드 출력의 자세한 정도 드롭다운 진단으로 설정
4.  **확인**을 클릭합니다.
5.  패키지를 지우고 다시 빌드합니다.
6.  진단 출력을 출력 패널에 표시 됩니다.


Mac/OS x:에 대 한 진단 출력을 Visual Studio에서 MSBuild를 사용 하도록 설정 하려면

1.  클릭 **Mac 용 Visual Studio > 기본 설정 중...**
2.  왼쪽 트리 뷰에서 선택 **프로젝트 > 빌드**
3.  오른쪽 패널에는 로그의 자세한 정도 드롭 다운으로 설정 진단
4.  **확인**을 클릭합니다.
5.  Mac용 Visual Studio 다시 시작
6.  패키지를 지우고 다시 빌드합니다.
7.  진단 출력 오류 패드 안에서 표시 되는 (**보기 > 패드 > 오류** ), 빌드 출력 단추를 클릭 하 여 합니다.




## <a name="device-deployment-logs"></a>장치 배포 로그

Visual Studio 내에서 장치 배포 로깅을 설정 하려면

1.  **도구 > 옵션...**>
2.  왼쪽 트리 뷰에서 선택 **Xamarin > Android 설정**
3.  오른쪽 패널에서 [X]를 사용 하도록 설정 **(바탕 화면에 monodroid.log 쓰기) 확장 디버그 로깅을** 확인란 합니다.
4.  로그 메시지는 바탕 화면에 monodroid.log 파일에 기록 됩니다.


Mac 용 visual Studio는 항상 장치 배포 로그를 기록합니다. 찾아서 병렬화가 약간 더 어렵습니다. *AndroidUtils* 모든 일 + 예를 들어 배포 발생 하는 시간에 대 한 로그 파일을 만듭니다: **AndroidTools-2012-10-24_12-35-45.log**합니다.

-  Windows에서 로그 파일에 기록 됩니다 `%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`합니다.
-  OS X에서 로그 파일에 기록 됩니다 `$HOME/Library/Logs/XamarinStudio-{VERSION}`합니다.




## <a name="android-debug-log-output"></a>Android 디버그 로그 출력

Android에는 많은 메시지를 작성 합니다는 [Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md)합니다.
Xamarin.Android Android 시스템 등록 정보를 사용 하 여 Android 디버그 로그에 추가 메시지를 생성을 제어 합니다. 통해 android 시스템 속성을 설정할 수는 *setprop* 내에서 명령을 [Android 디버그 브리지 (adb)](http://developer.android.com/guide/developing/tools/adb.html):

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

시스템 속성 프로세스 시작 하는 동안 읽고 응용 프로그램이 시작 되거나 시스템 속성 변경 된 후 응용 프로그램을 다시 시작 해야 하기 전에 설정 되어야 합니다.



### <a name="xamarinandroid-system-properties"></a>Xamarin.Android 시스템 속성

Xamarin.Android 다음 시스템 속성을 지원합니다.

-   *debug.mono.debug*:이에 해당 하는 경우 비어 있지 않은 문자열 `*mono-debug*`합니다.

-   *debug.mono.env*: 파이프 구분 된 ('*|*') 응용 프로그램 시작 중 내보낼 환경 변수 목록을 *전에* 모노 활성화 되었습니다. 그러면 컨트롤 모노 로깅이 환경 변수를 설정 합니다.

    - *참고*: 값 이므로 '*|*'-으로 구분 하 여 값이 있어야 추가 수준의 인용 부호를로 \` *adb 셸* \` 명령을 제거 합니다는 따옴표의 집합입니다.

    - *참고*: Android 시스템 속성 값 길이가 92 자까지 될 수 없습니다.

    - 예제:

            adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"

-   *debug.mono.log*: 쉼표로 구분 된 ('*,*')에서 Android 디버그 로그에 추가 메시지를 인쇄 하도록 하는 구성 요소 목록입니다. 기본적으로 아무 것도 설정 됩니다. 구성 요소는 다음과 같습니다.

    -   *모든*: 모든 메시지를 인쇄 합니다.
    -   *gc*: 인쇄 GC 관련 메시지입니다.
    -   *gref*: 참조 (약한, 글로벌) 할당 및 할당 취소 메시지를 인쇄 합니다.
    -   *lref*: 지역 참조 할당 및 할당 취소 메시지를 인쇄 합니다.

    *참고*: 이들은 *매우* 자세한 정보 표시 합니다. 필요한 경우에 실제로 사용 하지 마십시오.

-   *debug.mono.trace*:을 설정할 수 있습니다는 [모노-추적](http://docs.go-mono.com/?link=man%3amono(1)) `=PROPERTY_VALUE` 설정 합니다.



## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin.Android cannot resolve System.ValueTuple

이 오류는 Visual Studio와의 비 호환성으로 인해 발생합니다.

- **Visual Studio 2017 업데이트 1** (15.1 또는 이전 버전) 호환 되는 **System.ValueTuple NuGet 4.3.0** (또는 이전 버전).

- **Visual Studio 2017 업데이트 2** (15.2 또는 최신 버전) 호환 되는 **System.ValueTuple NuGet 4.3.1** (이상)입니다.

Visual Studio 2017 설치와 일치 하는 올바른 System.ValueTuple NuGet을 선택 하십시오.


## <a name="gc-messages"></a>GC 메시지

Gc를 포함 하는 값을 debug.mono.log 시스템 속성을 설정 하 여 GC 구성 요소 메시지를 볼 수 있습니다.

GC 메시지 GC 실행 하 고 않은 GC를 작동 크기에 대 한 정보를 제공 될 때마다 생성 됩니다.

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

타이밍 정보 등의 추가 GC 정보를 설정 하 여 생성할 수 있습니다는 `MONO_LOG_LEVEL` 환경 변수를 `debug`:

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

이 경우 (많은) 추가 모노 메시지를 결과의이 세 가지를 포함 하 여 발생 합니다.

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

에 `GC_BRIDGE` 메시지 `num-objects` 이 단계를 고려 하는 연결 개체의 수 및 `num_hash_entries` 브리지 코드의이 호출 하는 동안 처리 하는 개체 수입니다.

에 `GC_MINOR` 및 `GC_MAJOR` 메시지, `total` 세계 일시 중지 된 동안 시간의 양입니다 (스레드가 실행 중), 동안 `bridge` 의 브리지 처리 코드 (Java VM 처리) 하는 데 걸린 시간입니다. 세계는 *하지* 브리지 처리 될 때 일시 중지 합니다.

 *일반적*의 값이 클수록 `num_hash_entries`, 하는 많은 시간이 `bridge` 컬렉션 사용 하는 및 더 큰 숫자는 `total` 수집 하는 데 걸린 시간 사용 됩니다.



## <a name="global-reference-messages"></a>전역 참조 메시지

로깅, 전역 참조 loggig (GREF)를 사용 하도록 설정 하려면는 *debug.mono.log* 시스템을 포함 해야 *gref*, 예::

```shell
adb shell setprop debug.mono.log gref
```

Xamarin.Android Android 전역 참조를 사용 하 여 Java 인스턴스 java 제공 해야 하는 Java 메서드를 호출할 때으로 Java 인스턴스 및 연결 된 관리 되는 인스턴스 간의 매핑을 제공.

안타깝게도, Android 에뮬레이터 2000 글로벌 참조에서 한 번에 존재할 수 있습니다. 하드웨어에는 훨씬 더 높은 한도가 52000 전역 참조 합니다. 하한값 되기 때문에 에뮬레이터에서 응용 프로그램을 실행 하는 경우 문제가 될 수 있습니다 *여기서* 인스턴스 준 매우 유용할 수 있습니다.

 *참고*: 전역 참조 횟수는 Xamarin.Android, 내부 및 하지 않습니다 (및 수 없습니다) 프로세스에 로드 하는 다른 네이티브 라이브러리 수행한 전역 참조를 포함 합니다. 전역 참조 횟수 예측으로 사용 합니다.

```shell
I/monodroid-gref(12405): +g+ grefc 108 gwrefc 0 obj-handle 0x40517468/L -> new-handle 0x40517468/L from    at Java.Lang.Object.RegisterInstance(IJavaObject instance, IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object.SetHandle(IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object..ctor(IntPtr handle, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler, Boolean removable)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler)
I/monodroid-gref(12405):    at Android.App.Activity.RunOnUiThread(System.Action action)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.UseLotsOfMemory(Android.Widget.TextView textview)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.<OnCreate>m__3(System.Object o)
I/monodroid-gref(12405): handle 0x40517468; key_handle 0x40517468: Java Type: `mono/java/lang/RunnableImplementor`; MCW type: `Java.Lang.Thread+RunnableImplementor`
I/monodroid-gref(12405): Disposing handle 0x40517468
I/monodroid-gref(12405): -g- grefc 107 gwrefc 0 handle 0x40517468/L from    at Java.Lang.Object.Dispose(System.Object instance, IntPtr handle, IntPtr key_handle, JObjectRefType handle_type)
I/monodroid-gref(12405):    at Java.Lang.Object.Dispose()
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor.Run()
I/monodroid-gref(12405):    at Java.Lang.IRunnableInvoker.n_Run(IntPtr jnienv, IntPtr native__this)
I/monodroid-gref(12405):    at System.Object.c200fe6f-ac33-441b-a3a0-47659e3f6750(IntPtr , IntPtr )
I/monodroid-gref(27679): +w+ grefc 1916 gwrefc 296 obj-handle 0x406b2b98/G -> new-handle 0xde68f4bf/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1915 gwrefc 294 handle 0xde691aaf/W from take_global_ref_jni
```

결과의 네 개의 메시지가 있습니다.

-  전역 참조 만들기:로 시작 하는 줄은 이러한 *+ g +* 를 만드는 코드 경로 대 한 스택 추적을 제공 합니다.
-  전역 참조 소멸:로 시작 하는 줄은 이러한 *-g-* , 그리고 전역 참조를 삭제 하는 코드 경로 대 한 스택 추적을 제공할 수 있습니다. GC는 gref 삭제, 스택 추적이 없습니다 제공 됩니다.
-  전역 약한 참조 만들기:로 시작 하는 줄은 이러한 *+ w +* 합니다.
-  전역 약한 참조 소멸:로 시작 하는 줄 이들은 *-w-* 합니다.


모든 메시지에는 *grefc* 값은 만들어진 Xamarin.Android 전역 참조의 개수 동안는 *grefwc* 값 Xamarin.Android 만들어진 글로벌 약한 참조의 수입니다. *처리* 또는 *obj 핸들* 값 JNI 핸들 값 이며 다음 문자는 '  */* ' 핸들 값의 유형: */L* 로컬 참조에 대 한 */G* 전역 참조에 대 한 및 */W* 약한 전역 참조에 대 한 합니다.

GC 프로세스의 일환으로, 전역 참조 (+ g +)로 변환 하는 전역 약한 참조 (a + w + 발생 하 고-g-), Java 측 GC이 시작 및 수집 된 확인 약한 전역 참조 확인 합니다. 여전히 활성 경우 새 gref 약한 ref 주위 만들어집니다 (+ g +,-w-), 약한 ref 그렇지 않은 경우 제거 됩니다 (-w).

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Java 인스턴스를 만들고는 MCW 래핑된

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>GC는 수행 하 고...

```shell
I/monodroid-gref(27679): +w+ grefc 1953 gwrefc 259 obj-handle 0x4066df10/G -> new-handle 0xde68f95f/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -g- grefc 1952 gwrefc 259 handle 0x4066df10/G from take_weak_global_ref_jni
```

## <a name="object-is-still-alive-as-handle--null"></a>개체는 여전히 활성 핸들로! = null
## <a name="wref-turned-back-into-a-gref"></a>wref는 gref로 다시 설정

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x4066df10
I/monodroid-gref(27679): +g+ grefc 1930 gwrefc 39 obj-handle 0xde68f95f/W -> new-handle 0x4066df10/G from take_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1930 gwrefc 38 handle 0xde68f95f/W from take_global_ref_jni
```

## <a name="object-is-dead-as-handle--null"></a>개체가 실행 되는지, 핸들로 null = =
## <a name="wref-is-freed-no-new-gref-created"></a>wref가 해제 된, 새 gref 생성

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

하나의 "흥미로운" 주름: 4.0 이전 Android를 실행 하는 대상 gref 값은 Android 런타임 메모리의 Java 개체의 주소와 같습니다. (즉, GC 움직이지 않는, 보수적, 수집기에는 및 해당 개체에 대 한 참조를 직접 전달 됩니다.) 따라서 이후에 + g + + w +,-g-, + g +,-w-시퀀스, 결과 gref 원래 gref 값과 같은 값을 갖습니다. 따라서 로그 grepping 매우 간단 합니다.

그러나 Android 4.0 이동 수집기 않았으며 VM 개체를 Android 런타임에 대 한 직접 참조 더 이상 전달 됩니다. 따라서 후는 + g + + w +,-g-, + g +,-w 시퀀스 gref 값, *다릅니다*합니다. 개체의 여러 Gc 생존, 관리가 어려울 인스턴스에서 실제로 할당을 확인 하려면 몇 가지 gref 값으로 전환 됩니다.

### <a name="querying-programmatically"></a>프로그래밍 방식으로 쿼리

쿼리하여 GREF와 WREF 개수를 쿼리할 수 있습니다는 `JniRuntime` 개체입니다.

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount` -글로벌 참조 횟수

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount` -약한 참조 횟수



## <a name="offline-activation"></a>오프 라인 정품 인증

Windows에서는 Xamarin.Android를 활성화할 수 없습니다 또는 Mac OS x Xamarin.Android의 전체 버전을 설치할 수 없습니다 인 경우 참조 하십시오.는 [오프 라인 정품 인증](~/android/get-started/installation/index.md) 페이지.



## <a name="cant-upgrade-to-indiebusiness-from-trial-account"></a>평가판 계정에서 Indie/비즈니스로 업그레이드할 수 없습니다.

최근에 Xamarin.Android를 구입 하 고 이전에 Xamarin.Android 평가판을 시작 하는 경우 Visual Studio가 Mac 또는 Visual Studio에 대 한 선택이 라이선스 변경 하려면 다음 단계를 완료 해야 합니다.

-  Mac/Visual Studio 용 Visual Studio를 닫습니다.
-  Windows 용 Android\License\에 대 한 Mac에서 ~/Library/MonoAndroid 또는 %PROGRAMDATA%\Mono에서 모든 파일을 제거
-  Mac/Visual Studio 용 Visual Studio를 다시 엽니다 및 Xamarin.Android 프로젝트 빌드


이 가동 되 고 실행 발생 해야 합니다. 문제가 계속 되 면 수 하려는 시도 [오프 라인 정품 인증](~/android/get-started/installation/index.md) 워크스테이션의 활성화를 완료 합니다.



## <a name="receiving-activation-incomplete-error-message"></a>받는 ' 활성화 불완전 한 오류 메시지

이 문제는 Visual Studio에 대 한 Xamarin.Android를 사용 하는 경우에 발생할 수 있습니다. 이 문제를 해결 하려면 보내 주십시오 로그를 다음 위치에서  *contact@xamarin.com* 합니다.

-  로그 위치: **% LocalAppData %\\Xamarin\\로그**




## <a name="receiving-error-retrieving-update-information-error-message"></a>오류 메시지 '오류 업데이트 정보를 검색 하는 데 사용'

때때로, 업데이트가 업데이트를 확인할 때 자주 발생 하는 다음이 오류와 함께 실패 합니다.

상당한 시간이 로깅 Xamarin 아웃 하 여이 오류를 해결 하 고 다시 로그인 합니다.

이를 위해 아래 원하는 플랫폼을 찾을 한 단계를 따릅니다.

**Mac:**
1. Mac 용 Visual Studio를 엽니다
2. Mac 용 Visual Studio > 계정 중...
3. 로그 아웃을 클릭 합니다.
4. 로그인을 클릭 합니다.
5. 자격 증명 입력
6. 업데이트 확인

**Visual Studo를 사용 하 여 PC의 경우:**
1. Visual Studio를 엽니다.
2. 도구 > Xamarin 계정
3. 로그 아웃을 클릭 합니다.
4. 로그인을 클릭 합니다.
5. 자격 증명 입력
6. 업데이트 확인

이 오류 메시지가 계속 표시 되 면 전자 하십시오  **contact@xamarin.com** 합니다.




## <a name="android-debug-logs"></a>Android 디버그 로그

[Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md) 표시 되는 모든 런타임 오류에 대 한 추가 컨텍스트를 제공할 수 있습니다.



## <a name="floating-point-performance-is-terrible"></a>부동 소수점 성능은 테러!

또는 "내 응용 프로그램을 실행 10 배 더 빠른 릴리스 빌드와 보다 디버그 빌드와!"

Xamarin.Android 여러 장치 ABIs 지원: *armeabi*, *armeabi v7a*, 및 *x86*합니다. 장치 ABIs를 지정할 수 있습니다 **프로젝트 속성 > 응용 프로그램 탭 > 지원 되는 아키텍처**합니다.

디버그 빌드를 모든 ABIs 제공 하며 따라서 대상 장치에 대 한 가장 빠른 ABI를 사용 합니다 Android 패키지를 사용 합니다.

릴리스 빌드 프로젝트 속성 탭에서 선택한 ABIs 포함 됩니다. 여러 개 선택할 수 있습니다.

*armeabi* ABI를 기본 템플릿이 고 광범위 한 장치 지원. *그러나*, armeabi를 지원 하지 않습니다 다중 CPU 장치 및 하드웨어 부동 소수점, amont 다른 작업입니다. 따라서 armeabi 릴리스 런타임을 사용 하 여 앱 단일 코어 관련이 있고 소프트 float 구현을 사용 합니다. 둘 다에 해당 앱에 대 한 성능이 훨씬 저하 될 수 있습니다.

앱에 괜찮은 부동 소수점 성능 (예: 게임)가 필요한 경우 활성화 해야는 *armeabi v7a* ABI입니다. 만 지원 하려는 경우는 *armeabi v7a* 런타임이 의미 하는 경우에 지 원하는 이전 장치 *armeabi* 앱을 실행할 수 없습니다.



## <a name="could-not-locate-android-sdk"></a>Android SDK를 찾을 수 없습니다.

Windows 용 Android SDK에 대 한 Google에서 제공 2 다운로드 됩니다.
.Exe 설치 관리자를 선택 하면 설치 된 Xamarin.Android 지시 하는 레지스트리 키를 기록 합니다. .Zip 파일을 선택 하 고 압축을 풀고 직접 경우 Xamarin.Android를 SDK를 찾을 위치를 알지 못합니다. 로 이동 하 여 Visual Studio에서 SDK 인 Xamarin.Android 알 수 **도구 > 옵션 > Xamarin > Android 설정**:

[![Xamarin Android 설정에서 android SDK 위치](troubleshooting-images/01a.png)](troubleshooting-images/01a.png#lightbox)



## <a name="ide-does-not-display-target-device"></a>IDE는 대상 장치를 표시 하지 않습니다.

경우에 따라 장치를 되지만 장치 선택 대화 상자에 표시 되지 않습니다를 배포 하려면 장치에 응용 프로그램을 배포 하려고 합니다. 이 Android 디버그 브리지 휴가 이동 하기로 결정 하는 경우 발생할 수 있습니다.

이 문제를 진단 하는 [adb 프로그램](~/android/deploy-test/debugging/android-debug-log.md), 다음 실행:

```shell
adb devices
```

장치가 없으면 장치를 찾을 수 있도록 Android 디버그 브리지 서버를 다시 시작 해야 합니다.

```shell
adb kill-server
adb start-server
```

소프트웨어 HTC 동기화 되지 않을 수 있습니다 **adb 시작 서버** 제대로 작동 합니다. 경우는 **adb 시작 서버** 명령에서 시작 하는 포트를 출력 하지 않습니다, 하십시오 HTC 동기화 소프트웨어 종료 및 adb 서버를 다시 시작 하십시오.


## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>지정한 작업 실행 파일 "keytool"를 실행할 수 없습니다.

즉, 사용자의 경로 Java SDK의 bin 디렉터리 있는 디렉터리에 포함 되지 않습니다. 이러한 단계를 따르고 있는지 확인 하 고 [설치](~/android/get-started/installation/index.md) 가이드입니다.


## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>monodroid.exe 또는 aresgen.exe 1 코드로 종료 되었습니다.

이 문제를 디버깅 하 고 Visual Studio에 MSBuild의 자세한 정도 수준이를 수행 하려면 변경할 수 있도록 선택: **도구 > 옵션 > 프로젝트** 및 **솔루션 > 빌드** 및 **실행 > MSBuild 프로젝트 빌드 출력의 자세한 정도** 이 값을 설정 하 고 **보통**합니다.

를 다시 작성 하 고 전체 오류를 포함 해야 하는 Visual Studio의 출력 창을 확인 합니다.

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>패키지를 배포 하려면 장치에 저장 공간이 부족

Visual Studio 내에서 에뮬레이터를 시작 하지 않는 경우 발생 합니다. 전달 해야 하는 Visual Studio 외부에서 에뮬레이터를 시작할 때는 `-partition-size 512` 옵션, 예:

```shell
emulator -partition-size 512 -avd MonoDroid
```

즉, 올바른 시뮬레이터 이름을 사용 해야 [시뮬레이터를 구성할 때 사용한 이름](~/android/get-started/installation/windows.md#device)합니다.


## <a name="installfailedinvalidapk-when-installing-a-package"></a>설치\_실패\_잘못 된\_APK 패키지를 설치할 때

Android 패키지 이름 *해야* 마침표가 포함 ('*.*'). 마침표를 포함 하도록 패키지 이름을 편집 합니다.

-   Visual Studio 내에서:
    -   프로젝트를 마우스 오른쪽 단추로 클릭 > 속성
    -   왼쪽에 Android 매니페스트 탭을 클릭 합니다.
    -   패키지 이름 필드를 업데이트 합니다.
        -   메시지가 표시 되 면 &ldquo;아니요 AndroidManifest.xml 찾을 수 있습니다. 하나를 추가 하려면 클릭 합니다. &rdquo;, 링크를 클릭 한 다음 패키지 이름 필드를 업데이트 합니다.
-   Mac 용 Visual Studio 내에서:
    -   프로젝트를 마우스 오른쪽 단추로 클릭 > 옵션입니다.
    -   빌드로 이동 / Android 응용 프로그램 섹션.
    -   패키지 이름 필드를 포함 하도록 변경는 '.'.




## <a name="installfailedmissingsharedlibrary-when-installing-a-package"></a>설치\_실패\_MISSING\_SHARED\_라이브러리 패키지를 설치할 때

이 컨텍스트에서 "공유 라이브러리"는 *하지* 네이티브 공유 라이브러리 (*libfoo.so*) 파일; 호스팅하려는 Google 맵과 같은 대상 장치에 별도로 설치 해야 하는 라이브러리입니다.

Android 패키지에서 사용 중인 공유 라이브러리 됩니다 지정는 `<uses-library/>` 요소입니다. 경우는 *필요한* 라이브러리 대상 장치에 나타나지 않습니다. (예: `//uses-library/@android:required` 은 *true*, 기본값), 패키지 설치 실패와 *설치\_ 실패\_MISSING\_SHARED\_라이브러리*합니다.

필요한 공유 라이브러리를 확인, 보기는 *생성*
**AndroidManifest.xml** 파일 (예: **obj\\디버그\\android \\AndroidManifest.xml**)를 찾아서 여 `<uses-library/>` 요소입니다. `<uses-library/>` 프로젝트의 요소를 수동으로 추가 **속성\\AndroidManifest.xml** 파일 및 via는 [UsesLibraryAttribute 사용자 지정 특성](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/)합니다.

예를 들어, 어셈블리 참조를 추가 *Mono.Android.GoogleMaps.dll* 암시적으로 추가 됩니다 한 `<uses-library/>` Google 맵 공유 라이브러리에 대 한 합니다.



## <a name="installfailedupdateincompatible-when-installing-a-package"></a>설치\_실패\_업데이트\_패키지를 설치할 때 호환 되지 않습니다.

Android 패키지에는 세 가지 요구 사항이 있습니다.

-   포함 해야 합니다는 '.' (이전 항목 참조)
-   두 가지 범주인 패키지 이름을 가져야 합니다 (따라서 Android 응용 프로그램 이름, Chrome 앱에 대 한 예를 들어 com.android.chrome 시에 역방향 tld 규칙)
-   패키지를 업그레이드 하는 경우 패키지는 동일한 서명 키를 포함 해야 합니다.

따라서이 시나리오를 가정해 봅니다.

1.  빌드 및 디버그 응용 프로그램으로 앱 배포
2.  서명 키 예를 들어로 변경한 릴리스 응용 프로그램으로 사용 하 여 (또는 마음에 들지 않는 되기 때문에 기본 제공 디버깅 서명 키)
3.  먼저 제거 하지 않고 응용 프로그램을 설치, 예: 디버그 > Visual Studio 내에서 디버깅 하지 않고 시작


이 경우 패키지를 설치할 설치\_실패\_업데이트\_호환 되지 않는 오류는 서명 하는 동안 변경 되지 않은 패키지 이름 때문에 키가 있습니다. [Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md) 비슷한 오류 메시지가 포함 됩니다.

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

이 오류를 해결 하려면에서 완전히 제거 응용 프로그램 장치를 다시 설치 하기 전에.


## <a name="installfaileduidchanged-when-installing-a-package"></a>설치\_실패\_UID\_패키지를 설치할 때 변경

Android 패키지를 설치 하면 할당 된 *사용자 id* (UID)입니다.
*경우에 따라*를 현재 알 수 없는 이유를 이미 설치 된 응용 프로그램을 설치할 때 설치와 함께 실패 합니다 `INSTALL_FAILED_UID_CHANGED`:

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

이 문제를 해결 하려면 *완전히 제거* Android 대상의 GUI에서 앱을 설치 하거나 사용 하 여 Android 패키지 `adb`:

```shell
$ adb uninstall @PACKAGE_NAME@
```

**사용 하지 마십시오** `adb uninstall -k`으로이 표시 *유지* 응용 프로그램 데이터를를 따라서 충돌 하는 대상 장치에서 UID를 보관 합니다.



## <a name="release-apps-fail-to-launch-on-device"></a>릴리스 앱 장치에서 실행 되지 않습니다.

에 Android 디버그 로그 출력 비슷한 오류 메시지가 포함 됩니다.

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

이 경우는이 두 가지 가능한 원인은

1.  .Apk 대상 장치에서 지 원하는 ABI를 제공 하지 않습니다.
    예를 들어.apk armeabi v7a 바이너리 포함 되며 대상 장치에만 armeabi 지원.

2.  [Android 버그](http://code.google.com/p/android/issues/detail?id=21670)합니다. 이 경우 앱을 제거, 손가락을 교차 및 응용 프로그램을 다시 설치 합니다.

(1)를 해결 하려면 프로젝트 옵션/속성을 편집 하 고 [ABIs 지원 목록에 필요한 ABI에 대 한 지원을 추가](~/android/app-fundamentals/cpu-architectures.md)합니다. 에 추가 해야 하는 ABI를 확인 하려면 대상 장치에 대해 다음 adb 명령을 실행 합니다.

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

주 출력에 포함 됩니다 (및 선택적 보조) ABIs 합니다.

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>프로젝트에 대 한 OutPath 속성은 &ldquo;MyApp.csproj&rdquo;

HP 컴퓨터와 환경 변수는 일반적으로 의미 &ldquo;플랫폼&rdquo; MCD 또는 HPD와 같은 값으로 설정 합니다. 일반적으로로 설정 된 MSBuild 플랫폼 속성과 충돌이 &ldquo;Any CPU&rdquo; 또는 &ldquo;x86&rdquo;합니다. MSBuild 작동할 수 전에 컴퓨터에서이 환경 변수를 제거 해야 합니다.

-   제어판 > 시스템 > 고급 > 환경 변수

Mac 용 Visual Studio 또는 Visual Studio를 다시 시작 하 고 다시 작성 하려고 합니다. 작업이 정상적으로 작동 해야 합니다.

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>java.lang.ClassCastException: mono.android.runtime.JavaObject으로 캐스팅할 수 없습니다...

Xamarin.Android 4.x 중첩 된 제네릭 형식을 제대로 마샬링할 적절 하지 않습니다. 예를 들어 다음 C\# 사용 하는 코드 [SimpleExpandableListAdapter](https://developer.xamarin.com/api/type/Android.Widget.SimpleExpandableListAdapter/):


```csharp
// BAD CODE; DO NOT USE
var groupData = new List<IDictionary<string, object>> () {
        new Dictionary<string, object> {
                { "NAME", "Group 1" },
                { "IS_EVEN", "This group is odd" },
        },
};
var childData = new List<IList<IDictionary<string, object>>> () {
        new List<IDictionary<string, object>> {
                new Dictionary<string, object> {
                        { "NAME", "Child 1" },
                        { "IS_EVEN", "This group is odd" },
                },
        },
};
mAdapter = new SimpleExpandableListAdapter (
        this,
        groupData,
        Android.Resource.Layout.SimpleExpandableListItem1,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
        childData,
        Android.Resource.Layout.SimpleExpandableListItem2,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
);
```


문제는 Xamarin.Android 올바르게 마샬링합니다 하지 중첩 된 제네릭 형식을입니다. `List<IDictionary<string, object>>` 마샬링 되 고는 [java.lang.ArrrayList](https://developer.xamarin.com/api/type/Java.Util.ArrayList/), 하지만 `ArrayList` 포함 하는 `mono.android.runtime.JavaObject` 인스턴스 (어떤 참조는 `Dictionary<string, object>` 인스턴스) 를구현하는대신[java.util.Map](https://developer.xamarin.com/api/type/Java.Util.IMap/), 다음과 같은 예외가 발생 합니다.

```shell
E/AndroidRuntime( 2991): FATAL EXCEPTION: main
E/AndroidRuntime( 2991): java.lang.ClassCastException: mono.android.runtime.JavaObject cannot be cast to java.util.Map
E/AndroidRuntime( 2991):        at android.widget.SimpleExpandableListAdapter.getGroupView(SimpleExpandableListAdapter.java:278)
E/AndroidRuntime( 2991):        at android.widget.ExpandableListConnector.getView(ExpandableListConnector.java:446)
E/AndroidRuntime( 2991):        at android.widget.AbsListView.obtainView(AbsListView.java:2271)
E/AndroidRuntime( 2991):        at android.widget.ListView.makeAndAddView(ListView.java:1769)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillDown(ListView.java:672)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillFromTop(ListView.java:733)
E/AndroidRuntime( 2991):        at android.widget.ListView.layoutChildren(ListView.java:1622)
```

이 제공 된를 해결 하려면 [Java 컬렉션 형식](~/android/internals/api-design.md) 대신는 `System.Collections.Generic` 에 대 한 형식이 &ldquo;내부&rdquo; 형식입니다. 이 인스턴스 마샬링 때 적절 한 Java 형식 발생 합니다. (다음 코드는 gref 수명을 최소화 하기 위해 필요한 것 보다 더 복잡 합니다. 통해 원래 코드 변경에 단순화 `s/List/JavaList/g` 및 `s/Dictionary/JavaDictionary/g` gref 수명에는 문제를 모두 해결 하지 않은 경우.)

```csharp
// insert good code here
using (var groupData = new JavaList<IDictionary<string, object>> ()) {
    using (var groupEntry = new JavaDictionary<string, object> ()) {
        groupEntry.Add ("NAME", "Group 1");
        groupEntry.Add ("IS_EVEN", "This group is odd");
        groupData.Add (groupEntry);
    }
    using (var childData = new JavaList<IList<IDictionary<string, object>>> ()) {
        using (var childEntry = new JavaList<IDictionary<string, object>> ())
        using (var childEntryDict = new JavaDictionary<string, object> ()) {
            childEntryDict.Add ("NAME", "Child 1");
            childEntryDict.Add ("IS_EVEN", "This child is odd.");
            childEntry.Add (childEntryDict);
            childData.Add (childEntry);
        }
        mAdapter = new SimpleExpandableListAdapter (
            this,
            groupData,
            Android.Resource.Layout.SimpleExpandableListItem1,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
            childData,
            Android.Resource.Layout.SimpleExpandableListItem2,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
        );
    }
}
```

[그러면 이후 릴리스에서 수정 될](https://bugzilla.xamarin.com/show_bug.cgi?id=5401)합니다.


## <a name="unexpected-nullreferenceexceptions"></a>Unexpected NullReferenceExceptions

경우에 따라는 [Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md) 발생 된 Nullreferenceexception 언급 하는 &ldquo;발생할 수 없습니다&rdquo; 값 이나 앱 모두 소모 될 되기 바로 전에 Android 런타임 코드 모노에서 가져온:

```shell
E/mono(15202): Unhandled Exception: System.NullReferenceException: Object reference not set to an instance of an object
E/mono(15202):   at Java.Lang.Object.GetObject (IntPtr handle, System.Type type, Boolean owned)
E/mono(15202):   at Java.Lang.Object._GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Java.Lang.Object.GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Android.Views.View+IOnTouchListenerAdapter.n_OnTouch_Landroid_view_View_Landroid_view_MotionEvent_(IntPtr jnienv, IntPtr native__this, IntPtr native_v, IntPtr native_e)
E/mono(15202):   at (wrapper dynamic-method) object:b039cbb0-15e9-4f47-87ce-442060701362 (intptr,intptr,intptr,intptr)
```

또는

```shell
E/mono    ( 4176): Unhandled Exception:
E/mono    ( 4176): System.NullReferenceException: Object reference not set to an instance of an object
E/mono    ( 4176): at Android.Runtime.JNIEnv.NewString (string)
E/mono    ( 4176): at Android.Util.Log.Info (string,string)
```

이 Android 런타임 오류가 발생 하는 여러 가지 이유로, 대상의 GREF 제한에 도달 하거나 작업을 수행할를 포함 하는 프로세스를 중단 하기로 결정 하는 경우에 발생할 수 &ldquo;잘못&rdquo; JNI 사용 합니다.

대/소문자 인지를 확인 하려면 유사한 프로세스에서 메시지에 대 한 Android 디버그 로그를 확인 합니다.

```shell
E/dalvikvm(  123): VM aborting
```


## <a name="abort-due-to-global-reference-exhaustion"></a>전역 참조 소모도 인해 중단

Android 런타임의 JNI 레이어는 제한 된 수의 특정된 시점에서 유효한 시간에 포함 되도록 JNI 개체 참조만 지원 합니다. 이 제한을 초과 하는 작업을 중단 합니다.

GREF (*전역 참조*) 제한 값은 에뮬레이터에서 2000 참조 고 ~ 52000 하드웨어에서 참조 합니다.

Android 디버그 로그에는 이러한 메시지를 확인할 때 너무 많은 GREFs를 만들려는 시작 정보:

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

GREF 제한에 도달 하는 경우에 다음과 같은 메시지가 출력 됩니다.

```shell
D/dalvikvm(  602): GREF has increased to 2001
W/dalvikvm(  602): Last 10 entries in JNI global reference table:
W/dalvikvm(  602):  1991: 0x4057eff8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1992: 0x4057f010 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1993: 0x40698e70 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1994: 0x40698e88 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1995: 0x40698ea0 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1996: 0x406981f0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1997: 0x40698208 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1998: 0x40698220 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1999: 0x406956a8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  2000: 0x406956c0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602): JNI global reference table summary (2001 entries):
W/dalvikvm(  602):    51 of Ljava/lang/Class; 164B (41 unique)
W/dalvikvm(  602):    46 of Ljava/lang/Class; 188B (17 unique)
W/dalvikvm(  602):     6 of Ljava/lang/Class; 212B (6 unique)
W/dalvikvm(  602):    11 of Ljava/lang/Class; 236B (7 unique)
W/dalvikvm(  602):     3 of Ljava/lang/Class; 260B (3 unique)
W/dalvikvm(  602):     4 of Ljava/lang/Class; 284B (2 unique)
W/dalvikvm(  602):     8 of Ljava/lang/Class; 308B (6 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 316B
W/dalvikvm(  602):     4 of Ljava/lang/Class; 332B (3 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 356B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 380B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 452B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 476B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 500B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 548B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 572B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 596B (2 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 692B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 956B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1004B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1148B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 1172B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1316B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3452B
W/dalvikvm(  602):     1 of Ljava/lang/String; 28B
W/dalvikvm(  602):     2 of Ldalvik/system/VMRuntime; 12B (1 unique)
W/dalvikvm(  602):    10 of Ljava/lang/ref/WeakReference; 28B (10 unique)
W/dalvikvm(  602):     1 of Ldalvik/system/PathClassLoader; 44B
W/dalvikvm(  602):  1553 of Landroid/graphics/Point; 20B (1553 unique)
W/dalvikvm(  602):   261 of Landroid/graphics/Point; 28B (261 unique)
W/dalvikvm(  602):     1 of Landroid/view/MotionEvent; 100B
W/dalvikvm(  602):     1 of Landroid/app/ActivityThread$ApplicationThread; 28B
W/dalvikvm(  602):     1 of Landroid/content/ContentProvider$Transport; 28B
W/dalvikvm(  602):     1 of Landroid/view/Surface$CompatibleCanvas; 44B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$ControlledInputConnectionWrapper; 36B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$1; 12B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$W; 28B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/view/accessibility/AccessibilityManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout$LayoutParams; 44B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout; 332B
W/dalvikvm(  602):     2 of Lorg/apache/harmony/xnet/provider/jsse/TrustManagerImpl; 28B (1 unique)
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$MyWindow; 36B
W/dalvikvm(  602):     1 of Ltouchtest/RenderThread; 92B
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$3; 12B
W/dalvikvm(  602):     1 of Ltouchtest/DrawingView; 412B
W/dalvikvm(  602):     1 of Ltouchtest/Activity1; 180B
W/dalvikvm(  602): Memory held directly by tracked refs is 75624 bytes
E/dalvikvm(  602): Excessive JNI global references (2001)
E/dalvikvm(  602): VM aborting
```


위의 예제에서 (참고로,에서 가져온는 [685215 버그](https://bugzilla.novell.com/show_bug.cgi?id=685215)) 문제가 너무 많은 Android.Graphics.Point 인스턴스가 생성 되 고은; 참조 [주석 \#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2) 에 대 한 수정 목록은 이 특정 버그입니다.

일반적으로 유용한 솔루션에 너무 많은 인스턴스가 형식을 찾는 것 할당 된 &ndash; 위의 덤프에 Android.Graphics.Point &ndash; 여기서 만들어질 소스 코드와 그 중 dispose 적절 하 게 찾습니다 (되도록 해당 Java-object 수명 축약 됨). 항상 적절 한 아닙니다 (\#685215는 Dispose 호출을 방지 하는 간단한 솔루션 이므로 다중 스레드,), 가장 먼저 고려해 야 할 이지만 합니다.

사용 하도록 설정할 수 [GREF 로깅](~/android/troubleshooting/index.md) GREFs 만들어지는 시기와 횟수 존재 합니다.


## <a name="abort-due-to-jni-type-mismatch"></a>JNI 유형이 일치 하지 않아 중단

하면 손 롤 JNI 코드,이 경우 형식은 않습니다 일치 하 올바르게, 예를 들어 호출 하려고 할 경우 `java.lang.Runnable.run` 구현 하지 않는 형식에서 `java.lang.Runnable`합니다. 이 경우 메시지가 있을 것는 Android 디버그 로그에 다음과 비슷한:

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>동적 코드 지원

### <a name="dynamic-code-does-not-compile"></a>동적 코드를 컴파일할 수 없는

C를 사용 하려면\# 을 System.Core.dll, Microsoft.CSharp.dll 및 Mono.CSharp.dll 프로젝트에 추가 해야 응용 프로그램 또는 라이브러리에 동적입니다.

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>릴리스 빌드에서 MissingMethodException 동적 코드에 대 한 런타임 시 발생합니다.

-   응용 프로그램 프로젝트 System.Core.dll, Microsoft.CSharp.dll 또는 Mono.CSharp.dll에 대 한 참조가 없는 가능성이 높습니다. 해당 어셈블리가 있는지 확인 합니다.

    -   염두에서에 둬야 동적 코드를 항상 비용입니다. 효율적인 코드를 필요 하지 동적 코드를 사용 하는 것이 좋습니다.

-   첫 번째 미리 보기에서 해당 어셈블리에는 각 어셈블리의 형식이 응용 프로그램 코드에서 명시적으로 사용 되지 않는 한 제외 되었습니다. 해결 방법 다음을 참조 하십시오: [http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)


## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>X86 AOT + 원하는 LLVM 크래시로 빌드된 프로젝트 장치

사용 하 여 빌드한 앱을 배포 하는 경우 [AOT + 원하는 LLVM](~/android/deploy-test/release-prep/index.md) x86 기반 장치에서 다음과 비슷한 예외 오류 메시지가 표시 될 수 있습니다.

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (amarin.bug56111)
```

이것은 알려진된 문제에 보고 된 [56111](https://bugzilla.xamarin.com/show_bug.cgi?id=56111)합니다. 원하는 LLVM 사용 하지 않으려면이 문제를 해결 합니다.
