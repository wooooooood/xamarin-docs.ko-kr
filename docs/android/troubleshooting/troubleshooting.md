---
title: 문제 해결 팁
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/15/2018
ms.openlocfilehash: ec5c6e4c4c47995e78c1819007a8fa5660873bd2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026588"
---
# <a name="troubleshooting-tips"></a>문제 해결 팁

## <a name="getting-diagnostic-information"></a>진단 정보 가져오기

Xamarin.ios에는 다양 한 버그를 추적할 때 살펴볼 몇 가지 위치가 있습니다.
여기에는 다음이 포함됩니다.

1. 진단 MSBuild 출력입니다.
2. 장치 배포 로그.
3. Android 디버그 로그 출력입니다.

<a name="Diagnostic_MSBuild_Output" />

## <a name="diagnostic-msbuild-output"></a>진단 MSBuild 출력

진단 MSBuild는 패키지 빌드와 관련 된 추가 정보를 포함할 수 있으며 일부 패키지 배포 정보를 포함할 수 있습니다.

Visual Studio 내에서 진단 MSBuild 출력을 사용하려면:

1. **도구 > 옵션 ...** 을 클릭 합니다.
2. 왼쪽 트리 보기에서 **프로젝트 및 솔루션 > 빌드 및 실행** 을 선택 합니다.
3. 오른쪽 패널에서 MSBuild 빌드 출력 세부 정보 표시 드롭다운을 진단으로 설정 합니다.
4. **확인**을 클릭합니다.
5. 패키지를 지우고 다시 빌드합니다.
6. 진단 출력이 출력 패널에 표시 됩니다.

Mac용 Visual Studio/OS X 내에서 진단 MSBuild 출력을 사용 하려면 다음을 수행 합니다.

1. **Mac용 Visual Studio > 기본 설정** ...을 클릭 합니다.
2. 왼쪽 트리 뷰에서 **프로젝트 > 빌드** 를 선택 합니다.
3. 오른쪽 패널에서 로그 세부 정보 표시 드롭다운을 진단으로 설정 합니다.
4. **확인**을 클릭합니다.
5. Mac용 Visual Studio 다시 시작
6. 패키지를 지우고 다시 빌드합니다.
7. 빌드 출력 단추를 클릭 하 여 오류 패드 (**보기 > 패드 > 오류** ) 내에 진단 출력이 표시 됩니다.

## <a name="device-deployment-logs"></a>장치 배포 로그

Visual Studio 내에서 장치 배포 로깅을 사용 하도록 설정 하려면

1. **도구 > 옵션 ...** >
2. 왼쪽 트리 보기에서 **Xamarin > Android 설정** 을 선택 합니다.
3. 오른쪽 패널에서 [X] **확장 디버그 로깅 (바탕 화면에 monodroid 쓰기)** 확인란을 사용 하도록 설정 합니다.
4. 로그 메시지는 데스크톱의 monodroid 파일에 기록 됩니다.

Mac용 Visual Studio는 항상 장치 배포 로그를 작성 합니다. 찾기는 약간 더 어렵습니다. *AndroidUtils* 로그 파일은 배포가 발생 하는 일 수 + 시간 마다 생성 됩니다 (예: **Androidtools-2012-10 -24 _12-35 -45)** .

- Windows에서는 로그 파일이 `%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`에 기록 됩니다.
- OS X에서 로그 파일은 `$HOME/Library/Logs/XamarinStudio-{VERSION}`에 기록 됩니다.

## <a name="android-debug-log-output"></a>Android 디버그 로그 출력

