---
title: Android 디버그 로그
description: 디버그 로그를 사용하여 Xamarin.Android 응용 프로그램을 디버깅하는 방법입니다.
ms.prod: xamarin
ms.assetid: 01A715FE-9E9D-9B85-8A59-6568D8A09CA5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: 442606f456e6f42ee178cd93253883a1d9de52c4
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935219"
---
# <a name="android-debug-log"></a>Android 디버그 로그

개발자가 응용 프로그램 디버그에 사용하는 아주 일반적인 트릭은 `Console.WriteLine`에 대한 호출을 수행하는 것입니다. 하지만 Android와 같은 모바일 플랫폼에는 콘솔이 없습니다. Android 장치에는 앱을 작성하는 동안 사용할 수 있는 로그가 제공됩니다. 이 명령은 검색을 위해 입력하는 명령 때문에 _logcat_이라고도 합니다. 로그된 데이터를 보려면 **디버그 로그** 도구를 사용하세요.

## <a name="android-debug-log-overview"></a>Android 디버그 로그 개요

**디버그 로그** 도구는 Visual Studio를 통해 앱을 디버그하는 동안 로그 출력을 보는 방법을 제공합니다. 디버그 로그는 다음 장치를 지원합니다.

-   물리적인 Android 휴대폰, 태블릿 및 착용식 장치
-   Android Emulator에서 실행 중인 Android 가상 장치 

> [!NOTE]
> **디버그 로그** 도구는 Xamarin Live Player에서 작동하지 않습니다.

**디버그 로그**는 앱이 장치에서 독립형으로 실행 중인 동안(즉 Visual Studio에서 연결이 끊어진 동안)에는 로그 메시지를 표시하지 않습니다.


## <a name="accessing-the-debug-log-from-visual-studio"></a>Visual Studio에서 디버그 로그에 액세스

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**장치 로그** 도구를 열려면 도구 모음에서 **장치 로그(logcat)** 아이콘을 클릭합니다.

