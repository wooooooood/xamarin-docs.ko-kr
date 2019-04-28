---
title: 문제 해결 팁
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: b2f11bd09e1b1b3fd7af29a026229494a081ad11
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61085053"
---
# <a name="troubleshooting-tips"></a>문제 해결 팁


## <a name="getting-diagnostic-information"></a>진단 정보 가져오기

Xamarin.Android는 다양 한 버그를 추적 하는 경우 검색할 몇몇 위치에 있습니다.
여기에는 다음이 포함됩니다.

1.  진단 MSBuild 출력입니다.
2.  장치 배포 로그입니다.
3.  Android 디버그 로그 출력입니다.


<a name="Diagnostic_MSBuild_Output" />

## <a name="diagnostic-msbuild-output"></a>진단 MSBuild 출력

진단 MSBuild 패키지 구성에 관련 된 추가 정보를 포함할 수 있습니다 및 일부 패키지 배포 정보를 포함 될 수 있습니다.

Visual Studio 내에서 진단 MSBuild 출력을 사용하려면:

1.  클릭 **도구 > 옵션...**
2.  왼쪽 트리 뷰에서 선택 **프로젝트 및 솔루션 > 빌드 및 실행**
3.  오른쪽 창에서 MSBuild 빌드 출력 세부 정보 표시 드롭다운을 진단 설정
4.   **확인**을 클릭합니다.
5.  패키지를 지우고 다시 빌드합니다.
6.  진단 출력이 출력 패널에 표시 됩니다.


Mac/OS x:에 대 한 Visual Studio 내에서 진단 MSBuild 출력을 사용 하도록 설정 하려면

1.  클릭 **Mac 용 Visual Studio > 기본 설정...**
2.  왼쪽 트리 뷰에서 선택 **프로젝트 > 빌드**
3.  오른쪽 패널에서로 로그 세부 정보 표시 드롭다운 진단
4.   **확인**을 클릭합니다.
5.  Mac용 Visual Studio 다시 시작
6.  패키지를 지우고 다시 빌드합니다.
7.  진단 출력이 오류 패드 내에서 표시 됩니다 (**보기 > 패드 > 오류** ), 빌드 출력 단추를 클릭 합니다.




## <a name="device-deployment-logs"></a>장치 배포 로그

Visual Studio 내 장치 배포 로깅을 설정 하려면

1.  **도구 > 옵션...**>
2.  왼쪽 트리 뷰에서 선택 **Xamarin > Android 설정**
3.  오른쪽 창에서 [X]를 사용 하도록 설정 **확장 디버그 로깅 (에 monodroid.log 작성 데스크톱)** 확인란 합니다.
4.  로그 메시지는 데스크톱에 monodroid.log 파일에 기록 됩니다.


Mac 용 visual Studio는 항상 장치 배포 로그를 기록합니다. 찾을 수 있으며 병렬화가 약간 더 어렵습니다. *AndroidUtils* 모든 날짜 + 예를 들어 배포를 발생 하는 시간에 대 한 로그 파일이 만들어집니다. **AndroidTools-2012-10-24_12-35-45.log**.

-  Windows에서의 로그 파일에 기록 됩니다 `%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`합니다.
-  OS X에서 로그 파일에 기록 됩니다 `$HOME/Library/Logs/XamarinStudio-{VERSION}`합니다.




## <a name="android-debug-log-output"></a>Android 디버그 로그 출력