Android는 [Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md)에 많은 메시지를 기록 합니다.
Xamarin android는 android 시스템 속성을 사용 하 여 Android 디버그 로그에 대 한 추가 메시지의 생성을 제어 합니다. Android Debug Bridge 내에서 *setprop* 명령을 통해 Android 시스템 속성을 설정할 수 있습니다 [(adb)](https://developer.android.com/guide/developing/tools/adb.html).

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

시스템 속성은 프로세스를 시작 하는 동안 읽혀집니다. 따라서 응용 프로그램을 시작 하기 전에 설정 하거나 시스템 속성을 변경한 후 응용 프로그램을 다시 시작 해야 합니다.

### <a name="xamarinandroid-system-properties"></a>Xamarin.Android 시스템 속성

Xamarin Android는 다음과 같은 시스템 속성을 지원 합니다.

- *debug. mono. debug*: 비어 있지 않은 문자열의 경우 `*mono-debug*`와 동일 합니다.

- *디버그. mono*: mono가 초기화 *되기 전에* 응용 프로그램 시작 중에 내보낼 환경 변수의 파이프로 구분 된 (' *|* ') 목록입니다. 이렇게 하면 mono 로깅을 제어 하는 환경 변수를 설정할 수 있습니다.

  > [!NOTE]
  > 값은 ' *|* '로 구분 되므로 \`*adb shell*\` 명령이 따옴표 집합을 제거 하기 때문에 값은 추가 수준의 따옴표를 포함 해야 합니다.

  > [!NOTE]
  > Android 시스템 속성 값의 길이는 92 자를 초과할 수 없습니다.

  예:

  ```
  adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"
  ```

- *디버그. mono. 로그*: Android 디버그 로그에 추가 메시지를 인쇄 해야 하는 구성 요소의 쉼표로 구분 된 (' *,* ') 목록입니다. 기본적으로는 아무것도 설정 되지 않습니다. 구성 요소는 다음과 같습니다.

  - *모든*: 모든 메시지를 인쇄 합니다.
  - *gc*: gc 관련 메시지를 인쇄 합니다.
  - *gref*: Print (weak, global) 참조 할당 및 할당 취소 메시지입니다.
  - *lref*: 로컬 참조 할당 및 할당 취소 메시지를 인쇄 합니다.

  > [!NOTE]
  > *매우* 자세한 정보를 표시 합니다. 반드시 필요한 경우가 아니면 사용 하지 마십시오.

- *debug. mono. trace*: [mono--trace](http://docs.go-mono.com/?link=man%3amono(1))`=PROPERTY_VALUE` 설정을 지정할 수 있습니다.

## <a name="deleting-bin-and-obj"></a>`bin` 및 `obj` 삭제

Xamarin Android는 다음과 같은 상황에서 과거에 발생 했습니다.

- 이상한 빌드 또는 런타임 오류가 발생 합니다.
- `bin` 및 `obj` 디렉터리를 `Clean`, `Rebuild`또는 수동으로 삭제 합니다.
- 문제가 사라집니다.

개발자 생산성에 미치는 영향 때문에 이러한 문제를 해결 하는 데 많은 투자를 드립니다.

이와 같은 문제가 발생 하는 경우:

1. 멘 탈 노트를 만듭니다. 프로젝트를이 상태로 전환 하는 마지막 작업은 무엇 인가요?
1. 현재 빌드 로그를 저장 합니다. 빌드를 다시 시도 하 고 [진단 빌드 로그](#diagnostic-msbuild-output)를 기록 합니다.
1. [버그 보고서][bug]를 제출 합니다.

`bin` 및 `obj` 디렉터리를 삭제 하기 전에 압축 하 고 필요한 경우 나중에 진단할 수 있도록 저장 합니다. 아마도 Xamarin Android 응용 프로그램 프로젝트를 `Clean` 하 여 작업을 다시 수행할 수 있습니다.

[bug]: https://github.com/xamarin/xamarin-android/wiki/Submitting-Bugs,-Feature-Requests,-and-Pull-Requests

## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin.ios가 System.valuetuple를 확인할 수 없습니다.

이 오류는 Visual Studio와의 비 호환성으로 인해 발생 합니다.

- **Visual Studio 2017 업데이트 1** (버전 15.1 또는 이전 버전)은 **system.valuetuple NuGet 4.3.0** (또는 이전 버전)와만 호환 됩니다.

- **Visual Studio 2017 업데이트 2** (버전 15.2 이상)는 **system.valuetuple NuGet 4.3.1** (또는 최신 버전)와만 호환 됩니다.

Visual Studio 2017 설치에 해당 하는 올바른 System.valuetuple NuGet을 선택 하세요.

## <a name="gc-messages"></a>GC 메시지

Gc 구성 요소 메시지는 gc를 포함 하는 값으로 설정 하 여 볼 수 있습니다.

Gc 메시지는 gc가 실행 될 때마다 생성 되며 GC에서 수행한 작업의 양에 대 한 정보를 제공 합니다.

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

`MONO_LOG_LEVEL` 환경 변수를 `debug`으로 설정 하 여 타이밍 정보와 같은 추가 GC 정보를 생성할 수 있습니다.

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

이렇게 하면 다음과 같은 세 가지 결과를 포함 하 여 (많은) 추가 Mono 메시지가 생성 됩니다.

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

`GC_BRIDGE` 메시지에서이 패스가 고려 하 고 있는 브리지 개체의 수는 `num-objects` 이며이 브리지 코드를 호출 하는 동안 처리 되는 개체의 수는 `num_hash_entries`입니다.

`GC_MINOR` 및 `GC_MAJOR` 메시지에서 `total`는 스레드가 실행 되 고 있지 않은 동안에는 시간이 소요 되는 반면, `bridge`는 연결 처리 코드 (Java VM을 처리 함)에서 소요 된 시간입니다. 브리지 처리가 발생 하는 동안 전 세계는 일시 중지 *되지 않습니다* .

 *일반적*으로 `num_hash_entries`값이 클수록 `bridge` 컬렉션에 소요 되는 시간이 증가 하 고 수집 하는 데 소요 되는 `total` 시간이 더 커집니다.

## <a name="global-reference-messages"></a>전역 참조 메시지

GREF (Global Reference loggig) 로깅을 사용 하도록 설정 하려면 *GREF*(예:)를 포함 해야 *합니다.*

```shell
adb shell setprop debug.mono.log gref
```

Xamarin.ios는 java 메서드를 호출할 때 java 인스턴스를 java에 제공 해야 하는 경우와 마찬가지로, Android 전역 참조를 사용 하 여 Java 인스턴스와 연결 된 관리 되는 인스턴스 간에 매핑을 제공 합니다.

불행 하 게도 Android 에뮬레이터는 2000 전역 참조를 한 번에 하나만 허용 합니다. 하드웨어는 52000 전역 참조의 제한이 훨씬 높습니다. 한도는 에뮬레이터에서 응용 프로그램을 실행할 때 문제가 될 수 있으므로 인스턴스가 제공 되는 *위치* 를 파악 하는 것이 매우 유용할 수 있습니다.

> [!NOTE]
> 전역 참조 횟수는 Xamarin.ios의 내부 이며 프로세스에 로드 된 다른 네이티브 라이브러리에서 수행 되는 전역 참조를 포함 하지 않습니다. 전역 참조 횟수를 예상 값으로 사용 합니다.

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

결과에는 다음과 같은 네 가지 메시지가 있습니다.

- 전역 참조 만들기: *+ g +* 로 시작 하는 줄 이며, 코드 생성 경로에 대 한 스택 추적을 제공 합니다.
- 전역 참조 소멸: *-g-* 로 시작 하는 줄 이며 전역 참조의 코드 경로 삭제에 대 한 스택 추적을 제공할 수 있습니다. GC가 gref를 삭제 하는 경우 스택 추적이 제공 되지 않습니다.
- 약한 전역 참조 만들기: *+ w +* 로 시작 하는 줄입니다.
- 약한 전역 참조 소멸: *-w-* 로 시작 하는 선입니다.

모든 메시지에서 *grefc* 값은 xamarin.ios가 만든 전역 참조의 수이 고 *Grefwc* 값은 xamarin.ios가 만든 약한 전역 참조의 수입니다. *핸들* 또는 *obj 핸들* 값은 JNI handle 값이 고 ' */* ' 뒤의 문자는 핸들 값의 형식입니다. 로컬 참조의 경우 */l* , 전역 참조의 경우 */g* , 약한 전역 참조의 경우 */w*

GC 프로세스의 일부로 전역 참조 (+ g +)는 약한 전역 참조 (+ w + 및-g-로 인해)로 변환 되 고, Java 쪽 GC가 시작 된 후 weak 전역 참조가 수집 되었는지 확인 합니다. 아직 활성 상태 이면 weak ref (+ g +,-w-)를 중심으로 새 gref이 생성 되 고, 그렇지 않으면 약한 참조가 제거 됩니다 (-w).

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Java 인스턴스가 생성 되 고 MCW에 의해 래핑됩니다.

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>GC를 수행 하는 동안 ...

```shell
I/monodroid-gref(27679): +w+ grefc 1953 gwrefc 259 obj-handle 0x4066df10/G -> new-handle 0xde68f95f/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -g- grefc 1952 gwrefc 259 handle 0x4066df10/G from take_weak_global_ref_jni
```

## <a name="object-is-still-alive-as-handle--null"></a>개체가 여전히 활성 상태입니다. 핸들! = null
## <a name="wref-turned-back-into-a-gref"></a>wref가 gref로 다시 설정 되었습니다.

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x4066df10
I/monodroid-gref(27679): +g+ grefc 1930 gwrefc 39 obj-handle 0xde68f95f/W -> new-handle 0x4066df10/G from take_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1930 gwrefc 38 handle 0xde68f95f/W from take_global_ref_jni
```

## <a name="object-is-dead-as-handle--null"></a>개체는 handle = = null로 작동 합니다.
## <a name="wref-is-freed-no-new-gref-created"></a>wref가 해제 되 고 새 gref 생성 되지 않음

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

여기서는 "흥미로운" 특이 한 점이 있습니다. 4.0 이전에 Android를 실행 하는 대상에서 gref 값은 Android 런타임의 메모리에 있는 Java 개체의 주소와 같습니다. 즉, GC는 이동 중이 아닌 일반 수집기 이며 해당 개체에 대 한 직접 참조를 처리 합니다. 따라서 + g +, + w +,-g-, + g +,-w 시퀀스 이후에는 결과 gref 원래 gref 값과 동일한 값을 갖게 됩니다. 이렇게 하면 grepping 로그를 매우 간단 하 게 만들 수 있습니다.

그러나 android 4.0에는 이동 수집기가 있으며, 더 이상 Android runtime VM 개체에 대 한 직접 참조를 전달 하지 않습니다. 따라서 + g +, + w +,-g-, + g +,-w 시퀀스 후에는 gref 값 *이 달라 집니다*. 개체가 여러 Gc에 영향을 주는 경우 여러 개의 gref 값으로 이동 하 여 인스턴스가 실제로 할당 된 위치를 확인 하기가 더 어려워집니다.

### <a name="querying-programmatically"></a>프로그래밍 방식으로 쿼리

`JniRuntime` 개체를 쿼리하여 GREF 및 WREF 개수를 모두 쿼리할 수 있습니다.

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount`-전역 참조 수

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount`-Weak 참조 횟수

## <a name="android-debug-logs"></a>Android 디버그 로그

[Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md) 는 표시 되는 모든 런타임 오류에 대 한 추가 컨텍스트를 제공할 수 있습니다.

## <a name="floating-point-performance-is-terrible"></a>부동 소수점 성능은 매우 형편 없습니다.

또는 "내 앱은 릴리스 빌드를 사용 하는 것 보다 디버그 빌드를 사용 하 여 더 빠르게 10 배 실행 됩니다."

Xamarin.ios는 *armeabi-v7a*, *armeabi-v7a-armeabi-v7a*및 *x 86*과 같은 여러 장치를 지원 합니다. Device ABIs는 **프로젝트 속성 > 응용 프로그램 탭 내에서 지원 되는 아키텍처 >** 지정할 수 있습니다.

디버그 빌드는 모든 ABIs를 제공 하는 Android 패키지를 사용 하므로 대상 장치에 대 한 가장 빠른 ABI를 사용 하 게 됩니다.

릴리스 빌드는 프로젝트 속성 탭에서 선택 된 ABIs만 포함 합니다. 둘 이상의를 선택할 수 있습니다.

*armeabi-v7a* 는 기본 ABI 이며 가장 광범위 한 장치를 지원 합니다. *그러나*armeabi-v7a는 다중 CPU 장치 및 하드웨어 부동 소수점을 지원 하지 않습니다. amont 기타 작업을 수행 합니다. 따라서 armeabi-v7a 릴리스 런타임을 사용 하는 앱은 단일 코어에 연결 되며 소프트 부동 소수점 구현을 사용 합니다. 이러한 두 가지 모두 응용 프로그램의 성능을 크게 저하 시킬 수 있습니다.

앱에 적절 한 부동 소수점 성능 (예: 게임)이 필요한 경우 *armeabi-Armeabi-v7a* ABI를 사용 하도록 설정 해야 합니다. *Armeabi-v7a-armeabi-v7a* runtime만 지원할 수 있습니다. 즉, *armeabi-v7a* 만 지 원하는 이전 장치에서는 앱을 실행할 수 없습니다.

## <a name="could-not-locate-android-sdk"></a>Android SDK를 찾을 수 없습니다.

Google에서 Windows 용 Android SDK에 대해 2 개의 다운로드를 사용할 수 있습니다.
.Exe 설치 관리자를 선택 하는 경우 설치 된 위치에 Xamarin.ios를 지시 하는 레지스트리 키를 작성 합니다. .Zip 파일을 선택 하 고 압축을 풀면 Xamarin.ios는 SDK를 찾을 위치를 알 수 없습니다. **Xamarin > Android 설정 > 도구 > 옵션**으로 이동 하 여 SDK가 Visual Studio에 있는 경우 xamarin.ios에 지시할 수 있습니다.

[Xamarin Android 설정에서 ![Android SDK 위치](troubleshooting-images/01.png)](troubleshooting-images/01.png#lightbox)

## <a name="ide-does-not-display-target-device"></a>IDE에서 대상 장치를 표시 하지 않음

경우에 따라 장치에 응용 프로그램을 배포 하려고 하지만 배포 하려는 장치가 장치 선택 대화 상자에 표시 되지 않습니다. 이 문제는 Android Debug Bridge 휴가로 이동 하기로 결정 하는 경우에 발생할 수 있습니다.

이 문제를 진단 하려면 [adb 프로그램](~/android/deploy-test/debugging/android-debug-log.md)을 찾은 후 다음을 실행 합니다.

```shell
adb devices
```

장치가 존재 하지 않는 경우 장치를 찾을 수 있도록 Android Debug Bridge 서버를 다시 시작 해야 합니다.

```shell
adb kill-server
adb start-server
```

HTC Sync 소프트웨어로 인해 **adb 시작 서버가** 제대로 작동 하지 않을 수 있습니다. **Adb 시작-서버** 명령이 시작 중인 포트를 인쇄 하지 않는 경우 HTC Sync 소프트웨어를 종료 하 고 adb 서버를 다시 시작 해 보세요.

## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>지정한 작업 실행 파일 "keytool"을 (를) 실행할 수 없습니다.

즉, 경로에 Java SDK bin 디렉터리가 있는 디렉터리가 포함 되어 있지 않습니다. [설치](~/android/get-started/installation/index.md) 가이드의 단계를 따르고 있는지 확인 합니다.

## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>monodroid 또는 aresgen가 코드 1로 종료 되었습니다.

이 문제를 디버깅 하는 데 도움이 되도록 Visual Studio로 이동 하 여 MSBuild의 자세한 정도를 변경 합니다 .이 작업을 수행 하려면 **도구 > 옵션 > 프로젝트** 및 **솔루션 > 빌드** 및 **실행 > MSBuild 프로젝트 빌드 출력의 자세한 정도** 를 선택 하 고이 값을 **Normal**으로 설정 합니다.

다시 빌드하고 전체 오류를 포함 하는 Visual Studio의 출력 창을 확인 합니다.

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>장치에 패키지를 배포 하는 데 필요한 저장소 공간이 부족 합니다.

이는 Visual Studio 내에서 에뮬레이터를 시작 하지 않은 경우에 발생 합니다. Visual Studio 외부에서 에뮬레이터를 시작할 때 `-partition-size 512` 옵션 (예:)을 전달 해야 합니다.

```shell
emulator -partition-size 512 -avd MonoDroid
```

[시뮬레이터를 구성할 때 사용한 이름](~/android/get-started/installation/windows.md#device)(예: 올바른 시뮬레이터 이름)을 사용 해야 합니다.

## <a name="install_failed_invalid_apk-when-installing-a-package"></a>패키지를 설치 하는 동안 잘못 된\_APK\_\_설치 하지 못했습니다.

Android 패키지 이름은 마침표 (' *.* ')를 포함 *해야* 합니다. 마침표를 포함 하도록 패키지 이름을 편집 합니다.

- Visual Studio 내에서:
  - 프로젝트 > 속성을 마우스 오른쪽 단추로 클릭 합니다.
  - 왼쪽의 Android 매니페스트 탭을 클릭 합니다.
  - 패키지 이름 필드를 업데이트 합니다.
    - 메시지 &ldquo;표시 되는 경우에는 AndroidManifest을 찾을 수 없습니다. 하나를 클릭 하 여 추가 합니다.&rdquo;링크를 클릭 한 다음 패키지 이름 필드를 업데이트 합니다.
- Mac용 Visual Studio 내에서:
  - 프로젝트 > 옵션을 마우스 오른쪽 단추로 클릭 합니다.
  - 빌드/Android 응용 프로그램 섹션으로 이동 합니다.
  - 패키지 이름 필드를 '. '를 포함 하도록 변경 합니다.

## <a name="install_failed_missing_shared_library-when-installing-a-package"></a>패키지를 설치할 때\_공유\_라이브러리가 누락\_\_설치 하지 못했습니다.

이 컨텍스트의 "공유 라이브러리"는*libfoo.so*(네이티브 공유 라이브러리) 파일이 *아닙니다* . 대신 Google Maps와 같이 대상 장치에 별도로 설치 해야 하는 라이브러리입니다.

Android 패키지는 `<uses-library/>` 요소에 필요한 공유 라이브러리를 지정 합니다. *필요한* 라이브러리가 대상 장치에 없는 경우 (`//uses-library/@android:required` 예: 기본값은 *True*임) *설치\_실패\_\_공유\_라이브러리가 누락*된 경우 패키지 설치가 실패 합니다.

필요한 공유 라이브러리를 확인 하려면 *생성* 된
**androidmanifest** 파일 (예: **obj\\Debug\\Android\\androidmanifest .xml**)을 보고 `<uses-library/>` 요소를 찾습니다. `<uses-library/>` 요소는 프로젝트의 **속성\\AndroidManifest .xml** 파일에 수동으로 추가 하 고, [사용자 지정 특성](xref:Android.App.UsesLibraryAttribute)을 통해 사용할 수 있습니다.

예를 들어 *GoogleMaps* 에 어셈블리 참조를 추가 하면 Google Maps 공유 라이브러리에 대 한 `<uses-library/>`이 암시적으로 추가 됩니다.

## <a name="install_failed_update_incompatible-when-installing-a-package"></a>패키지를 설치할 때 호환 되지\_업데이트\_\_설치 하지 못했습니다.

Android 패키지에는 세 가지 요구 사항이 있습니다.

- '. '를 포함 해야 합니다. (이전 항목 참조)
- 고유한 문자열 패키지 이름을 포함 해야 합니다 (따라서 Android 앱 이름에 표시 되는, 즉 Chrome 앱에 대 한 com. android. chrome).
- 패키지를 업그레이드 하는 경우 패키지에 동일한 서명 키가 있어야 합니다.

따라서 다음 시나리오를 가정 합니다.

1. 앱을 빌드 & 디버그 앱으로 배포 합니다.
2. 서명 키를 변경 합니다. 예를 들어,를 릴리스 앱으로 사용 하거나 기본 제공 디버그 서명 키를 사용 하지 않는 것이 좋습니다.
3. 응용 프로그램을 먼저 제거 하지 않고 설치 합니다. 예를 들어 Visual Studio 내에서 디버깅을 사용 하지 않고 디버그 > 시작 합니다.

이 경우 패키지는 서명 키가 변경 되지 않았기 때문에 설치\_실패\_업데이트\_호환 되지 않는 오류가 발생 하 여 패키지 설치가 실패 합니다. [Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md) 에는 다음과 유사한 메시지도 포함 됩니다.

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

이 오류를 해결 하려면 다시 설치 하기 전에 장치에서 응용 프로그램을 완전히 제거 합니다.

## <a name="install_failed_uid_changed-when-installing-a-package"></a>패키지를 설치할 때\_UID\_변경\_설치 하지 못했습니다.

Android 패키지를 설치 하면 UID ( *사용자 id* )가 할당 됩니다.
현재 알 수 없는 이유로 이미 설치 된 앱을 통해 설치 하 *는 경우 설치가*실패 하는 `INSTALL_FAILED_UID_CHANGED`:

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

이 문제를 해결 하려면 Android 대상의 GUI에서 앱을 설치 하거나 `adb`를 사용 하 여 Android 패키지를 *완전히 제거* 합니다.

```shell
$ adb uninstall @PACKAGE_NAME@
```

응용 프로그램 데이터를 *유지* 하 고 대상 장치에서 충돌 하는 UID를 유지 하므로 `adb uninstall -k`를 **사용 하지 마세요** .

## <a name="release-apps-fail-to-launch-on-device"></a>장치에서 릴리스 앱이 시작 되지 않음

Android 디버그 로그 출력은 다음과 유사한 메시지를 포함 합니다.

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

그렇다면 다음과 같은 두 가지 원인이 있을 수 있습니다.

1. . Apk는 대상 장치에서 지 원하는 ABI를 제공 하지 않습니다.
    예를 들어 .apk에는 armeabi-armeabi-v7a 이진 파일만 포함 되 고 대상 장치는 armeabi만 지원 합니다.

2. [Android 버그](https://code.google.com/p/android/issues/detail?id=21670). 이 경우 앱을 제거 하 고 손가락을 교차 하 고 앱을 다시 설치 합니다.

(1)를 수정 하려면 프로젝트 옵션/속성을 편집 하 고 [필요한 ABI에 대 한 지원을 지원 되는 ABIs 목록에 추가](~/android/app-fundamentals/cpu-architectures.md)합니다. 추가 해야 하는 ABI를 결정 하려면 대상 장치에 대해 다음 adb 명령을 실행 합니다.

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

출력에는 기본 (및 선택적인 보조)이 포함 됩니다.

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>Project &ldquo;MyApp .csproj&rdquo;에 대해 OutPath 속성이 설정 되지 않았습니다.

이는 일반적으로 HP 컴퓨터와 환경 변수 &ldquo;Platform&rdquo;가 MCD 또는 HPD와 같이 설정 된 것을 의미 합니다. 이는 일반적으로 &ldquo;모든 CPU&rdquo; 또는 &ldquo;x86&rdquo;로 설정 된 MSBuild Platform 속성과 충돌 합니다. MSBuild를 작동 하려면 컴퓨터에서이 환경 변수를 제거 해야 합니다.

- 제어판 > System > 고급 > 환경 변수

Visual Studio 또는 Mac용 Visual Studio을 다시 시작 하 고 다시 빌드 해 보세요. 이제 작업이 예상 대로 작동 합니다.

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>ClassCastException: JavaObject를 ...로 캐스팅할 수 없습니다..

Xamarin Android 4.x에서는 중첩 된 제네릭 형식을 제대로 마샬링할 필요가 없습니다. 예를 들어 [SimpleExpandableListAdapter](xref:Android.Widget.SimpleExpandableListAdapter)를 사용 하는 다음 C\# 코드를 고려 합니다.

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

문제는 Xamarin.ios에서 중첩 된 제네릭 형식을 잘못 마샬링하는 것입니다. 이 `List<IDictionary<string, object>>`은 [java.lang.ArrrayList](xref:Java.Util.ArrayList)로 마샬링될 때, `ArrayList`는 `mono.android.runtime.JavaObject`java.util.Map`Dictionary<string, object>`을 구현 하는 항목 대신 [인스턴스를 참조 하는 ](xref:Java.Util.IMap) 인스턴스를 포함 합니다. 다음 예외가 발생 합니다.

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

해결 방법은 [내부](~/android/internals/api-design.md) 형식에대한 `System.Collections.Generic`형식 대신 제공된 &ldquo;Java 컬렉션 형식&rdquo;을 사용하는 것 입니다. 그러면 인스턴스를 마샬링할 때 적절 한 Java 형식이 생성 됩니다. (다음 코드는 gref 수명을 줄이기 위해 필요한 것 보다 더 복잡 합니다. `s/List/JavaList/g`를 통해 원래 코드를 변경 하는 것이 간단 하 고 gref 수명이 염려 되지 않는 경우 `s/Dictionary/JavaDictionary/g` 수 있습니다.

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

[이 문제는 향후 릴리스에서 수정 될 예정](https://bugzilla.xamarin.com/show_bug.cgi?id=5401)입니다.

## <a name="unexpected-nullreferenceexceptions"></a>예기치 않은 NullReferenceExceptions

경우에 따라 [Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md) 에는 &ldquo;수 없는 NullReferenceExceptions가 있습니다 .이 예외는 앱이 중단 되기 직전에 android 런타임 코드를&rdquo; 하거나 Mono에서 제공 됩니다.

```shell
E/mono(15202): Unhandled Exception: System.NullReferenceException: Object reference not set to an instance of an object
E/mono(15202):   at Java.Lang.Object.GetObject (IntPtr handle, System.Type type, Boolean owned)
E/mono(15202):   at Java.Lang.Object._GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Java.Lang.Object.GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Android.Views.View+IOnTouchListenerAdapter.n_OnTouch_Landroid_view_View_Landroid_view_MotionEvent_(IntPtr jnienv, IntPtr native__this, IntPtr native_v, IntPtr native_e)
E/mono(15202):   at (wrapper dynamic-method) object:b039cbb0-15e9-4f47-87ce-442060701362 (intptr,intptr,intptr,intptr)
```

로 구분하거나 여러

```shell
E/mono    ( 4176): Unhandled Exception:
E/mono    ( 4176): System.NullReferenceException: Object reference not set to an instance of an object
E/mono    ( 4176): at Android.Runtime.JNIEnv.NewString (string)
E/mono    ( 4176): at Android.Util.Log.Info (string,string)
```

이 문제는 Android 런타임이 프로세스를 중단 하기로 결정 한 경우에 발생할 수 있습니다 .이는 대상의 GREF 제한에 도달 하거나 JNI를 사용 하 여 잘못 된&rdquo; &ldquo;하는 것을 포함 하 여 여러 가지 이유로 발생할 수 있습니다.

이 경우에 해당 하는지 확인 하려면 다음과 유사한 프로세스에서 메시지에 대 한 Android 디버그 로그를 확인 합니다.

```shell
E/dalvikvm(  123): VM aborting
```

## <a name="abort-due-to-global-reference-exhaustion"></a>전역 참조 고갈로 인해 중단 됩니다.

Android 런타임의 JNI 계층은 지정 된 시점에 유효 해야 하는 제한 된 수의 JNI 개체 참조만 지원 합니다. 이 제한을 초과 하면 작업이 중단 됩니다.

GREF (*전역 참조*) 제한은 에뮬레이터에서 2000 참조 이며 하드웨어에 대 한 ~ 52000 참조입니다.

Android 디버그 로그에 다음과 같은 메시지가 표시 될 때 GREFs를 너무 많이 만들도록 시작 하 고 있음을 알 수 있습니다.

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

위의 예제 (여기서는 [버그 685215](https://bugzilla.novell.com/show_bug.cgi?id=685215)에서 제공 됨)에서 문제가 너무 많은 Android. Point 인스턴스가 생성 되 고 있는 것입니다. 이 특정 버그의 수정 목록은 [설명 \#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2) 를 참조 하세요.

일반적으로 유용 하 게 사용할 수 있는 솔루션은 위의 덤프 &ndash;에서 &ndash; 할당 된 인스턴스가 너무 많은 형식을 찾은 다음 소스 코드에서 생성 된 위치를 찾아서 적절 하 게 삭제 하는 것입니다 (Java 개체 수명이 단축 됨). 이는 항상 적절 한 것은 아닙니다. (\#685215은 다중 스레드 이므로 trivial 솔루션은 Dispose 호출을 방지 하지만 가장 먼저 고려해 야 할 사항입니다.

[GREF 로깅을](~/android/troubleshooting/index.md) 사용 하도록 설정 하 여 grefs가 생성 된 시기와 존재 하는 수를 확인할 수 있습니다.

## <a name="abort-due-to-jni-type-mismatch"></a>JNI 형식 불일치로 인해 중단 됩니다.

JNI 코드를 직접 롤백하는 경우 형식이 올바르게 일치 하지 않을 수 있습니다 (예: `java.lang.Runnable`를 구현 하지 않는 형식에서 `java.lang.Runnable.run`를 호출 하려고 하는 경우). 이 문제가 발생 하면 Android 디버그 로그에 다음과 유사한 메시지가 나타납니다.

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>동적 코드 지원

### <a name="dynamic-code-does-not-compile"></a>동적 코드는 컴파일되지 않습니다.

응용 프로그램 또는 라이브러리에서 C\# 동적을 사용 하려면 프로젝트에 system.string, Microsoft CSharp 및 Mono .dll을 추가 해야 합니다.

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>릴리스 빌드에서 MissingMethodException는 런타임에 동적 코드에 대해 발생 합니다.

- 응용 프로그램 프로젝트에는 System.object, Microsoft CSharp 또는 Mono .dll에 대 한 참조가 없을 수 있습니다. 해당 어셈블리가 참조 되는지 확인 합니다.

  - 동적 코드는 항상 비용을 지불 해야 합니다. 효율적인 코드가 필요한 경우 동적 코드를 사용 하지 않는 것이 좋습니다.

- 첫 번째 미리 보기에서는 각 어셈블리의 형식을 응용 프로그램 코드에서 명시적으로 사용 하지 않는 한 해당 어셈블리가 제외 되었습니다. 해결 방법은 다음을 참조 하십시오. [http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)

## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>X86 장치에서 AOT + LLVM crash로 빌드된 프로젝트

X86 기반 장치에서 [AOT + LLVM](~/android/deploy-test/release-prep/index.md) 를 사용 하 여 빌드한 앱을 배포 하는 경우 다음과 유사한 예외 오류 메시지가 표시 될 수 있습니다.

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (amarin.bug56111)
```

[56111](https://bugzilla.xamarin.com/show_bug.cgi?id=56111)에 보고 된 알려진 문제입니다. 해결 방법은 LLVM을 사용 하지 않도록 설정 하는 것입니다.