[![도구 모음에서 장치 로그 도구의 위치](android-debug-log-images/vswin-01-logcat-sml.png)](android-debug-log-images/vswin-01-logcat.png#lightbox)

또는 다음 메뉴 선택 사항 중 하나에서 **장치 로그** 도구를 시작합니다.

-   **보기 > 다른 창 > 장치 로그**
-   **도구 > Android > 장치 로그**

다음 스크린샷은 **디버그 도구** 창의 다양한 부분을 보여줍니다.

[![디버그 도구 창의 부분](android-debug-log-images/vswin-03-features-sml.png)](android-debug-log-images/vswin-03-features.png#lightbox)

-   **Device Selector**(장치 선택기) &ndash; 모니터링할 물리적 장치 또는 실행 중인 에뮬레이터를 선택합니다.

-   **Log Entries**(로그 항목) &ndash; logcat의 로그 메시지 테이블입니다.

-   **Clear Log Entries**(로그 항목 지우기) &ndash; 테이블에서 현재 로그를 모두 지웁니다.

-   **Play/Pause**(재생/일시 중지) &ndash; 새 로그 항목 표시 업데이트 또는 일시 중지 간을 토글합니다.

-   **중지** &ndash; 새 로그 항목 표시를 중지합니다.

-   **Search Box**(검색 상자) &ndash; 이 상자에 검색 문자열을 입력하여 로그 항목의 하위 집합을 필터링합니다.


**디버그 로그** 도구 창이 표시되면 장치 풀다운 메뉴를 사용하여 모니터링할 Android 장치를 선택합니다.

[![Device Selector(장치 선택기)의 위치](android-debug-log-images/vswin-02-devices-combo-sml.png)](android-debug-log-images/vswin-02-devices-combo.png#lightbox)

장치가 선택되면 **장치 로그** 도구가 자동으로 실행 중인 앱의 로그 항목을 추가합니다. 이러한 로그 항목은 로그 항목 테이블에 표시됩니다.&ndash; 장치 간에 전환하면 장치 로깅이 중지되었다가 다시 시작됩니다. 장치 선택기에 장치가 표시되기 전에 Android 프로젝트가 로드되어야 합니다. 장치 선택기에 장치가 표시되지 않는 경우 **시작** 단추 옆의 Visual Studio 장치 드롭다운 메뉴에서 장치가 사용 가능한지 확인합니다.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**장치 로그**를 열려면 **보기 > 패드 > 장치 로그**를 클릭합니다.

[![장치 로그 메뉴 항목의 위치](android-debug-log-images/vsmac-01-logcat-sml.png)](android-debug-log-images/vsmac-01-logcat.png#lightbox)

다음 스크린샷은 **디버그 도구** 창의 다양한 부분을 보여줍니다.

[![디버그 도구 창의 기능](android-debug-log-images/vsmac-03-features-sml.png)](android-debug-log-images/vsmac-03-features.png#lightbox)

-   **Device Selector**(장치 선택기) &ndash; 모니터링할 물리적 장치 또는 실행 중인 에뮬레이터를 선택합니다.

-   **Log Entries**(로그 항목) &ndash; logcat의 로그 메시지 테이블입니다.

-   **Clear Log Entries**(로그 항목 지우기) &ndash; 테이블에서 현재 로그를 모두 지웁니다.

-   **Search Box**(검색 상자) &ndash; 이 상자에 검색 문자열을 입력하여 로그 항목의 하위 집합을 필터링합니다.

-   **메시지 표시** &ndash; 정보 메시지 표시를 토글합니다.

-   **경고 표시** &ndash; 경고 메시지 표시를 토글합니다(경고 메시지는 노란색으로 표시됨).

-   **오류 표시** &ndash; 오류 메시지 표시를 토글합니다(오류 메시지는 빨란색으로 표시됨).

-   **다시 연결** &ndash; 장치에 다시 연결하고 로그 항목 표시를 새로 고칩니다.

-   **마커 추가** &ndash; 마지막 로그 항목 뒤에 마커 메시지(예: `--- Marker N ---`)를 삽입합니다. 여기서 _N_은 1에서 시작하여 새 마커가 추가되면 1씩 증가하는 카운터입니다.

[디버그 로그] 도구 창이 표시되면 장치 풀다운 메뉴를 사용하여 모니터링할 Android 장치를 선택합니다.

[![Device selector(장치 선택기)의 위치](android-debug-log-images/vsmac-02-devices-combo-sml.png)](android-debug-log-images/vsmac-02-devices-combo.png#lightbox)

장치가 선택되면 **장치 로그** 도구가 자동으로 실행 중인 앱의 로그 항목을 추가합니다. 이러한 로그 항목은 로그 항목 테이블에 표시됩니다.&ndash; 장치 간에 전환하면 장치 로깅이 중지되었다가 다시 시작됩니다. 장치 선택기에 장치가 표시되기 전에 Android 프로젝트가 로드되어야 합니다. 장치 선택기에 장치가 표시되지 않는 경우 **시작** 단추 옆의 Visual Studio 장치 드롭다운 메뉴에서 장치가 사용 가능한지 확인합니다.

-----


## <a name="accessing-from-the-command-line"></a>명령줄에서 액세스

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

디버그 로그를 보는 또 다른 옵션은 명령줄을 통해서 보는 것입니다. 명령 프롬프트 창을 열고 Android SDK 플랫폼 도구 폴더(일반적으로 SDK 플랫폼 도구 폴더는 **C:\\Program Files (x86)\\Android\\android-sdk\\platform-tools**에 있음)로 이동합니다.

장치(물리적 장치 또는 에뮬레이터)가 하나만 연결된 경우 다음 명령을 입력하여 로그를 볼 수 있습니다.

```shell
$ adb logcat
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

디버그 로그를 보는 또 다른 옵션은 명령줄을 통해서 보는 것입니다. 터미널 창을 열고 Android SDK 플랫폼 도구 폴더(일반적으로 SDK 플랫폼 도구 폴더는 **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/platform-tools**에 있음)로 이동합니다.

장치(물리적 장치 또는 에뮬레이터)가 하나만 연결된 경우 다음 명령을 입력하여 로그를 볼 수 있습니다.

```shell
$ ./adb logcat
```

-----


연결된 장치가 둘 이상이면 장치를 분명히 식별해야 합니다. 예를 들어 **adb -d logcat**은 연결된 물리적 장치의 로그만 보여주고, **adb -e logcat**은 실행 중인 에뮬레이터의 로그만 보여줍니다.

**adb**를 입력하고 도움말 메시지를 읽어서 더 많은 명령을 찾을 수 있습니다.


## <a name="writing-to-the-debug-log"></a>디버그 로그에 쓰기

[Android.Util.Log](https://developer.xamarin.com/api/type/Android.Util.Log/) 클래스의 메서드를 사용하여 **디버그 로그**에 메시지를 쓸 수 있습니다.
예: 

```csharp
string tag = "myapp";

Log.Info (tag, "this is an info message");
Log.Warn (tag, "this is a warning message");
Log.Error (tag, "this is an error message");
```

이 명령은 다음과 비슷한 출력을 생성합니다.

```shell
I/myapp   (11103): this is an info message
W/myapp   (11103): this is a warning message
E/myapp   (11103): this is an error message
```

`Console.WriteLine`을 사용하여 **디버그 로그** &ndash;에 쓸 수도 있습니다. 이 메시지는 출력 형식과는 약간 다르게 logcat에 나타납니다(이 기법은 Android에서 Xamarin.Forms 앱을 디버깅할 때 특히 유용합니다).

```csharp
System.Console.WriteLine ("DEBUG - Button Clicked!");
```

이 명령은 logcat에 다음과 비슷한 출력을 생성합니다.

```
Info (19543) / mono-stdout: DEBUG - Button Clicked!
```

## <a name="interesting-messages"></a>흥미로운 메시지

로그를 읽을 때(그리고 특히 다른 사람에게 로그 코드 조각을 제공할 때) 로그 파일을 전부 정독하는 일은 너무 힘든 경우가 많습니다.
로그 메시지를 보다 쉽게 탐색하려면 다음과 유사한 로그 항목을 찾습니다.

```shell
I/ActivityManager(12944): Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=GcTest.GcTest/gctest.Activity1 } from pid 24175
```

특히 응용 프로그램 패키지 이름도 포함하는 정규식과 일치하는 줄을 찾습니다.

```shell
^I.*ActivityManager.*Starting: Intent
```

작업의 시작 부분에 해당하는 줄이며 다음 메시지 중 *대부분*(전부는 아님)이 응용 프로그램과 관련되어야 합니다.

모든 메시지는 메시지를 생성하는 프로세스의 프로세스 식별자(pid)를 포함합니다. 위의 `ActivityManager` 메시지에서 `12944` 프로세스는 메시지를 생성합니다. 어떤 프로세스가 디버그 중인 응용 프로그램의 프로세스인지 확인하려면 **mono.MonoRuntimeProvider** 메시지를 찾습니다. 

```shell
I/ActivityThread(  602): Pub TouchTest.TouchTest.__mono_init__: mono.MonoRuntimeProvider
```

이 메시지는 시작된 프로세스에서 생성됩니다. 이 pid를 포함하는 모든 후속 메시지는 동일한 프로세스에서 생성됩니다.
