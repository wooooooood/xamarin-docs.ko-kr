---
title: GDB
ms.topic: article
ms.prod: xamarin
ms.assetid: CD0BE462-FA38-4881-B481-82AD05B3B8FE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 246dd135b8a6e8a60bca9ba38e91ca8fd2d43674
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2018
---
# <a name="gdb"></a>GDB

## <a name="overview"></a>개요

Xamarin.Android 4.10은 `_Gdb` MSBuild 대상을 사용하여 `gdb`를 사용하는 부분적인 지원을 도입했습니다. 

> [!NOTE]
> `gdb` 지원을 위해서는 Android NDK를 설치해야 합니다.

`gdb`를 사용하는 방법은 세 가지가 있습니다.

1.  [빠른 배포가 활성화된 디버그 빌드](#Debug_Builds_with_Fast_Deployment).
1.  [빠른 배포가 비활성화된 디버그 빌드](#Debug_Builds_without_Fast_Deployment).
1.  [릴리스 빌드](#Release_Builds).


문제가 발생할 경우 [문제 해결](#Troubleshooting) 섹션을 참조하세요.

<a name="Debug_Builds_with_Fast_Deployment" />

### <a name="debug-builds-with-fast-deployment"></a>빠른 배포를 사용하는 디버그 빌드

빠른 배포가 활성화된 디버그 빌드를 빌드하고 배포할 때는 `_Gdb` MSBuild 대상을 사용하여 `gdb`를 연결할 수 있습니다.

첫째, 앱을 설치합니다. 이는 IDE 또는 명령줄을 통해 수행할 수 있습니다.

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:Install *.csproj
```

둘째, `_Gdb` 대상을 실행합니다. 실행이 끝나면 `gdb` 명령줄이 인쇄됩니다.

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
...
```

`_Gdb` 대상이 `AndroidManifest.xml` 파일 내에 선언된 임의의 시작 관리자 작업을 시작합니다. 실행할 작업을 명시적으로 지정하려면 `RunActivity` MSBuild 속성을 사용합니다. 이때 서비스 및 다른 Android 구문은 지원되지 않습니다.

`_Gdb` 대상은 `gdb-symbols` 디렉터리를 만들고 대상의 `/system/lib` 및 `$APPDIR/lib` 디렉터리의 콘텐츠를 복사합니다.


> [!NOTE]
> `gdb-symbols` 디렉터리의 콘텐츠는 사용자가 배포한 Android 대상에 연결되고, 사용자가 대상을 변경하지 않는 한 자동으로 바뀌지 않습니다. (이는 버그로 간주하세요.) Android 대상 장치를 변경할 경우 이 디렉터리를 수동으로 삭제해야 합니다.

마지막으로, 생성된 `gdb` 명령을 복사하여 셸에서 실행합니다.

```bash
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) bt
#0  0x40082e84 in nanosleep () from /Users/jon/Development/Projects/Scratch.HelloXamarin20/gdb-symbols/libc.so
#1  0x4008ffe6 in sleep () from /Users/jon/Development/Projects/Scratch.HelloXamarin20/gdb-symbols/libc.so
#2  0x74e46240 in ?? ()
#3  0x74e46240 in ?? ()
(gdb) c
```

<a name="Debug_Builds_without_Fast_Deployment" />

## <a name="debug-builds-without-fast-deployment"></a>빠른 배포를 사용하지 않는 디버그 빌드

빠른 배포를 *사용하는* 디버그 빌드는 Android NDK의 `gdbserver` 프로그램을 빠른 배포 `.__override__` 디렉터리에 복사하는 방식으로 작동합니다. 빠른 배포가 비활성화되어 있으면 이 디렉터리가 존재하지 않을 수 있습니다.

두 가지 해결 방법이 있습니다.

-   `.__override__` 디렉터리가 생성되도록 `debug.mono.log` 시스템 속성을 설정합니다.
-   `.apk` 내에 `gdbserver`를 포함합니다.

### <a name="setting-the-debugmonolog-system-property"></a>`debug.mono.log` 시스템 속성 설정

`debug.mono.log` 시스템 속성을 설정하려면 `adb` 명령을 사용합니다.

```bash
$ adb shell setprop debug.mono.log gc
```

시스템 속성이 설정되면 빠른 배포를 사용하는 디버그 빌드 구성과 마찬가지로 `_Gdb` 대상과 인쇄된 `gdb` 명령을 실행합니다.

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
  ...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
  ...
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) c
```


### <a name="including-gdbserver-in-your-app"></a>앱에 `gdbserver` 포함

앱 내에 `gdbserver`를 포함하려면

1. Android NDK 내에서 `gdbserver`를 찾고(**$ANDROID\_NDK\_PATH/prebuilt/android-arm/gdbserver/gdbserver**에 있음) 프로젝트 디렉터리에 복사합니다.

2. `gdbserver`의 이름을 **libs/armeabi-v7a/libgdbserver.so**로 바꿉니다.

3. `AndroidNativeLibrary`의 **빌드 동작**을 사용하여 프로젝트에 **libs/armeabi-v7a/libgdbserver.so**를 추가합니다.

4. 응용 프로그램을 다시 빌드하고 다시 설치합니다.

앱이 다시 설치되면 빠른 배포를 사용하는 디버그 빌드 구성과 마찬가지로 `_Gdb` 대상과 인쇄된 `gdb` 명령을 실행합니다.

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
  ...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
  ...
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) c
```

<a name="Release_Builds" />

## <a name="release-builds"></a>릴리스 빌드

`gdb` 지원에는 다음 세 가지가 필요합니다.

1.  `INTERNET` 권한.
2.  앱 디버깅 활성화.
3.  액세스 가능한 `gdbserver`.

디버그 앱에서는 기본적으로 `INTERNET` 권한이 활성화됩니다. 응용 프로그램에 아직 없을 경우 **속성/AndroidManifest.xml**을 편집하거나 [프로젝트 속성](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest/)을 편집하여 추가할 수 있습니다.

앱 디버깅은 [ApplicationAttribute.Debugging](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Debuggable/) 사용자 지정 특성 속성을 `true`로 설정하거나 **속성/AndroidManifest.xml**을 편집하고 `//application/@android:debuggable` 특성을 `true`로 설정하여 활성화할 수 있습니다.

```xml
<application android:label="Example.Name.Here" android:debuggable="true">
```

액세스 가능한 `gdbserver`는 [빠른 배포를 사용하지 않는 디버그 빌드](#Debug_Builds_without_Fast_Deployment) 섹션을 따라 제공할 수 있습니다.

한 가지 정보: `_Gdb` MSBuild 대상은 이전에 실행 중이던 모든 앱 인스턴스를 종료합니다. Android v4.0 이전 대상에서는 이 기능이 작동하지 않습니다.

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>문제 해결

### <a name="monopmip-doesnt-work"></a>`mono_pmip`가 작동하지 않음

`mono_pmip` 함수([관리되는 스택 프레임 가져오기](http://www.mono-project.com/docs/debug+profile/debug/#debugging-with-gdb)에 유용)가 `libmonosgen-2.0.so`에서 내보내졌고, `_Gdb` 대상을 현재 끌어내릴 수 없습니다. (이 문제는 향후 릴리스에서 수정됩니다.)

`libmonosgen-2.0.so`에 있는 함수 호출을 활성화하려면 대상 장치에서 `gdb-symbols` 디렉터리로 복사합니다.

```bash
$ adb pull /data/data/Mono.Android.DebugRuntime/lib/libmonosgen-2.0.so Project/gdb-symbols
```

그런 다음, 디버깅 세션을 다시 시작합니다.

### <a name="bus-error-10-when-running-the-gdb-command"></a>`gdb` 명령을 실행할 경우 Bus error: 10

`"Bus error: 10"`과 함께 `gdb` 명령 오류가 출력될 경우 Android 장치를 다시 시작합니다.

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
Bus error: 10
$
```

### <a name="no-stack-trace-after-attach"></a>연결 후 스택 추적 없음

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
(gdb) bt
No stack.
```

이는 일반적으로 `gdb-symbols` 디렉터리의 콘텐츠가 Android 대상과 동기화되지 않는다는 신호입니다. (Android 대상을 변경하셨습니까?)

`gdb-symbols` 디렉터리를 삭제하고 다시 시도하세요.
