---
title: 문제 해결 팁
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/15/2018
ms.openlocfilehash: 8855c80dd7478813408abaf2cfec68d48eced3bc
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571833"
---
# <a name="troubleshooting-tips"></a>문제 해결 팁

## <a name="getting-diagnostic-information"></a>진단 정보 가져오기

Xamarin.Android에는 각종 버그를 추적할 때 몇 군데의 장소를 살펴봅니다.
여기에는 다음이 포함됩니다.

1. 진단 MSBuild 출력.
2. 디바이스 배포 로그.
3. Android 디버그 로그 출력.

<a name="Diagnostic_MSBuild_Output"></a>

## <a name="diagnostic-msbuild-output"></a>진단 MSBuild 출력

진단 MSBuild는 패키지 빌드와 관련된 추가 정보와 얼마간의 패키지 배포 정보를 포함할 수 있습니다.

Visual Studio 내에서 진단 MSBuild 출력을 사용하려면:

1. **도구 > 옵션...** 을 클릭합니다.
2. 왼쪽 트리 뷰에서 **프로젝트 및 솔루션 > 빌드 및 실행**을 선택합니다.
3. 오른쪽 패널에서 [MSBuild 빌드 출력의 자세한 정도] 드롭다운을 [진단]으로 설정합니다.
4. **확인** 을 클릭합니다.
5. 패키지를 지우고 다시 빌드합니다.
6. 출력 패널에 진단 출력이 표시됩니다.

Mac/OS X용 Visual Studio 내에서 진단 MSBuild 출력을 사용하려면:

1. **Mac용 Visual Studio > 기본 설정...** 을 클릭합니다.
2. 왼쪽 트리 뷰에서 **프로젝트 > 빌드** 를 선택합니다.
3. 오른쪽 패널에서 [로그 자세한 정도] 드롭다운을 [진단]으로 설정합니다.
4. **확인** 을 클릭합니다.
5. Mac용 Visual Studio 다시 시작
6. 패키지를 지우고 다시 빌드합니다.
7. [빌드 출력] 단추를 클릭하면 오류 패드(**보기 > 패드 > 오류**)에 진단 출력이 표시됩니다.

## <a name="device-deployment-logs"></a>디바이스 배포 로그

Visual Studio 내에서 디바이스 배포 로깅을 사용하려면:

1. **도구 > 옵션...** >을 클릭합니다.
2. 왼쪽 트리 뷰에서 **Xamarin > Android 설정**을 선택합니다.
3. 오른쪽 패널에서 [X]  **확장 디버그 로깅(데스크톱에 monodroid.log 작성)** 확인란을 선택합니다.
4. 데스크톱의 monodroid.log 파일에 로그 메시지가 기록됩니다.

Mac용 Visual Studio는 항상 디바이스 배포 로그를 기록합니다. 로그를 찾는 방법은 약간 까다로운데, 매일, 그리고 배포가 이루어질 때마다 *AndroidUtils* 로그 파일(예: **AndroidTools-2012-10-24_12-35-45.log**)이 만들어집니다.

- Windows에서는 로그 파일이 `%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`에 기록됩니다.
- OS X에서는 로그 파일이 `$HOME/Library/Logs/XamarinStudio-{VERSION}`에 기록됩니다.

## <a name="android-debug-log-output"></a>Android 디버그 로그 출력

