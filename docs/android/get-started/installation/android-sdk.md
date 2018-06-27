---
title: Xamarin.Android에 대한 Android SDK 설정
description: Visual Studio에는 Google의 독립 실행형 SDK Manager를 대체하는 Android SDK Manager가 포함되어 있습니다. 이 가이드에서는 SDK Manager를 사용하여 Android SDK 도구, 플랫폼 및 Xamarin.Android 앱 개발에 필요한 기타 구성 요소를 다운로드하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 9A857F52-2EC1-414F-8010-CEE67B60A4B4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/02/2018
ms.openlocfilehash: 0af0ef56630103498041ad61f7c5ce900358b055
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732855"
---
# <a name="setting-up-the-android-sdk-for-xamarinandroid"></a>Xamarin.Android에 대한 Android SDK 설정

_Visual Studio에는 Google의 독립 실행형 SDK Manager를 대체하는 Android SDK Manager가 포함되어 있습니다. 이 가이드에서는 SDK Manager를 사용하여 Android SDK 도구, 플랫폼 및 Xamarin.Android 앱 개발에 필요한 기타 구성 요소를 다운로드하는 방법을 설명합니다._


## <a name="overview"></a>개요

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

이 가이드에서는 Windows(또는 [Mac](?tabs=vsmac))용 Visual Studio를 위해 Xamarin Android SDK Manager를 설치하고 사용하는 방법을 설명합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

이 가이드에서는 Mac(또는 [Windows](?tabs=vswin))용 Visual Studio를 위해 Xamarin Android SDK Manager를 설치하고 사용하는 방법을 설명합니다.

> [!NOTE]
> 이 가이드는 Visual Studio 2017 및 Mac용 Visual Studio에만 적용됩니다.  

-----

Xamarin Android SDK Manager를 사용하여 Xamarin.Android 앱 개발에 필요한 최신 Android 구성 요소를 다운로드할 수 있습니다.
이는 더 이상 사용되지 않는 Google의 독립 실행형 SDK Manager를 대체합니다.

Android SDK에 포함되어 있는 SDK Manager 대신 Xamarin Android SDK Manager를 사용해야 하는 이유는 무엇일까요? Android SDK Tools 패키지 버전 25.2.3에서 Google은 Android SDK를 유지하기 위해 새로운 도구를 도입했습니다. **[sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)** 라는 이 새 도구는 Android SDK의 독립 실행형 UI 관리자를 대체하는 명령줄 유틸리티입니다. 따라서 SDK Tools 버전 26.0.1(Android 8.0에 필요) 이상으로 업데이트하고 UI 인터페이스를 통해 Android SDK를 계속 관리하려는 경우 Xamarin Android SDK Manager를 사용해야 합니다.

## <a name="requirements"></a>요구 사항

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin Android SDK Manager를 사용하려면 다음이 필요합니다.

- Visual Studio 2017(Community, Professional 또는 Enterprise 버전). Visual Studio 2017 버전 15.5 이상이 필요합니다.

- Visual Studio Tools for Xamarin 버전 4.5.0 이상 

Xamarin Android SDK Manager는 Visual Studio와 호환되지 않습니다.
2015. Visual Studio 2015 사용자는 Google에서 Android SDK에 제공하는 SDK Manager 도구를 사용해야 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   Mac용 Visual Studio 7.0.0.3146(이상).

-----

Xamarin Android SDK Manager에는 (Xamarin.Android와 함께 자동으로 설치되는) Java Development Kit도 필요합니다.
Xamarin.Android는 API 수준 24 이상을 대상으로 개발하는 경우에 필요한 [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)을 사용합니다(JDK 8은 API 24 미만도 지원함). API 수준 23 이하를 대상으로 개발하는 경우 [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)을 계속 사용할 수 있습니다.

> [!IMPORTANT]
> Xamarin.Android는 JDK 9를 지원하지 않습니다.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="installation"></a>설치