Android 많은 메시지를 작성 합니다 [Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md)합니다.
Xamarin.Android는 Android 시스템 속성을 사용 하 여 Android 디버그 로그에 추가 메시지의 생성을 제어 하 합니다. Android 시스템 속성을 통해 설정할 수 있습니다는 *setprop* 내에서 명령을 합니다 [Android Debug Bridge (adb)](https://developer.android.com/guide/developing/tools/adb.html):

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

시스템 속성 프로세스 시작 시 읽고 따라서 있어야 설정 하거나 응용 프로그램이 시작 되거나 시스템 속성이 변경 된 후 응용 프로그램을 다시 시작 해야 합니다.



### <a name="xamarinandroid-system-properties"></a>Xamarin.Android 시스템 속성

Xamarin.Android에는 다음 시스템 속성을 지원합니다.

-   *debug.mono.debug*: 이 해당 하는 경우 비어 있지 않은 문자열로 `*mono-debug*`합니다.

-   *debug.mono.env*: 파이프로 구분 된 ('*|*') 응용 프로그램을 시작 하는 동안 내보내려면 환경 변수 목록을 *하기 전에* mono가 초기화 되었습니다. 따라서 해당 컨트롤 mono 로깅 환경 변수를 설정 합니다.

    - *참고*: 값 이므로 '*|*'-구분 값이 있어야 인용에 대 한 추가 수준으로는 \` *adb 셸* \` 명령 집합이 따옴표를 제거 합니다.

    - *참고*: 않을 Android 시스템 속성 값 길이가 92 문자 보다 길지 수 없습니다.

    - 예제:

            adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"

-   *debug.mono.log*: 쉼표로 구분 된 ('*,*') Android 디버그 로그에 추가 메시지를 인쇄 해야 하는 구성 요소의 목록입니다. 기본적으로 아무 것도 설정 됩니다. 구성 요소에는 다음이 포함 됩니다.

    -   *all*: 모든 메시지를 인쇄 합니다.
    -   *gc*: GC 관련 메시지를 인쇄 합니다.
    -   *gref*: (약한, 전역) 참조 할당 및 할당 취소 메시지를 인쇄 합니다.
    -   *lref*: 로컬 참조 할당 및 할당 취소 메시지를 인쇄 합니다.

    *참고*: 이들은 *매우* 자세한 정보. 필요한 경우에 실제로 사용 하지 마십시오.

-   *debug.mono.trace*: 설정할 수 있습니다 합니다 [mono-추적](http://docs.go-mono.com/?link=man%3amono(1)) `=PROPERTY_VALUE` 설정 합니다.



## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin.Android System.ValueTuple를 확인할 수 없습니다.

Visual Studio를 사용 하 여 비 호환성으로 인해이 오류가 발생합니다.

- **Visual Studio 2017 업데이트 1** (버전 15.1 이상)만 호환 됩니다. 합니다 **System.ValueTuple NuGet 4.3.0** (또는 이전 버전).

- **Visual Studio 2017 업데이트 2** (버전 15.2 이상)만 호환 됩니다. 합니다 **System.ValueTuple NuGet 4.3.1** (이상).

Visual Studio 2017 설치를 사용 하 여 해당 하는 올바른 System.ValueTuple NuGet을 선택 하세요.


## <a name="gc-messages"></a>GC 메시지

Gc를 포함 하는 값을 debug.mono.log 시스템 속성을 설정 하 여 GC 구성 요소 메시지를 볼 수 있습니다.

GC 메시지는 실행 하 고 않았습니다에 얼마나 많은 GC를 작동 하는 정보를 제공 하는 GC 때마다 생성 됩니다.

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

타이밍 정보 등 추가 GC 정보를 설정 하 여 생성할 수 있습니다 합니다 `MONO_LOG_LEVEL` 환경 변수를 `debug`:

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

이렇게 하면 (많은) 추가 Mono 메시지를 결과의이 세 가지를 포함 하 여:

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

에 `GC_BRIDGE` 메시지 `num-objects` 이 통과 고려 하는 연결 개체의 수 및 `num_hash_entries` 브리지 코드의이 호출 하는 동안 처리 되는 개체입니다.

에 `GC_MINOR` 및 `GC_MAJOR` 메시지 `total` 세계 일시 중지 되는 시간 (스레드가 실행 중)을 하는 동안 `bridge` 브리지 처리 코드 (Java VM을 사용 하 여 처리)에 소요 된 시간입니다. 지구가 *되지* 브리지 처리에서 발생 하는 동안 일시 중지 합니다.

 *일반적*의 값이 클수록 `num_hash_entries`,으로 자세히를 `bridge` 수집에 걸리는 커집니다는 `total` 수집 하는 데 걸린 시간이 됩니다.



## <a name="global-reference-messages"></a>전역 참조 메시지

전역 참조 loggig (GREF) 로깅을 사용 하도록 설정 합니다 *debug.mono.log* 시스템 속성이 포함 되어야 합니다 *gref*예:

```shell
adb shell setprop debug.mono.log gref
```

Xamarin.Android Android 전역 참조를 사용 하 여 Java 인스턴스를 Java를 제공 해야 하는 Java 메서드를 호출할 때으로 Java 인스턴스 및 연결된 된 관리 되는 인스턴스 간의 매핑을 제공 합니다.

그러나 Android 에뮬레이터는 한 번에 존재에 대 한 2000 전역 참조만 허용 합니다. 하드웨어 제한이 훨씬 더 높은 52000 전역 참조 합니다. 하한값 때문 에뮬레이터에서 응용 프로그램을 실행 하는 경우 문제가 될 수 있습니다 *여기서* 인스턴스 준 매우 유용할 수 있습니다.

 *참고*: 전역 참조 횟수는 Xamarin.Android의 내부 및 않습니다 (이며) 프로세스에 로드 하는 다른 네이티브 라이브러리 수행한 전역 참조를 포함 합니다. 전역 참조 횟수는 예상치로 사용 합니다.

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

결과의 네 가지 메시지 가지가 있습니다.

-  전역 참조를 만들지:로 시작 하는 줄을 이들은 *+ g +* , 만드는 코드 경로 대 한 스택 추적을 제공 합니다.
-  전역 참조 소멸:로 시작 하는 줄을 이들은 *-g-* , 그리고 전역 참조를 삭제 하는 코드 경로 대 한 스택 추적을 제공할 수 있습니다. GC는 gref 삭제, 스택 추적 없음 제공 됩니다.
-  전역 약한 참조 만들기:로 시작 하는 줄을 이들은 *+ w +* 합니다.
-  전역 약한 참조 소멸:로 시작 하는 줄을 이들은 *-w-* 합니다.


모든 메시지에는 *grefc* 값은 Xamarin.Android가 생성 되는 전역 참조의 개수 하는 동안 합니다 *grefwc* 값은 Xamarin.Android 만들어진 약한 전역 참조의 개수입니다. 합니다 *처리할* 또는 *obj 핸들* 값 JNI 핸들 값 이며 다음 문자는 ' */*' 핸들 값의 형식입니다: */L* 로컬 참조용 */G* 전역 참조 및 */W* 약한 전역 참조에 대 한 합니다.

GC 프로세스의 일부로, 전역 참조 (+ g +)으로 변환 됩니다 약한 전역 참조 (a + w + 일으키는 및-g-)는 Java 쪽 GC 시작 되 고 전역 weak reference는 인지를 확인 하는 경우 수집 된 합니다. 여전히 활성 상태인 경우 새 gref 약한 참조 주위 만들어집니다 (+ g +,-w-), 약한 참조 제거는 그렇지 않은 경우 (-w).

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Java 인스턴스가 만들어지고는 MCW 래핑한

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>GC는 수행 하는 중...

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

## <a name="object-is-dead-as-handle--null"></a>개체가 소멸 핸들로 = = null
## <a name="wref-is-freed-no-new-gref-created"></a>wref가 해제 된, 새 gref 생성

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

하나의 "흥미로운" 가지: Android 4.0 이전 실행 대상 gref 값이 같은 Java 개체의 주소에는 Android 런타임 메모리입니다. (즉, GC는 이동 되지 않은, 일반, 수집기 및 해당 개체에 대 한 참조를 직접 처리 됩니다.) 따라서 후는 g + + w +,-g-, + g +,-w-순서, 결과 gref 원래 gref 값으로 동일한 값을 갖게 됩니다. 이렇게 하면 로그를 통해 grepping 매우 간단 합니다.

그러나 Android 4.0 이동 수집기 있으며 VM 개체를 Android 런타임에 대 한 직접 참조 더 이상 전달 합니다. 따라서 후는 g + + w +,-g-, + g +,-w-순서, gref 값 *달라 지므로*합니다. 여러 Gc 생존 하는 개체, 경우 인스턴스에서 실제로 할당을 확인 하기는 힘듭니다 여러 gref 값으로 이동 합니다.

### <a name="querying-programmatically"></a>프로그래밍 방식으로 쿼리

쿼리하여 GREF와 WREF 개수를 쿼리할 수 있습니다는 `JniRuntime` 개체입니다.

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount` 전역 참조 횟수

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount` -약한 참조 횟수



## <a name="android-debug-logs"></a>Android 디버그 로그

합니다 [Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md) 왔다면 런타임 오류에 대 한 추가 컨텍스트를 제공할 수 있습니다.



## <a name="floating-point-performance-is-terrible"></a>부동 소수점 성능은 끔찍한!

또는 "내 앱이 실행 10 배 더 빠른 릴리스 빌드를 사용 하 여 보다 디버그 빌드를 사용 하 여!"

Xamarin.Android에는 여러 장치 Abi 지원: *armeabi*하십시오 *armeabi-v7a*, 및 *x86*. 내 장치 Abi를 지정할 수 있습니다 **프로젝트 속성 > 응용 프로그램 탭 > 지원 되는 아키텍처**합니다.

디버그 빌드에서 모든 Abi를 제공 하 고 있으므로 가장 빠른 ABI에 사용할 대상 장치를 Android 패키지를 사용 합니다.

릴리스 빌드 프로젝트 속성 탭에서 선택한 Abi 포함 됩니다. 둘 이상 선택할 수 있습니다.

*armeabi* 기본 ABI에 있고 광범위 하 게 장치 지원. *그러나*, armeabi 및 지원 하지 않습니다 다중 CPU 장치 하드웨어 부동 소수점, amont 다른 작업입니다. 따라서 armeabi 릴리스 런타임을 사용 하 여 앱 단일 코어에 연결 됩니다 및 소프트 float 구현을 사용 합니다. 이러한 두 앱에 대 한 성능이 크게 저하 될 수 있습니다.

사용자 앱에 부동 소수점 성능을 (예: 게임)가 필요한 경우 사용 해야 합니다 *armeabi-v7a* ABI입니다. 지원 하려는 경우는 *armeabi-v7a* 런타임, 즉 있지만 지 원하는 이전 버전의 장치 *armeabi* 앱을 실행할 수 없습니다.



## <a name="could-not-locate-android-sdk"></a>Android SDK를 찾을 수 없습니다.

Google Android SDK에 대 한 Windows에서에서 사용할 수 있는 2 다운로드 됩니다.
.Exe 설치 관리자를 선택 하면 설치 된 Xamarin.Android를 지시 하는 레지스트리 키를 기록 합니다. .Zip 파일을 선택 하 고 압축을 풉니다 직접 Xamarin.Android SDK를 검색할 수 있는 위치를 인식 하지 못합니다. 으로 이동 하 여 Visual Studio에서 SDK 인 Xamarin.Android 알 수 있습니다 **도구 > 옵션 > Xamarin > Android 설정**:

[![Xamarin Android 설정에서 android SDK 위치](troubleshooting-images/01.png)](troubleshooting-images/01.png#lightbox)



## <a name="ide-does-not-display-target-device"></a>IDE는 대상 장치를 표시 하지 않습니다.

경우에 따라 장치 되지만 장치 선택 대화 상자에서 표시 되지 않습니다에 배포 하려는 장치에 응용 프로그램을 배포 하려고 합니다. 이 Android Debug Bridge 휴가 이동 하기로 결정 하는 경우 발생할 수 있습니다.

이 문제를 진단 하려면 합니다 [adb 프로그램](~/android/deploy-test/debugging/android-debug-log.md), 다음 실행:

```shell
adb devices
```

장치 목록에 없으면 다음 해야 장치를 찾을 수 있도록 Android Debug Bridge 서버를 다시 시작 합니다.

```shell
adb kill-server
adb start-server
```

HTC 동기화 소프트웨어 하지 못할 수 있습니다 **adb 서버 시작** 제대로 작동 합니다. 경우는 **adb 서버 시작** 명령에서 시작 하는 포트를 출력 하지 않습니다, HTC 동기화 소프트웨어 종료 후 adb 서버 다시 시작 해 보세요.


## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>지정 된 작업 실행 파일 "keytool"를 실행할 수 없음

즉, Java SDK의 bin 디렉터리 위치한 디렉터리 경로 포함 되지 않도록 합니다. 이러한 단계를 수행 했는지 확인 합니다 [설치](~/android/get-started/installation/index.md) 가이드입니다.


## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>monodroid.exe 또는 aresgen.exe 1 코드로 종료 되었습니다.

이 문제를 디버깅, Visual studio 및 MSBuild의 자세한 정도 수준이를 변경 하려면 선택 합니다. **도구 > 옵션 > 프로젝트** 하 고 **솔루션 > 빌드** 하 고 **실행 > MSBuild 프로젝트 빌드 출력의 자세한 정도** 이 값을 설정 하 고 **보통**.

다시 작성 하 고 전체 오류를 포함 해야 하는 Visual Studio의 출력 창을 확인 합니다.

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>패키지를 배포 하려면 장치에 저장 공간이 충분 한

이 경우에 Visual Studio 내에서 에뮬레이터를 시작 하지 않습니다. 전달 해야 하는 Visual Studio 외부에서 에뮬레이터를 시작 하는 경우는 `-partition-size 512` 옵션, 예:

```shell
emulator -partition-size 512 -avd MonoDroid
```

즉, 올바른 시뮬레이터 이름을 사용 해야 [시뮬레이터를 구성할 때 사용한 이름을](~/android/get-started/installation/windows.md#device)입니다.


## <a name="installfailedinvalidapk-when-installing-a-package"></a>설치할\_실패\_잘못 된\_APK 패키지를 설치 하는 경우

Android 패키지 이름 *해야 합니다* 마침표가 포함 ('*.*'). 마침표 포함 되도록 패키지 이름을 편집 합니다.

-   Visual studio:
    -   프로젝트를 마우스 오른쪽 단추로 클릭 > 속성
    -   왼쪽의 Android 매니페스트 탭을 클릭 합니다.
    -   패키지 이름 필드를 업데이트 합니다.
        -   메시지를 표시 하는 경우 &ldquo;아니요 AndroidManifest.xml을 찾을 수 있습니다. 하나를 추가 하려면 클릭 합니다. &rdquo;, 링크를 클릭 하 고 패키지 이름 필드를 업데이트 합니다.
-   Mac 용 Visual studio:
    -   프로젝트를 마우스 오른쪽 단추로 클릭 > 옵션.
    -   빌드로 이동 / Android 응용 프로그램 섹션입니다.
    -   패키지 name 필드를 포함 하도록 변경 된 '.'.




## <a name="installfailedmissingsharedlibrary-when-installing-a-package"></a>설치할\_실패\_MISSING\_공유\_라이브러리 패키지를 설치 하는 경우

이 컨텍스트에서 "공유 라이브러리" *되지* 네이티브 공유 라이브러리 (*libfoo.so*) 파일는 Google Maps 등 대상 장치에 별도로 설치 해야 하는 라이브러리입니다.

Android 패키지는 공유 라이브러리를 사용 하 여 필요한 지정 된 `<uses-library/>` 요소입니다. 경우는 *필요* 라이브러리를 대상 장치에 표시 되지 (예: `//uses-library/@android:required` 은 *true*, 기본값)를 사용 하 여 패키지 설치 실패 *설치\_ 실패\_MISSING\_SHARED\_라이브러리*합니다.

공유 라이브러리는 필요한 확인, 보고 된 *생성*
**AndroidManifest.xml** 파일 (예: **obj\\디버그\\android \\AndroidManifest.xml**)를 찾아서는 `<uses-library/>` 요소입니다. `<uses-library/>` 프로젝트의 요소를 수동으로 추가할 수 있습니다 **속성\\AndroidManifest.xml** 파일을 통해 합니다 [UsesLibraryAttribute 사용자 지정 특성](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/)합니다.

예를 들어, 어셈블리 참조를 추가 *Mono.Android.GoogleMaps.dll* 암시적으로 추가 된 `<uses-library/>` Google Maps 공유 라이브러리에 대 한 합니다.



## <a name="installfailedupdateincompatible-when-installing-a-package"></a>설치할\_실패\_업데이트\_패키지를 설치 하는 경우 호환 되지 않습니다.

Android 패키지에는 세 가지 요구 사항이 있습니다.

-   포함 해야 합니다는 '.' (이전 항목 참조)
-   고유한 문자열 패키지 이름을 가져야 합니다 (따라서 Android 앱 이름, 예: com.android.chrome Chrome 앱에 대 한 역방향 tld 규칙)
-   패키지를 업그레이드 하는 경우 동일한 서명 키를 패키지 있어야 합니다.

따라서이 시나리오를 가정해 보겠습니다.

1.  빌드 및 디버그 앱으로 앱 배포
2.  서명 키를 예를 들어으로 변경한 릴리스 앱으로 사용 하 여 (또는 마음에 들지 않는 되기 때문에 기본 제공 디버깅 서명 키)
3.  먼저 제거 하지 않고 앱을 설치, 예: 디버그 > Visual Studio 내에서 디버깅 하지 않고 시작


설치 패키지를 설치 하지 못합니다이 문제가 발생 하면\_실패\_업데이트\_호환 되지 않는 오류를 서명 하는 동안 패키지 이름을 변경 하지 않은 때문에 키가 있습니다. 합니다 [Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md) 비슷한 메시지가 포함 됩니다.

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

이 오류를 해결 하려면 완전히 제거 응용 프로그램이 장치에서 다시 설치 하기 전에 합니다.


## <a name="installfaileduidchanged-when-installing-a-package"></a>설치할\_실패\_UID\_패키지를 설치 하는 경우 변경 합니다.

Android 패키지를 설치 될 때 할당 된 *사용자 id* (UID).
*때로는*를 현재 알 수 없는 이유를 이미 설치 된 응용 프로그램을 통해 설치 하는 경우에 대 한 설치 하지 못합니다 `INSTALL_FAILED_UID_CHANGED`:

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

**사용 하지 마세요** `adb uninstall -k`는이 처럼 *유지* 응용 프로그램 데이터를 따라서 대상 장치에서 충돌 하는 UID를 유지 합니다.



## <a name="release-apps-fail-to-launch-on-device"></a>릴리스 앱 장치에서 시작 하지 못함

Android 디버그 로그 출력 않습니다 유사한 메시지에 포함 됩니다.

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

따라서 경우이 대 한 가능한 원인은 두 가지 있습니다.

1.  .Apk 대상 장치에서 지 원하는 ABI를 제공 하지 않습니다.
    예를 들어.apk armeabi-v7a 이진 파일, 포함 되며 대상 장치에 armeabi만 지원 합니다.

2.  [Android 버그](http://code.google.com/p/android/issues/detail?id=21670)합니다. 이 경우 앱을 제거, 손가락을 교차 및 앱을 다시 설치 합니다.

(1)를 해결 하려면 프로젝트 옵션 속성을 편집 하 고 [지원 되는 Abi의 목록에 필요한 ABI에 대 한 지원을 추가](~/android/app-fundamentals/cpu-architectures.md)합니다. 추가 해야 하는 ABI를 확인 하려면 대상 장치에 대해 다음 adb 명령을 실행 합니다.

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

출력에는 기본 포함 됩니다 (및 선택적 보조) Abi입니다.

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>프로젝트에 대 한 OutPath 속성은 &ldquo;MyApp.csproj&rdquo;

일반적으로 HP 컴퓨터 있고 환경 변수 의미 &ldquo;플랫폼&rdquo; MCD 또는 HPD와 같은 값으로 설정 합니다. 일반적으로 설정 된 MSBuild 플랫폼 속성을 사용 하 여이 충돌 &ldquo;Any CPU&rdquo; 하거나 &ldquo;x86&rdquo;합니다. MSBuild 작동할 수 전에 컴퓨터에서이 환경 변수를 제거 해야 합니다.

-   제어판 > 시스템 > 고급 > 환경 변수

Mac 용 Visual Studio 또는 Visual Studio를 다시 시작 하 고 다시 해 보세요. 작업 예상 대로 작동 합니다.

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>java.lang.ClassCastException: mono.android.runtime.JavaObject 캐스팅할 수 없습니다...

Xamarin.Android 4.x 중첩 된 제네릭 형식을 올바르게 마샬링하 제대로 하지 않습니다. 예를 들어 다음 C\# 를 사용 하 여 코드 [SimpleExpandableListAdapter](https://developer.xamarin.com/api/type/Android.Widget.SimpleExpandableListAdapter/):


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


문제는 Xamarin.Android 올바르게 마샬링해야 하지 중첩 된 제네릭 형식입니다. `List<IDictionary<string, object>>` 마샬링 중를 [java.lang.ArrrayList](https://developer.xamarin.com/api/type/Java.Util.ArrayList/), 하지만 `ArrayList` 포함 하는 `mono.android.runtime.JavaObject` 인스턴스 (참조는 `Dictionary<string, object>` 인스턴스) 구현하는대신[java.util.Map](https://developer.xamarin.com/api/type/Java.Util.IMap/), 예외가 발생 합니다.

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

제공 된 데이 문제를 해결 [Java 컬렉션 형식](~/android/internals/api-design.md) 대신 합니다 `System.Collections.Generic` 에 대 한 형식 합니다 &ldquo;내부&rdquo; 형식. 이렇게 하면 적절 한 Java 형식 인스턴스를 마샬링할 때. (다음 코드는 gref 수명을 줄이기 위해 필요한 것 보다 더 복잡 합니다. 통해 원본 코드를 변경 하려면 단순화할 수 있습니다 `s/List/JavaList/g` 및 `s/Dictionary/JavaDictionary/g` gref 수명을 걱정할 필요가 없는 경우.)

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

[향후 릴리스에서 수정 될 예정이](https://bugzilla.xamarin.com/show_bug.cgi?id=5401)합니다.


## <a name="unexpected-nullreferenceexceptions"></a>NullReferenceExceptions

경우에 따라를 [Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md) NullReferenceExceptions를 설명 하겠습니다는 &ldquo;발생할 수 없도록&rdquo; 앱 중단 되기 바로 전에 Android 런타임 코드에 대 한 Mono에서 발생 하거나:

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

이 Android 런타임 오류가 발생 하는 다양 한 이유로, 대상의 GREF 제한에 도달 하거나 작업을 수행한 포함 하는 프로세스를 중단 하기로 결정 하는 경우에 발생할 수 있습니다 &ldquo;잘못 된&rdquo; JNI 사용 합니다.

이 경우를 확인 하려면 유사한 프로세스에서 메시지에 대 한 Android 디버그 로그를 확인 합니다.

```shell
E/dalvikvm(  123): VM aborting
```


## <a name="abort-due-to-global-reference-exhaustion"></a>전역 참조 소모로 인해 중단

Android 런타임 JNI 레이어는 제한 된 수의 시간에 특정된 시점에서 유효한 것으로 JNI 개체 참조만 지원 합니다. 이 제한을 초과 하는 작업 중단 합니다.

GREF (*전역 참조*) 제한은 에뮬레이터에서 2000 참조 및 ~ 52000 하드웨어에서 참조 합니다.

너무 많은 GREFs 만들면 Android 디버그 로그에 다음과 같은 메시지가 표시를 시작 하는 것이 알고:

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

GREF 제한에 도달 하면 다음과 같은 메시지가 출력 됩니다.

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


위의 예제에서 (참고로,는에서 제공 [685215 버그](https://bugzilla.novell.com/show_bug.cgi?id=685215)) 문제는 너무 많은 Android.Graphics.Point 인스턴스가 만들어지고; 참조 [주석 \#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2) 용 수정 사항 목록은 이 특정 버그입니다.

형식에 너무 많은 인스턴스를 찾으려고 유용한 솔루션은 일반적으로 할당 &ndash; 위의 덤프에 Android.Graphics.Point &ndash; 여기서 만들어지는 소스 코드를 dispose 중에 적절 하 게 찾을 (있도록 해당 Java-object 수명을 단축 됩니다). 항상 적합 하지 않습니다 (\#685215 이므로 다중 스레드, Dispose 호출을 방지 하는 간단한 솔루션)에 있지만 가장 먼저 고려해 야 할 것입니다.

설정할 수 있습니다 [GREF 로깅](~/android/troubleshooting/index.md) GREFs 만들어지는 시기와 횟수 존재를 확인 합니다.


## <a name="abort-due-to-jni-type-mismatch"></a>JNI 형식 불일치로 인해 중단

하는 경우 하면 손-앨범 JNI 코드는 형식이 일치 하지 않습니다 올바르게 예: 수 호출 하려고 하면 `java.lang.Runnable.run` 구현 하지 않는 형식의 `java.lang.Runnable`합니다. 이 경우 있습니다 됩니다 메시지 Android 디버그 로그에 다음과 유사한:

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>동적 코드 지원

### <a name="dynamic-code-does-not-compile"></a>동적 코드를 컴파일되지 않습니다.

C를 사용 하려면\# System.Core.dll Microsoft.CSharp.dll을 Mono.CSharp.dll 프로젝트에 추가 해야 응용 프로그램 또는 라이브러리에 동적입니다.

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>릴리스 빌드에서 MissingMethodException 동적 코드에 대 한 런타임 시 발생합니다.

-   응용 프로그램 프로젝트, Microsoft.CSharp.dll 또는 Mono.CSharp.dll에 대 한 참조가 없는 것입니다. 이러한 어셈블리를 참조 해야 합니다.

    -   염두에 동적 코드 항상 비용입니다. 효율적인 코드를 해야 하는 경우에 동적 코드를 사용 하지 하는 것이 좋습니다.

-   첫 번째 미리 보기에서 해당 어셈블리에는 각 어셈블리의 형식은 응용 프로그램 코드에서 명시적으로 사용 되지 않는 한 제외 되었습니다. 해결 방법에 대해 다음을 참조 하세요. [http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)


## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>프로젝트 AOT + LLVM 크래시를 사용 하 여 x86 기반 장치

사용 하 여 빌드한 앱을 배포할 때 [AOT + LLVM](~/android/deploy-test/release-prep/index.md) x86 기반 장치에서 다음과 비슷한 예외 오류 메시지가 표시 될 수 있습니다.

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (amarin.bug56111)
```

이것은 알려진된 문제에 보고 된 대로 [56111](https://bugzilla.xamarin.com/show_bug.cgi?id=56111)합니다. LLVM 사용 하지 않으려면이 문제를 해결 합니다.