Android는 [Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md)에 많은 메시지를 기록합니다.
Xamarin.Android는 Android 시스템 속성을 사용하여 Android 디버그 로그로의 추가 메시지 생성을 제어합니다. Android 시스템 속성은 [Android Debug Bridge(adb)](https://developer.android.com/guide/developing/tools/adb.html) 내에서 다음과 같이 *setprop* 명령을 사용하여 설정할 수 있습니다.

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

시스템 속성은 프로세스가 시작할 때 읽히므로 애플리케이션이 시작되기 전에 설정하거나 시스템 속성을 변경한 후에 애플리케이션을 다시 시작해야 합니다.

### <a name="xamarinandroid-system-properties"></a>Xamarin.Android 시스템 속성

Xamarin.Android는 다음과 같은 시스템 속성을 지원합니다.

- *debug.mono.debug*: 빈 문자열이 아닌 경우 이는 `*mono-debug*`와 동일합니다.

- *debug.mono.env*: 파이프 문자(‘ *|* ’)로 구분된 환경 변수로, 애플리케이션을 시작할 때 mono가 초기화되기 ‘전에’ 내보냅니다. 이렇게 하면 mono 로깅을 제어하는 환경 변수를 설정할 수 있습니다.

  > [!NOTE]
  > 값이 ‘ *|* ’ 문자로 구분되며 \`*adb shell*\` 명령은 따옴표 쌍을 제거하므로 값에 따옴표를 한 수준 더 사용해야 합니다.

  > [!NOTE]
  > Android 시스템 속성은 길이가 92자를 넘을 수 없습니다.

  예:

  ```
  adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"
  ```

- *debug.mono.log*: Android 디버그 로그에 추가 메시지를 출력하는 쉼표(‘ *,* ’)로 구분된 목록입니다. 기본적으로 아무것도 설정되지 않습니다. 구성 요소에는 다음이 포함됩니다.

  - *all*: 모든 메시지를 출력합니다.
  - *gc*: GC 관련 메시지를 출력합니다.
  - *gref*: 약한 전역 참조 할당 및 할당 취소 메시지를 출력합니다.
  - *lref*: 로컬 참조 할당 및 할당 취소 메시지를 출력합니다.

  > [!NOTE]
  > 세부 정보가 ‘매우 많이’ 표시됩니다. 반드시 필요한 경우가 아닌 이상 사용하도록 설정하지 마세요.

- *debug.mono.trace*: [mono --trace](http://docs.go-mono.com/?link=man%3amono(1))`=PROPERTY_VALUE` 설정의 설정을 허용합니다.

## <a name="deleting-bin-and-obj"></a>`bin` 및 `obj` 삭제

이제까지 Xamarin.Android에서 다음과 같은 상황이 발생하는 경우가 있었습니다.

- 알 수 없는 빌드 또는 런타임 오류가 발생합니다.
- 사용자가 `bin` 및 `obj` 디렉터리를 `Clean`, `Rebuild` 또는 수동으로 삭제합니다.
- 문제가 사라집니다.

이러한 문제가 개발자 생산성에 미치는 영향이 큰 만큼 Microsoft는 이러한 문제를 수정하기 위해 끊임없이 노력합니다.

이러한 문제가 발생할 경우:

1. 기억해 보세요. 프로젝트가 이 상태가 되기 직전에 수행한 작업은 무엇이었나요?
1. 현재 빌드 로그를 저장합니다. 빌드를 다시 시도하고 [진단 빌드 로그](#diagnostic-msbuild-output)를 기록합니다.
1. [버그 보고서][bug]를 제출합니다.

`bin` 및 `obj` 디렉터리를 삭제하기 전에 먼저 두 디렉터리를 압축하고 나중에 필요할 경우 진단에서 사용할 수 있도록 저장합니다. 대부분의 경우 Xamarin.Android 애플리케이션 프로젝트를 `Clean`하기만 해도 다시 진행할 수 있습니다.

[bug]: https://github.com/xamarin/xamarin-android/wiki/Submitting-Bugs,-Feature-Requests,-and-Pull-Requests

## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin.Android가 System.ValueTuple을 확인할 수 없음

이 오류는 Visual Studio와의 비호환성으로 인해 발생합니다.

- **Visual Studio 2017 업데이트 1**(버전 15.1 및 이전 버전)은 **System.ValueTuple NuGet 4.3.0**(및 이전 버전)하고만 호환됩니다.

- **Visual Studio 2017 업데이트 2**(버전 15.2 및 이후 버전)는 **System.ValueTuple NuGet 4.3.1**(및 이후 버전)하고만 호환됩니다.

사용 중인 Visual Studio 2017 설치와 호환되는 올바른 System.ValueTuple NuGet을 선택하세요.

## <a name="gc-messages"></a>GC 메시지

GC 구성 요소 메시지는 debug.mono.log 시스템 속성을 gc를 포함하는 값으로 설정하여 확인할 수 있습니다.

GC 메시지는 GC가 실행될 때마다 생성되며, GC가 얼마나 많은 작업을 수행했는지에 관한 정보를 제공합니다.

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

타이밍 정보와 같은 추가 GC 정보는 `MONO_LOG_LEVEL` 환경 변수를 `debug`로 설정하여 생성할 수 있습니다.

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

이렇게 하면 다음과 같은 세 개의 중요한 메시지를 포함하여 매우 많은 추가 Mono 메시지가 표시됩니다.

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

`GC_BRIDGE` 메시지에서 `num-objects`는 이 통과에서 고려 중인 브리지 개체의 개수이고 `num_hash_entries`는 브리지 코드의 이번 호출에서 처리된 개체의 개수입니다.

`GC_MINOR` 및 `GC_MAJOR` 메시지에서 `total`은 전역 일시 중지된(실행 중인 스레드가 없는) 지속 시간이고 `bridge`는 (Java VM을 처리하는) 브리지 처리 코드에서 소요된 시간입니다. 브리지 처리가 진행 중인 동안에는 전역 중지되지 ‘않습니다’.

 ‘일반적으로’ `num_hash_entries`의 값이 클수록 `bridge` 수집에 소요되는 시간이 길어지고 수집하는 데 소요되는 `total` 시간이 길어집니다.

## <a name="global-reference-messages"></a>전역 참조 메시지

GREF(전역 참조) 로깅을 사용하려면 *debug.mono.log* 시스템 속성이 다음과 같이 *gref*를 포함해야 합니다.

```shell
adb shell setprop debug.mono.log gref
```

Xamarin.Android는 Java 메서드를 호출할 때 Java에 Java 인스턴스를 제공해야 하는 경우에서처럼 Java 인스턴스와 관련 관리형 인스턴스 사이의 매핑을 제공하기 위해 Android 전역 참조를 사용합니다.

하지만 Android 에뮬레이터에서는 한 번에 2000개의 전역 참조만 존재하도록 허용합니다. 하드웨어에는 이보다 훨씬 큰 52000개의 전역 참조 제한이 적용됩니다. 에뮬레이터에서 애플리케이션을 실행할 때 하드웨어보다 작은 제한이 문제가 될 수 있으므로 인스턴스가 ‘어디에서’ 왔는지 아는 것이 매우 유용할 수 있습니다.

> [!NOTE]
> 전역 참조 개수는 Xamarin.Android 내부적으로 적용되며, 프로세스에 로드된 다른 네이티브 라이브러리에서 사용하는 전역 참조는 포함하지 않으며 포함할 수 없습니다. 전역 참조 개수는 추정용으로 사용하세요.

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

다음과 같은 네 개의 중요한 메시지가 있습니다.

- 전역 참조 생성: *+g+* 로 시작하는 줄로, 생성하는 코드 경로를 위한 스택 추적을 제공합니다.
- 전역 참조 소멸: *-g-* 로 시작하는 줄로, 전역 참조를 폐기하는 코드 경로를 위한 스택 추적을 제공할 수 있습니다. GC가 gref를 폐기하는 경우 스택 추적이 제공되지 않습니다.
- 약한 전역 참조 생성: *+w+* 로 시작하는 줄입니다.
- 약한 전역 참조 소멸: *-w-* 로 시작하는 줄입니다.

모든 메시지에서, *grefc* 값은 Xamarin.Android가 만든 전역 참조의 개수이고, *grefwc* 값은 Xamarin.Android가 만든 약한 전역 참조의 개수입니다. *handle* 또는 *obj-handle* 값은 JNI 핸들 값이고, ‘ */* ’ 뒤에 오는 문자는 핸들 값의 유형입니다. */L*은 로컬 참조, */G*는 전역 참조, */W*는 약한 전역 참조를 나타냅니다.

GC 프로세스의 일환으로, 전역 참조(+g+)는 약한 전역 참조로 변환되고(이로 인해 +w+와 -g-가 나타남), Java쪽 GC가 시작되고, 약한 전역 참조가 수집되었는지 확인됩니다. 아직 활성 상태이면 약한 참조 주변에 새 gref가 생성되고(+g+, -w-), 활성 상태가 아니면 약한 참조가 폐기됩니다(-w).

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>MCW에 의해 Java 인스턴스가 생성되고 래핑됨

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>GC가 수행됨...

```shell
I/monodroid-gref(27679): +w+ grefc 1953 gwrefc 259 obj-handle 0x4066df10/G -> new-handle 0xde68f95f/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -g- grefc 1952 gwrefc 259 handle 0x4066df10/G from take_weak_global_ref_jni
```

## <a name="object-is-still-alive-as-handle--null"></a>핸들 != null이므로 개체가 아직 활성 상태임
## <a name="wref-turned-back-into-a-gref"></a>wref가 다시 gref로 변환됨

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x4066df10
I/monodroid-gref(27679): +g+ grefc 1930 gwrefc 39 obj-handle 0xde68f95f/W -> new-handle 0x4066df10/G from take_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1930 gwrefc 38 handle 0xde68f95f/W from take_global_ref_jni
```

## <a name="object-is-dead-as-handle--null"></a>핸들 == null이므로 개체가 활성 상태가 아님
## <a name="wref-is-freed-no-new-gref-created"></a>wref가 해제되고 새 gref가 생성되지 않음

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

여기서 한 가지 “재미있는” 사실을 볼 수 있습니다. 4.0 이전의 Android를 실행 중인 대상에서 gref 값은 Android 런타임의 메모리에 있는 Java 개체의 주소와 같습니다. (즉, GC는 이동하지 않는 보수적인 수집기이며 해당 개체에 직접 참조를 전달합니다.) 따라서 +g+, +w+, -g-, +g+, -w- 시퀀스 후에 결과로 나오는 gref는 원래 gref 값과 동일한 값을 갖습니다. 이로 인해 로그 문자열을 찾기가 비교적 쉬워집니다.

그러나 Android 4.0은 이동하는 수집기를 가지며 더 이상 Android 런타임 VM 개체에 직접 참조를 전달하지 않습니다. 따라서 +g+, +w+, -g-, +g+, -w- 시퀀스 후에는 gref 값이 ‘달라집니다’. 여러 번의 GC 후에도 개체가 존속할 경우 여러 개의 gref 값을 갖게 되므로 인스턴스가 실제로 어디에서 할당되었는지 확인하기가 어려워집니다.

### <a name="querying-programmatically"></a>프로그래밍 방식으로 쿼리

`JniRuntime` 개체를 쿼리하여 GREF 개수와 WREF 개수를 쿼리할 수 있습니다.

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount` - 전역 참조 개수

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount` - 약한 참조 개수

## <a name="android-debug-logs"></a>Android 디버그 로그

[Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md)는 지금 보고 있는 런타임 오류에 관한 추가 컨텍스트를 제공할 수 있습니다.

## <a name="floating-point-performance-is-terrible"></a>부동 소수점 성능이 매우 좋지 않습니다!

또는 “내 앱의 릴리스 빌드보다 디버그 빌드가 10배 빨리 실행됩니다!”

Xamarin.Android는 *armeabi*, *armeabi-v7a*, *x86*과 같은 여러 디바이스 ABI를 지원합니다. 디바이스 ABI는 **프로젝트 속성 > 애플리케이션 탭 > 지원되는 아키텍처**에서 지정할 수 있습니다.

디버그 빌드는 모든 ABI를 제공하는 Android 패키지를 사용하므로 대상 디바이스에서 가장 빠른 ABI를 사용합니다.

릴리스 빌드는 프로젝트 속성 탭에서 선택한 ABI만 포함합니다. ABI는 하나 또는 그 이상을 선택할 수 있습니다.

기본 ABI는 *armeabi*로, 가장 많은 디바이스를 지원합니다. ‘그러나’ armeabi는 다중 CPU 디바이스 및 하드웨어 부동 소수점 등을 지원하지 않습니다. 따라서 릴리스 런타임에서 armeabi를 사용하는 앱은 단일 코어에 종속되고 soft-float 구현을 사용하게 됩니다. 이 두 가지 문제는 앱의 성능을 대폭 저하할 수 있습니다.

앱에 양호한 부동 소수점 성능이 필요한 경우(예: 게임 앱) *armeabi-v7a* ABI를 사용하도록 설정해야 합니다. *armeabi-v7a* 런타임만 지원하도록 할 수 있으나, 이렇게 하면 *armeabi*만 지원하는 구형 디바이스에서 앱을 실행할 수 없습니다.

## <a name="could-not-locate-android-sdk"></a>Android SDK를 찾을 수 없음

Google에서 제공하는 Windows용 Android SDK 다운로드에는 두 가지가 있습니다.
.exe 설치 프로그램을 선택한 경우, 설치 프로그램은 Xamarin.Android에 설치 위치를 알려 주는 레지스트리 키를 기록합니다. .zip 파일을 선택하여 직접 압축을 푼 경우, Xamarin.Android는 SDK를 어디에서 찾아야 하는지 알지 못합니다. Visual Studio에서 **도구 > 옵션 > Xamarin > Android 설정**으로 이동하여 Xamarin.Android에 SDK가 어디 있는지 알려 줄 수 있습니다.

[![Xamarin Android 설정의 Android SDK 위치](troubleshooting-images/01.png)](troubleshooting-images/01.png#lightbox)

## <a name="ide-does-not-display-target-device"></a>IDE에 대상 디바이스가 표시되지 않음

디바이스에 애플리케이션을 배포하려고 할 때 배포하려는 디바이스가 디바이스 선택 대화 상자에 표시되지 않는 경우가 있습니다. 이 문제는 Android Debug Bridge가 휴지기에 진입한 경우 발생할 수 있습니다.

이 문제를 진단하려면 [adb 프로그램](~/android/deploy-test/debugging/android-debug-log.md)을 찾아서 다음을 실행합니다.

```shell
adb devices
```

디바이스가 표시되지 않을 경우 디바이스를 찾을 수 있도록 Android Debug Bridge 서버를 다시 시작해야 합니다.

```shell
adb kill-server
adb start-server
```

HTC Sync 소프트웨어로 인해 **adb start-server**가 올바르게 작동하지 않을 수 있습니다. **adb start-server** 명령을 실행해도 서버가 시작되는 포트가 출력되지 않을 경우 HTC Sync 소프트웨어를 종료하고 adb 서버를 다시 시작해 보시기 바랍니다.

## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>지정된 작업 실행 파일 “keytool”을 실행할 수 없음

이는 PATH에 Java SDK의 bin 디렉터리가 있는 디렉터리가 없음을 의미합니다. [설치](~/android/get-started/installation/index.md) 가이드의 단계를 따랐는지 확인하세요.

## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>monodroid.exe 또는 aresgen.exe가 코드 1과 함께 종료됨

이 문제를 디버그하는 데 도움이 될 수 있도록 Visual Studio에서 MSBuild의 자세한 정도 수준을 변경합니다. 이렇게 하려면 **도구 > 옵션 > 프로젝트**, **솔루션 > 빌드** 및 **실행 > MSBuild 프로젝트 빌드 출력의 자세한 정도**를 선택하고 이 값을 **기본**으로 설정합니다.

다시 빌드한 후 Visual Studio의 출력 창을 확인하면 전체 오류를 볼 수 있습니다.

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>디바이스에 패키지를 배포할 만큼 충분한 스토리지 공간이 없음

이 문제는 에뮬레이터를 Visual Studio 내에서 시작하지 않은 경우에 발생합니다. 에뮬레이터를 Visual Studio 외부에서 시작한 경우 다음과 같이 `-partition-size 512` 옵션을 전달해야 합니다.

```shell
emulator -partition-size 512 -avd MonoDroid
```

올바른 시뮬레이터 이름, 즉 [시뮬레이터를 구성할 때 사용한 이름](~/android/get-started/installation/windows.md#device)을 사용했는지 확인하세요.

## <a name="install_failed_invalid_apk-when-installing-a-package"></a>패키지를 설치할 때 INSTALL\_FAILED\_INVALID\_APK가 표시됨

Android 패키지 이름은 ‘반드시’ 마침표(‘ *.* ’)를 포함해야 합니다. 마침표를 포함하도록 패키지 이름을 편집합니다.

- Visual Studio에서:
  - 프로젝트를 마우스 오른쪽 단추로 클릭하고 [속성]을 선택합니다.
  - 왼쪽의 [Android 매니페스트] 탭을 클릭합니다.
  - 패키지 이름 필드를 업데이트합니다.
    - &ldquo;AndroidManifest.xml을 찾을 수 없습니다. 하나를 추가하려면 클릭하세요.&rdquo; 메시지가 표시되면 링크를 클릭하고 패키지 이름 필드를 업데이트합니다.
- Mac용 Visual Studio에서:
  - 프로젝트를 마우스 오른쪽 단추로 클릭하고 [옵션]을 선택합니다.
  - 빌드/Android 애플리케이션 섹션으로 이동합니다.
  - ‘.’를 포함하도록 패키지 이름 필드를 변경합니다.

## <a name="install_failed_missing_shared_library-when-installing-a-package"></a>패키지를 설치할 때 INSTALL\_FAILED\_MISSING\_SHARED\_LIBRARY가 표시됨

이 컨텍스트에서 “공유 라이브러리”는 네이티브 공유 라이브러리(*libfoo.so*) 파일이 아니라 Google Maps와 같이 대상 디바이스에 별도로 설치해야 하는 라이브러리입니다.

Android 패키지는 `<uses-library/>` 요소에 어느 공유 라이브러리가 필요한지 지정합니다. 대상 디바이스에 *required* 라이브러리가 없는 경우(예: `//uses-library/@android:required`가 *true*(기본값)인 경우) *INSTALL\_FAILED\_MISSING\_SHARED\_LIBRARY*와 함께 패키지 설치가 실패합니다.

어느 공유 라이브러리가 필요한지 확인하려면 생성된 
**AndroidManifest.xml** 파일(예: **obj\\Debug\\android\\AndroidManifest.xml**)에서 `<uses-library/>` 요소를 찾습니다. `<uses-library/>` 요소는 프로젝트의 **Properties\\AndroidManifest.xml** 파일에 수동으로 추가하거나 [UsesLibraryAttribute 사용자 지정 특성](xref:Android.App.UsesLibraryAttribute)을 통해 수동으로 추가할 수 있습니다.

예를 들어, *Mono.Android.GoogleMaps.dll*에 어셈블리 참조를 추가하면 Google Maps 공유 라이브러리에 대해 `<uses-library/>`가 암시적으로 추가됩니다.

## <a name="install_failed_update_incompatible-when-installing-a-package"></a>패키지를 설치할 때 INSTALL\_FAILED\_UPDATE\_INCOMPATIBLE이 표시됨

Android 패키지에는 다음과 같은 세 가지 요구 사항이 있습니다.

- ‘.’를 포함해야 합니다(이전 항목 참조).
- 고유한 문자열 패키지 이름을 가져야 합니다.(이에 따라 Android 앱에는 Chrome 앱의 com.android.chrome과 같이 reverse-tld 규칙이 적용된 것을 볼 수 있습니다.)
- 패키지를 업그레이드할 때 패키지가 동일한 서명 키를 가져야 합니다.

다음과 같은 시나리오를 가정해 보겠습니다.

1. 앱을 디버그 앱으로 빌드 및 배포합니다.
2. 릴리스 앱용으로 사용하기 위해 (또는 기본값으로 제공된 디버그 서명 키가 마음에 들지 않아서) 서명 키를 변경합니다.
3. 앱을 먼저 제거하지 않은 상태로 앱을 설치합니다(예: Visual Studio에서 디버그 > 디버깅하지 않고 시작).

이 경우 서명 키는 변경됐으나 패키지 이름은 변경되지 않았기 때문에 INSTALL\_FAILED\_UPDATE\_INCOMPATIBLE 오류와 함께 패키지 설치가 실패합니다. [Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md)에도 다음과 비슷한 메시지가 포함됩니다.

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

이 오류를 해결하려면 디바이스에서 애플리케이션을 완전히 제거한 후에 다시 설치하세요.

## <a name="install_failed_uid_changed-when-installing-a-package"></a>패키지를 설치할 때 INSTALL\_FAILED\_UID\_CHANGED가 표시됨

Android 패키지가 설치되면 패키지에 ‘UID’(사용자 ID)가 할당됩니다.
현재 알려지지 않은 이유로 인해, 이미 설치된 앱 위에 설치하면 `INSTALL_FAILED_UID_CHANGED`와 함께 설치가 실패하는 경우가 있습니다.

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

이 문제를 해결하려면 앱을 Android 대상의 GUI에서 설치하거나 다음과 같이 `adb`를 사용하여 Android 패키지를 ‘완전히 제거’하세요.

```shell
$ adb uninstall @PACKAGE_NAME@
```

`adb uninstall -k`는 **사용하면 안 됩니다**. 사용할 경우 애플리케이션 데이터가 보존되고, 그에 따라 대상 디바이스에서 충돌하는 UID가 보존됩니다.

## <a name="release-apps-fail-to-launch-on-device"></a>디바이스에서 릴리스 앱이 시작되지 않음

Android 디버그 로그 출력에 다음과 비슷한 메시지가 포함되어 있습니까?

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

그렇다면 두 가지 원인이 있을 수 있습니다.

1. .apk가 대상 디바이스가 지원하는 ABI를 제공하지 않습니다.
    예를 들어, .apk는 armeabi-v7a 이진 파일만 포함하는데 대상 디바이스는 armeabi만 지원할 수 있습니다.

2. [Android 버그](https://code.google.com/p/android/issues/detail?id=21670)입니다. 이 경우에는 앱을 제거하고 행운을 빈 다음 앱을 다시 설치하세요.

(1)의 경우 문제를 해결하려면 프로젝트 옵션/속성을 편집하여 [지원되는 ABI 목록에 필요한 ABI에 대한 지원을 추가](~/android/app-fundamentals/cpu-architectures.md)합니다. 어느 ABI를 추가해야 하는지 확인하려면 대상 디바이스에 대해 다음 adb 명령을 실행합니다.

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

출력에 기본(및 선택적인 보조) ABI가 포함됩니다.

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>OutPath 속성이 프로젝트 &ldquo;MyApp.csproj&rdquo;에 대해 설정되지 않음

일반적으로 이 문제는 현재 HP 컴퓨터를 사용 중이고 환경 변수 &ldquo;Platform&rdquo;이 MCD나 HPD 등으로 설정되었음을 의미합니다. 이는 일반적으로 &ldquo;모든 CPU&rdquo; 또는 &ldquo;x86&rdquo;으로 설정되는 MSBuild 플랫폼 속성과 충돌합니다. MSBuild가 제대로 작동하려면 머신에서 이 환경 변수를 제거해야 합니다.

- 제어판 > 시스템 > 고급 > 환경 변수

Visual Studio 또는 Mac용 Visual Studio를 다시 시작하고 다시 빌드해 봅니다. 이렇게 하면 예상대로 작동할 것입니다.

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>java.lang.ClassCastException: mono.android.runtime.JavaObject를 캐스팅할 수 없음

Xamarin.Android 4.x는 중첩된 제네릭 형식을 올바르게 마샬링하지 않습니다. [SimpleExpandableListAdapter](xref:Android.Widget.SimpleExpandableListAdapter)를 사용하는 다음과 같은 C\# 코드를 예로 들어 보겠습니다.

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

문제는 Xamarin.Android가 중첩된 제네릭 형식을 올바르지 않게 마샬링한다는 것입니다. `List<IDictionary<string, object>>`는 [java.lang.ArrrayList](xref:Java.Util.ArrayList)로 마샬링되었지만 `ArrayList`는 [java.util.Map](xref:Java.Util.IMap)을 구현하는 인스턴스가 아닌 (`Dictionary<string, object>` 인스턴스를 참조하는) `mono.android.runtime.JavaObject` 인스턴스를 포함하기 때문에 다음과 같은 예외가 발생합니다.

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

해결 방법은 &ldquo;내부&rdquo; 형식으로 `System.Collections.Generic` 형식이 아닌 제공된 [Java Collection 형식](~/android/internals/api-design.md)을 사용하는 것입니다. 이렇게 하면 인스턴스를 마샬링할 때 올바른 Java 형식이 생성됩니다. (다음 코드는 gref 수명을 줄이기 위해 여기서 필요한 것보다 더 복잡해졌습니다. gref 수명이 문제가 되지 않는다면 `s/List/JavaList/g` 및 `s/Dictionary/JavaDictionary/g`를 통해 원본 코드를 변경하는 것으로 간소화될 수 있습니다.)

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

[이 문제는 향후 릴리스에서 수정됩니다](https://bugzilla.xamarin.com/show_bug.cgi?id=5401).

## <a name="unexpected-nullreferenceexceptions"></a>예기치 않은 NullReferenceExceptions

[Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md)가 &ldquo;가능하지 않거나&rdquo; 앱이 종료되기 직전에 Android 런타임 코드에 대해 Mono에서 발생하는 NullReferenceExceptions를 언급하는 경우가 있습니다.

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

이 문제는 Android 런타임이 프로세스를 중단하는 경우에 발생할 수 있는데, 그 원인에는 대상의 GREF 제한에 도달한 경우, JNI에서 무언가가 &ldquo;잘못된&rdquo; 경우와 같이 몇 가지가 있을 수 있습니다.

이 경우에 해당하는지 보려면 Android 디버그 로그에서 프로세스에서 제공한 다음과 같은 메시지가 있는지 살펴봅니다.

```shell
E/dalvikvm(  123): VM aborting
```

## <a name="abort-due-to-global-reference-exhaustion"></a>전역 참조 소모로 인한 중단

Android 런타임의 JNI 레이어는 임의 시점에 유효한 제한적인 개수의 JNI 개체 참조만을 지원합니다. 이 제한이 초과되면 문제가 발생합니다.

GREF(‘전역 참조’) 제한은 에뮬레이터서는 참조 2000개이고 하드웨어에서는 참조 52000개입니다.

Android 디버그 로그에 다음과 같은 메시지가 표시되면 GREF를 너무 많이 만들었음을 알 수 있습니다.

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

GREF 제한에 도달하면 다음과 같은 메시지가 출력됩니다.

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

위 예제에서([버그 685215](https://bugzilla.novell.com/show_bug.cgi?id=685215)에서 사용된 예제임) 문제는 너무 많은 Android.Graphics.Point 인스턴스가 만들어졌다는 것입니다. 이 버그의 픽스 목록은 [주석 \#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2)를 참조하세요.

일반적으로 유용한 해결 방법은 너무 많은 인스턴스를 할당한 형식이 무엇인지 &ndash; 위 덤프에서는 Android.Graphics.Point &ndash; 찾은 다음 소스 코드의 어디에서 이 형식이 만들어졌는지 찾아서 (Java 개체의 수명이 단축되도록) 적절히 폐기하는 것입니다. 이 방법이 항상 적절한 것은 아니나(\#685215는 다중 스레드이므로 이 해결 방법으로 Dispose 호출이 방지됩니다) 가장 먼저 고려해야 하는 방법입니다.

[GREF 로깅](~/android/troubleshooting/index.md)을 사용하도록 설정하여 어느 GREF가 생성되었고 몇 개가 존재하는지 확인할 수 있습니다.

## <a name="abort-due-to-jni-type-mismatch"></a>JNI 형식 불일치로 인한 중단

JNI 코드를 직접 롤백하는 경우 형식이 올바르게 일치하지 않을 수 있습니다. `java.lang.Runnable`을 구현하지 않는 형식에서 `java.lang.Runnable.run`을 호출하는 경우를 예로 들 수 있습니다. 이 문제가 발생하면 Android 디버그 로그에 다음과 비슷한 메시지가 표시됩니다.

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>동적 코드 지원

### <a name="dynamic-code-does-not-compile"></a>동적 코드가 컴파일되지 않음

애플리케이션 또는 라이브러리에서 C\#을 사용하려면 프로젝트에 System.Core.dll, Microsoft.CSharp.dll 및 Mono.CSharp.dll을 추가해야 합니다.

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>릴리스 빌드에서 런타임에 동적 코드에 대해 MissingMethodException이 발생합니다.

- 애플리케이션 프로젝트에 System.Core.dll, Microsoft.CSharp.dll 또는 Mono.CSharp.dll에 대한 참조가 없는 것 같습니다. 해당 어셈블리가 참조되는지 확인하세요.

  - 동적 코드에는 항상 비용이 따른다는 사실에 유의하세요. 효율적인 코드가 필요한 경우 동적 코드를 사용하지 않는 것이 좋습니다.

- 첫 번째 미리 보기에서, 애플리케이션 코드에서 각 어셈블리의 형식을 명시적으로 사용한 경우가 아닌 이상 해당 어셈블리가 제외되었습니다. [http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)에서 해결 방법을 참조하세요.

## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>x86 디바이스에서 AOT+LLVM으로 빌드한 프로젝트가 크래시됨

x86 기반 디바이스에 [AOT+LLVM](~/android/deploy-test/release-prep/index.md)으로 빌드한 앱을 배포하면 다음과 비슷한 예외 오류 메시지가 표시될 수 있습니다.

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (Xamarin.bug56111)
```

이것은 알려진 문제입니다. 해결 방법은 LLVM을 사용하지 않도록 설정하는 것입니다.
