---
title: "Android 디버그 로그"
ms.topic: article
ms.prod: xamarin
ms.assetid: 01A715FE-9E9D-9B85-8A59-6568D8A09CA5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 26ac42e4b7acbe19dee746130fc335fdf18ffc46
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="android-debug-log"></a>Android 디버그 로그

## <a name="android-debug-log-overview"></a>Android 디버그 로그 개요

개발자가 응용 프로그램 디버그에 사용하는 아주 일반적인 트릭은 `Console.WriteLine`에 대한 호출을 수행하는 것입니다. 하지만 Android와 같은 모바일 플랫폼에는 콘솔이 없습니다. Android 장치에는 앱을 작성하는 동안 사용할 수 있는 로그가 제공됩니다. 이 명령은 검색을 위해 입력하는 명령 때문에 'logcat'이라고도 합니다.

## <a name="accessing-from-visual-studio"></a>Visual Studio에서 액세스

Visual Studio 2015 이상에서는 Android 및 iOS 로그 패드가 통합되어 있습니다.

### <a name="visual-studio-2015--2017"></a>Visual Studio 2015 & 2017

Visual Studio용 새로운 장치 로그 도구 창에는 Android 및 iOS 장치에 대한 로그가 표시됩니다. 다음 명령 중 하나를 실행하여 표시할 수 있습니다. 

-   **보기 > 다른 창 > 장치 로그**
-   **도구 > Android > 장치 로그**
-   **Android 도구 모음 > 장치 로그**

도구 창이 표시되면 장치 콤보 상자에서 물리적 장치를 선택할 수 있습니다. 장치를 선택하면 테이블의 실행 중인 앱의 로그 항목이 자동으로 추가되기 시작합니다. 장치를 전환하면 장치 로깅이 중지되고 시작됩니다. 장치를 콤보 상자에 나타내려면 Android 프로젝트를 로드해야 합니다. 장치가 콤보 상자에 나타나지 않으면, 먼저 Start Debug(디버그 시작) 드롭다운에서 해당 장치를 사용할 수 있는지 확인합니다. 

이 도구 창에는 로그 항목 테이블, 장치를 선택할 수 있는 콤보 상자, 로그 항목을 지우는 방법, 검색 상자 및 재생/중지/일시 중지 단추가 제공됩니다. 



## <a name="accessing-from-the-command-line"></a>명령줄에서 액세스

디버그 로그를 보는 또 다른 옵션은 명령줄을 통해서 보는 것입니다. 콘솔 창을 열고 Android SDK 플랫폼 도구 폴더(예: **C:\android-sdk-windows\platform-tools**)로 이동합니다. 

연결된 장치가 하나뿐이면 다음을 사용하여 로그를 볼 수 있습니다.

```shell
$ adb logcat
```

연결된 장치가 둘 이상이면 장치를 식별해야 합니다. 예를 들어 `adb -d logcat`은 유일하게 연결된 물리적 장치의 로그를 보여주지만 `adb -e logcat`은 유일하게 실행 중인 에뮬레이터의 로그를 보여줍니다. 

**adb**를 실행하면 더 많은 명령을 볼 수 있습니다.



## <a name="writing-to-the-debug-log"></a>디버그 로그에 쓰기

[Android.Util.Log](https://developer.xamarin.com/api/type/Android.Util.Log/) 클래스의 메서드를 사용하여 디버그 로그에 메시지를 쓸 수 있습니다. 

```csharp
string tag = "myapp";

Log.Info (tag, "this is an info message");
Log.Warn (tag, "this is a warning message");
Log.Error (tag, "this is an error message");
```

위의 내용은 아래 메시지를 생성합니다.

```shell
I/myapp   (11103): this is an info message
W/myapp   (11103): this is a warning message
E/myapp   (11103): this is an error message
```


## <a name="interesting-messages"></a>흥미로운 메시지

로그를 읽을 때, 특히 다른 사람에게 로그 조각을 제공할 때(전체 로그 파일을 제공하면 (1)로그가 지나치게 많고 (2)읽을 수 없기 때문에) 다음과 유사한 줄로 시작하는 것이 *가장 중요*합니다.

```shell
I/ActivityManager(12944): Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=GcTest.GcTest/gctest.Activity1 } from pid 24175
```

특히 정규식과 일치하는 줄:

```shell
^I.*ActivityManager.*Starting: Intent
```

및 응용 프로그램 패키지의 이름을 포함해야 합니다. 작업의 시작 부분에 해당하는 줄이며 다음 메시지 중 *대부분*(전부는 아님)이 응용 프로그램과 관련되어야 합니다. 

특히, 모든 메시지는 메시지를 생성하는 프로세스의 프로세스 식별자(pid)를 포함합니다. 위의 `ActivityManager` 메시지에서 `12944` 프로세스는 메시지를 생성합니다. 어떤 프로세스가 디버깅 중인 응용 프로그램의 프로세스인지 확인하려면 mono.MonoRuntimeProvider 메시지를 찾습니다. 

```shell
I/ActivityThread(  602): Pub TouchTest.TouchTest.__mono_init__: mono.MonoRuntimeProvider
```

이 메시지는 시작된 프로세스에서 발생하였으며 동일한 프로세스에서 발생한 동일한 pid를 포함하는 모든 후속 메시지가 제공됩니다. 