설치 중에 Visual Studio 2017에 Xamarin SDK Manager를 추가할 수 있습니다. Visual Studio를 설치할 때 **개별 구성 요소**를 클릭하고 **개발 활동** 섹션까지 아래로 스크롤하세요.
아직 선택하지 않은 경우 **Xamarin SDK Manager**를 사용하도록 설정합니다.

![개별 구성 요소에서 Xamarin SDK Manager를 사용하도록 설정](android-sdk-images/win/01-sdk-manager-install.png)

Visual Studio 2017을 이미 설치한 경우 [Visual Studio 2017 수정](https://docs.microsoft.com/en-us/visualstudio/install/modify-visual-studio)에서 Visual Studio를 수정하는 방법에 대한 지침을 참조한 후 위의 절차를 따라 Xamarin SDK Manager를 사용하도록 설정하세요. SDK Manager를 업데이트하라는 메시지가 표시되면 이와 동일한 절차를 따라 Xamarin SDK Manager를 설치할 수 있습니다.

(다음에 설명된 대로) **도구 > Android > Android SDK Manager**를 클릭하면 Google Android SDK Manager 대신 Xamarin Android SDK Manager가 실행됩니다. Google의 독립 실행형 Android SDK Manager를 지원하는 이전 버전의 Android SDK를 사용 중인 경우 Xamarin Android SDK Manager를 설치하면 충돌이 발생하지 않습니다. Visual Studio 외부에서 독립 실행형 Google SDK Manager를 실행하여 Android SDK를 계속 관리할 수 있습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
-----

 
## <a name="sdk-manager"></a>SDK Manager 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio의 SDK Manager를 시작하려면 **도구 -> Android-> Android SDK Manager**를 클릭합니다.

[![Android SDK Manager 메뉴 항목의 위치](android-sdk-images/win/02-sdk-manager-menu-item-sml.png)](android-sdk-images/win/02-sdk-manager-menu-item.png#lightbox)

**Xamarin Android SDK Manager**는 **Android SDK 및 도구**에서 열 수 있습니다. 이 화면에는 **플랫폼** 및 **도구**라는 두 개의 탭이 있습니다.

[![플랫폼 탭에서 열린 Android SDK Manager의 스크린샷](android-sdk-images/win/03-sdk-manager-platforms-sml.png)](android-sdk-images/win/03-sdk-manager-platforms.png#lightbox)

**Android SDK 및 도구** 화면에 대해서는 다음 섹션에서 자세히 설명합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac용 Visual Studio의 SDK Manager를 시작하려면 **도구 -> SDK Manager**를 클릭합니다.
 
![Android SDK Manager 메뉴 항목의 위치](android-sdk-images/mac/sdkmanager-01.png )

**Android SDK Manager**는 **플랫폼**, **도구** 및 **위치**라는 세 개의 탭이 있는 **기본 설정 창**에서 열 수 있습니다.

![플랫폼 탭에서 열린 Android SDK Manager의 스크린샷](android-sdk-images/mac/sdkmanager-02.png)

Xamarin Android SDK Manager의 탭에 대해서는 다음 섹션에서 자세히 설명합니다.

-----



# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="android-sdk-location"></a>Android SDK 위치

Android SDK 위치는 이전 이미지에서 볼 수 있는 것처럼 **Android SDK 및 도구** 화면의 상단에서 구성합니다. 이 위치를 올바르게 구성해야 **플랫폼** 및 **도구** 탭이 올바르게 작동합니다. 다음 이유 중 하나 이상으로 인해 Android SDK의 위치를 설정해야 할 수 있습니다.

1. Xamarin SDK Manager에서 Android SDK를 찾을 수 없는 경우. 

2. Android SDK를 대체(기본값이 아닌) 위치에 설치한 경우. 

Android SDK의 위치를 설정하려면 **Android SDK 위치**의 맨 오른쪽에 있는 &hellip; 단추를 클릭합니다. 그러면 Android SDK의 위치로 이동하는 데 사용할 수 있는 **폴더 찾아보기** 대화 상자가 열립니다. 다음 스크린샷에서는 **Program Files (x86)\\Android**에서 Android SDK를 선택합니다.

![Android SDK를 찾는 Windows 폴더 찾아보기 대화 상자의 스크린샷](android-sdk-images/win/05-browse-for-folder.png)

**확인**을 클릭하면 Xamarin Android SDK Manager가 선택된 위치에 설치된 Android SDK를 관리합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="locations-tab"></a>위치 탭

**위치** 탭에는 Android SDK, Android NDK 및 Java SDK(JDK)의 위치를 구성하기 위한 세 가지 설정이 있습니다. 이 위치를 올바르게 구성해야 **플랫폼** 및 **도구** 탭이 올바르게 작동합니다.

SDK Manager를 시작하면 설치된 각 패키지의 경로가 자동으로 확인되고, 경로 옆에 녹색 확인 표시 아이콘에 표시되며 **있음**을 알려줍니다.

![위치 탭의 스크린 샷](android-sdk-images/mac/sdkmanager-03.png)

SDK Manager가 기본 위치에 있는 SDK, NDK 및 JDK를 찾도록 하려면 **기본값으로 다시 설정** 단추를 클릭합니다. 

일반적으로 **위치** 탭을 사용하여 Android SDK 및/또는 Java JDK의 위치를 수정할 수 있습니다. Xamarin.Android 앱 개발을 위해 NDK를 설치할 필요는 없습니다. NDK는 앱의 일부분을 C 및 C++와 같은 네이티브 코드 언어를 사용하여 개발해야 하는 경우에만 사용됩니다.

-----


### <a name="tools-tab"></a>도구 탭

**도구** 탭에는 _도구_ 및 _추가 기능_ 목록이 표시됩니다. 이 탭을 사용하여 Android SDK 도구, 플랫폼 도구 및 빌드 도구를 설치할 수 있습니다.
또한 Android Emulator, 하위 수준 디버거(LLDB), NDK, HAXM 가속화 및 Google Play 라이브러리를 설치할 수 있습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

예를 들어 Google Android Emulator 패키지를 다운로드하려면 **Android Emulator** 옆에 있는 확인 표시를 클릭하고 **변경 내용 적용** 단추를 클릭합니다.

[![도구 탭에서 Android Emulator 설치](android-sdk-images/win/06-install-emulator-sml.png)](android-sdk-images/win/06-install-emulator.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

예를 들어 Google Android Emulator 패키지를 다운로드하려면 **Android Emulator** 옆에 있는 확인 표시를 클릭하고 **업데이트 설치** 단추를 클릭합니다.

![도구 탭에서 Android Emulator 설치](android-sdk-images/mac/sdkmanager-08.png)

-----


_일부 구성 요소를 업데이트할 수 있습니다. 지금 업데이트하시겠습니까?_ 라는 메시지가 포함된 대화 상자가 표시될 수 있습니다. **예**를 클릭합니다. 그러면 라이선스 승인 대화 상자가 표시됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![라이선스 승인 화면](android-sdk-images/win/07-license-acceptance.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![라이선스 승인 화면](android-sdk-images/mac/sdkmanager-09.png)

-----

사용 약관에 동의하려면 **동의**를 클릭합니다. 창 아래쪽에 다운로드 및 설치 진행률을 나타내는 진행률 표시줄이 표시됩니다. 설치가 완료되면 **도구** 탭에 선택한 도구 및 추가 기능이 설치된 것으로 표시됩니다.



### <a name="platforms-tab"></a>플랫폼 탭

**플랫폼** 탭에는 각 플랫폼을 위한 다른 리소스(예: 시스템 이미지)와 플랫폼 SDK 버전의 목록이 표시됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![플랫폼 창의 스크린샷](android-sdk-images/win/08-platforms-pane-sml.png)](android-sdk-images/win/08-platforms-pane.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![플랫폼 창의 스크린샷](android-sdk-images/mac/sdkmanager-11.png)

-----

이 화면에는 Android 버전(예: **Android 7.0**), 코드 이름(**Nougat**), API 수준(예: **24**), 상태(플랫폼이 설치된 경우 **설치됨**)가 표시됩니다. **플랫폼** 탭은 대상으로 하려는 Android API 수준에 구성 요소를 설치하는 데 사용합니다(Android 버전 및 API 수준에 대한 자세한 내용은 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md) 참조).

플랫폼의 모든 구성 요소가 설치된 경우 플랫폼 이름 옆에 확인 표시가 나타납니다. 플랫폼의 일부 구성 요소가 설치되지 않은 경우 해당 플랫폼에 대한 상자가 채워집니다. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

플랫폼의 왼쪽에 있는 **+** 상자를 클릭하면 플랫폼을 확장하여 구성 요소(및 설치된 구성 요소)를 볼 수 있습니다.
플랫폼의 구성 요소 목록 확장을 취소하려면 **-** 를 클릭합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

플랫폼의 왼쪽에 있는 **화살표**를 클릭하면 플랫폼을 확장하여 구성 요소(및 설치된 구성 요소)를 볼 수 있습니다.
플랫폼의 구성 요소 목록 확장을 취소하려면 **아래쪽 화살표**를 클릭합니다.

-----

SDK에 다른 플랫폼을 추가하려면 확인 표시가 나타날 때까지 플랫폼 옆의 상자를 클릭하여 모든 구성 요소를 설치한 후 **변경 내용 적용**을 클릭합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android SDK에 Android 7.1 Nougat 구성 요소를 추가하는 예제](android-sdk-images/win/09-adding-a-platform-sml.png)](android-sdk-images/win/09-adding-a-platform.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Android SDK에 Android 4.4 구성 요소를 추가하는 예제](android-sdk-images/mac/sdkmanager-12.png)

-----

SDK만 설치하려면 플랫폼 옆에 있는 상자를 한 번 클릭합니다. 그런 다음, 필요한 모든 개별 구성 요소를 선택할 수 있습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![일부 Android 7.1 구성 요소를 추가하는 예제](android-sdk-images/win/10-adding-some-components-sml.png)](android-sdk-images/win/10-adding-some-components.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![일부 Android 4.4 구성 요소를 추가하는 예제](android-sdk-images/mac/sdkmanager-13.png)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**변경 내용 적용** 단추 옆에 설치할 구성 요소 수가 표시됩니다. 위의 예제에서는 6개의 구성 요소를 설치할 수 있습니다. **변경 내용 적용** 단추를 클릭하면 **라이선스 승인** 화면이 표시됩니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**변경 내용 적용** 단추 옆에 설치할 구성 요소 수가 표시됩니다. **변경 내용 적용** 단추를 클릭하면 **라이선스 승인** 화면이 표시됩니다.

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![플랫폼 탭 라이선스 승인 대화 상자](android-sdk-images/win/11-license-screen.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![플랫폼 탭 라이선스 승인 대화 상자](android-sdk-images/mac/sdkmanager-14.png)

-----

사용 약관에 동의하려면 **동의**를 클릭합니다. 설치할 구성 요소가 여러 개 있는 경우 이 대화 상자가 두 번 이상 표시될 수 있습니다. 창 아래쪽에 다운로드 및 설치 진행률을 나타내는 진행률 표시줄이 표시됩니다. 다운로드 및 설치 프로세스가 완료되면(다운로드해야 하는 구성 요소 수에 따라 수 분이 소요될 수 있음) 추가된 구성 요소에 확인 표시와 **설치됨**이 표시됩니다.

이제 가장 높은 최신 Android API 수준을 대상으로 앱을 개발할 수 있습니다.


 
## <a name="summary"></a>요약

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

이 가이드에서는 Visual Studio에서 Xamarin Android SDK Manager 도구를 설치하고 사용하는 방법을 설명했습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

이 가이드에서는 Mac용 Visual Studio에서 Xamarin Android SDK Manager 도구를 사용하는 방법을 설명했습니다.

-----


## <a name="related-links"></a>관련 링크

- [Android SDK Tools에 대한 변경 내용](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)
- [SDK Tools 릴리스 정보(Google)](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
