---
title: Xamarin.Android에 대한 Android SDK 설정
description: Visual Studio에는 Xamarin.Android 앱 개발에 필요한 Android SDK 도구, 플랫폼 및 기타 구성 요소를 다운로드하는 데 사용하는 Android SDK Manager가 포함됩니다.
ms.prod: xamarin
ms.assetid: 9A857F52-2EC1-414F-8010-CEE67B60A4B4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/09/2018
ms.openlocfilehash: 6023bb4ebcc9d4adcd3408ca09bf9118d14351d4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120816"
---
# <a name="setting-up-the-android-sdk-for-xamarinandroid"></a>Xamarin.Android에 대한 Android SDK 설정

_Visual Studio에는 Xamarin.Android 앱 개발에 필요한 Android SDK 도구, 플랫폼 및 기타 구성 요소를 다운로드하는 데 사용하는 Android SDK Manager가 포함됩니다._


## <a name="overview"></a>개요

이 가이드에서는 Visual Studio 및 Mac용 Visual Studio에서 Xamarin Android SDK Manager를 사용하는 방법을 설명합니다.

> [!NOTE]
> 이 가이드는 Visual Studio 2017 및 Mac용 Visual Studio에만 적용됩니다.  

**.NET을 사용한 모바일 개발** 워크로드의 일부로 설치된 Xamarin Android SDK Manager를 사용하여 Xamarin.Android 앱 개발에 필요한 최신 Android 구성 요소를 다운로드할 수 있습니다. 이는 더 이상 사용되지 않는 Google의 독립 실행형 SDK Manager를 대체합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="requirements"></a>요구 사항

Xamarin Android SDK Manager를 사용하려면 다음이 필요합니다.

- Visual Studio 2017(Community, Professional 또는 Enterprise 버전). Visual Studio 2017 버전 15.7 이상이 필요합니다.

- Visual Studio Tools for Xamarin 버전 4.10.0 이상(**.NET을 사용한 모바일 개발** 워크로드의 일부로 설치됨). 

Xamarin Android SDK Manager는 Visual Studio 2015와 호환되지 않습니다. Visual Studio 2015 사용자는 Google에서 Android SDK에 제공하는 SDK Manager 도구를 사용해야 합니다.

Xamarin Android SDK Manager에는 (Xamarin.Android와 함께 자동으로 설치되는) Java Development Kit도 필요합니다. 여러 가지 JDK 대안 중에서 선택할 수 있습니다.

-   기본적으로 Xamarin.Android는 API 수준 24 이상을 대상으로 개발하는 경우에 필요한 [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)을 사용합니다(JDK 8은 API 24 미만도 지원함).

-   API 수준 23 이하를 대상으로 개발하는 경우 [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)을 계속 사용할 수 있습니다.

-   Visual Studio 15.8 미리 보기 5 이상을 사용하는 경우 JDK 8 대신 [Microsoft의 모바일 OpenJDK 배포](openjdk.md)를 사용해 볼 수 있습니다(현재 미리 보기 상태).

> [!IMPORTANT]
> Xamarin.Android는 JDK 9를 지원하지 않습니다.

 
## <a name="sdk-manager"></a>SDK Manager 

Visual Studio의 SDK Manager를 시작하려면 **도구 -> Android-> Android SDK Manager**를 클릭합니다.

[![Android SDK Manager 메뉴 항목의 위치](android-sdk-images/win/02-sdk-manager-menu-item-sml.png)](android-sdk-images/win/02-sdk-manager-menu-item.png#lightbox)

Android SDK Manager는 **Android SDK 및 도구** 화면에서 열립니다. 이 화면에는 **플랫폼** 및 **도구**라는 두 개의 탭이 있습니다.

[![플랫폼 탭에서 열린 Android SDK Manager의 스크린샷](android-sdk-images/win/03-sdk-manager-platforms-sml.png)](android-sdk-images/win/03-sdk-manager-platforms.png#lightbox)

**Android SDK 및 도구** 화면에 대해서는 다음 섹션에서 자세히 설명합니다.


### <a name="android-sdk-location"></a>Android SDK 위치

Android SDK 위치는 이전 스크린샷에서 볼 수 있는 것처럼 **Android SDK 및 도구** 화면의 상단에서 구성합니다. 이 위치를 올바르게 구성해야 **플랫폼** 및 **도구** 탭이 올바르게 작동합니다. 다음 이유 중 하나 이상으로 인해 Android SDK의 위치를 설정해야 할 수 있습니다.

1. Android SDK Manager에서 Android SDK를 찾을 수 없는 경우. 

2. Android SDK를 대체(기본값이 아닌) 위치에 설치한 경우. 

Android SDK의 위치를 설정하려면 **Android SDK 위치**의 맨 오른쪽에 있는 줄임표(&hellip;) 단추를 클릭합니다. 그러면 Android SDK의 위치로 이동하는 데 사용할 수 있는 **폴더 찾아보기** 대화 상자가 열립니다. 다음 스크린샷에서는 **Program Files (x86)\\Android**에서 Android SDK를 선택합니다.

![Android SDK를 찾는 Windows 폴더 찾아보기 대화 상자의 스크린샷](android-sdk-images/win/05-browse-for-folder.png)

**확인**을 클릭하면 SDK Manager가 선택된 위치에 설치된 Android SDK를 관리합니다.


### <a name="tools-tab"></a>도구 탭

**도구** 탭에는 _도구_ 및 _추가 기능_ 목록이 표시됩니다. 이 탭을 사용하여 Android SDK 도구, 플랫폼 도구 및 빌드 도구를 설치할 수 있습니다.
또한 Android Emulator, 하위 수준 디버거(LLDB), NDK, HAXM 가속화 및 Google Play 라이브러리를 설치할 수 있습니다.


예를 들어 Google Android Emulator 패키지를 다운로드하려면 **Android Emulator** 옆에 있는 확인 표시를 클릭하고 **변경 내용 적용** 단추를 클릭합니다.

[![도구 탭에서 Android Emulator 설치](android-sdk-images/win/06-install-emulator-sml.png)](android-sdk-images/win/06-install-emulator.png#lightbox)

메시지와 함께 대화 상자가 표시될 수 있습니다. _다음 패키지를 설치하려면 설치하기 전에 사용 조건에 동의해야 합니다_.

![라이선스 승인 화면](android-sdk-images/win/07-license-acceptance.png)

사용 약관에 동의하려면 **동의**를 클릭합니다. 창 아래쪽에 다운로드 및 설치 진행률을 나타내는 진행률 표시줄이 표시됩니다. 설치가 완료되면 **도구** 탭에 선택한 도구 및 추가 기능이 설치된 것으로 표시됩니다.

### <a name="platforms-tab"></a>플랫폼 탭

**플랫폼** 탭에는 각 플랫폼을 위한 다른 리소스(예: 시스템 이미지)와 플랫폼 SDK 버전의 목록이 표시됩니다.

[![플랫폼 창의 스크린샷](android-sdk-images/win/08-platforms-pane-sml.png)](android-sdk-images/win/08-platforms-pane.png#lightbox)

이 화면에는 Android 버전(예: **Android 8.0**), 코드 이름(**Oreo**), API 수준(예: **26**), 해당 플랫폼의 구성 요소 크기(예: **1GB**)가 나열됩니다. **플랫폼** 탭을 사용하여 대상으로 지정하려는 Android API 수준에 대한 구성 요소를 설치합니다. Android 버전 및 API 수준에 대한 자세한 내용은 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조하세요.

플랫폼의 모든 구성 요소가 설치된 경우 플랫폼 이름 옆에 확인 표시가 나타납니다. 플랫폼의 일부 구성 요소가 설치되지 않은 경우 해당 플랫폼에 대한 상자가 채워집니다. 플랫폼의 왼쪽에 있는 **+** 상자를 클릭하면 플랫폼을 확장하여 구성 요소(및 설치된 구성 요소)를 볼 수 있습니다.
플랫폼의 구성 요소 목록 확장을 취소하려면 **-** 를 클릭합니다.

SDK에 다른 플랫폼을 추가하려면 확인 표시가 나타날 때까지 플랫폼 옆의 상자를 클릭하여 모든 구성 요소를 설치한 후 **변경 내용 적용**을 클릭합니다.

[![Android SDK에 Android 7.1 Nougat 구성 요소를 추가하는 예제](android-sdk-images/win/09-adding-a-platform-sml.png)](android-sdk-images/win/09-adding-a-platform.png#lightbox)

특정 구성 요소만 설치하려면 플랫폼 옆에 있는 상자를 한 번 클릭합니다. 그런 다음, 필요한 모든 개별 구성 요소를 선택할 수 있습니다.

[![일부 Android 7.1 구성 요소를 추가하는 예제](android-sdk-images/win/10-adding-some-components-sml.png)](android-sdk-images/win/10-adding-some-components.png#lightbox)

**변경 내용 적용** 단추 옆에 설치할 구성 요소 수가 표시됩니다. 앞에 표시된 대로 **변경 내용 적용** 단추를 클릭하면 **라이선스 승인** 화면이 표시됩니다.
사용 약관에 동의하려면 **동의**를 클릭합니다. 설치할 구성 요소가 여러 개 있는 경우 이 대화 상자가 두 번 이상 표시될 수 있습니다. 창 아래쪽에 다운로드 및 설치 진행률을 나타내는 진행률 표시줄이 표시됩니다. 다운로드 및 설치 프로세스가 완료되면(다운로드해야 하는 구성 요소 수에 따라 수 분이 소요될 수 있음) 추가된 구성 요소에 확인 표시와 **설치됨**이 표시됩니다.

### <a name="respository-selection"></a>리포지토리 선택 영역

기본적으로 Android SDK Manager는 Microsoft 관리 리포지토리에서 플랫폼 구성 요소 및 도구를 다운로드합니다. Microsoft 리포지토리에서 아직 사용할 수 없는 실험적 알파/베타 플랫폼 및 도구에 액세스해야 하는 경우 Google의 리포지토리를 사용하도록 SDK Manager를 전환할 수 있습니다. 이렇게 전환하려면 오른쪽 아래 모서리에서 기어 아이콘을 클릭하고 **리포지토리 > Google(지원되지 않음)** 을 선택합니다.

[![Google의 리포지토리 선택](android-sdk-images/win/11-google-repo-w157-sml.png)](android-sdk-images/win/11-google-repo-w157.png#lightbox)

Google 리포지토리를 선택하면 추가 패키지가 이전에 지원되지 않던 **플랫폼** 탭에 표시될 수 있습니다. (위의 스크린 샷에서 Google 리포지토리로 전환하여 **Android SDK 플랫폼 28**이 추가되었습니다.) Google 리포지토리를 사용하도록 지원되지 않으며 따라서 일상적인 개발에 권장되지 않습니다.

플랫폼 및 도구에서 지원되는 리포지토리를 다시 전환하려면 **Microsoft(권장)** 를 클릭합니다. 그러면 패키지 및 도구 목록을 기본 선택 영역으로 복원합니다.


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="requirements"></a>요구 사항

Xamarin Android SDK Manager를 사용하려면 다음이 필요합니다.

-   Mac용 Visual Studio 7.5 이상

Xamarin Android SDK Manager에는 (Xamarin.Android와 함께 자동으로 설치되는) Java Development Kit도 필요합니다. 여러 가지 JDK 대안 중에서 선택할 수 있습니다.

-   기본적으로 Xamarin.Android는 API 수준 24 이상을 대상으로 개발하는 경우에 필요한 [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)을 사용합니다(JDK 8은 API 24 미만도 지원함).

-   API 수준 23 이하를 대상으로 개발하는 경우 [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)을 계속 사용할 수 있습니다.

-   Mac용 Visual Studio 7.7 이상을 사용하는 경우 JDK 8 대신 [Microsoft의 모바일 OpenJDK 배포](openjdk.md)를 사용해 볼 수 있습니다(현재 미리 보기 상태).

> [!IMPORTANT]
> Xamarin.Android는 JDK 9를 지원하지 않습니다.
 
## <a name="sdk-manager"></a>SDK Manager 

Mac용 Visual Studio의 SDK Manager를 시작하려면 **도구 -> SDK Manager**를 클릭합니다.
 
[![Android SDK Manager 메뉴 항목의 위치](android-sdk-images/mac/01-sdk-manager-menu-item-m75-sml.png)](android-sdk-images/mac/01-sdk-manager-menu-item-m75.png#lightbox)

**Android SDK Manager**는 **플랫폼**, **도구** 및 **위치**라는 세 개의 탭이 있는 **기본 설정 창**에서 열 수 있습니다.

[![플랫폼 탭에서 열린 Android SDK Manager의 스크린샷](android-sdk-images/mac/02-sdk-manager-platforms-m75-sml.png)](android-sdk-images/mac/02-sdk-manager-platforms-m75.png#lightbox)

Android SDK Manager의 탭에 대해서는 다음 섹션에서 자세히 설명합니다.


### <a name="locations-tab"></a>위치 탭

**위치** 탭에는 Android SDK, Android NDK 및 Java SDK(JDK)의 위치를 구성하기 위한 세 가지 설정이 있습니다. 이 위치를 올바르게 구성해야 **플랫폼** 및 **도구** 탭이 올바르게 작동합니다.

SDK Manager를 시작하면 설치된 각 패키지의 경로가 자동으로 확인되고, 경로 옆에 녹색 확인 표시 아이콘에 표시되며 **있음**을 알려줍니다.

[![위치 탭의 스크린 샷](android-sdk-images/mac/03-locations-tab-m75-sml.png)](android-sdk-images/mac/03-locations-tab-m75.png#lightbox)

SDK Manager가 기본 위치에 있는 SDK, NDK 및 JDK를 찾도록 하려면 **기본값으로 다시 설정** 단추를 클릭합니다. 

일반적으로 **위치** 탭을 사용하여 Android SDK 및/또는 Java JDK의 위치를 수정할 수 있습니다. Xamarin.Android 앱 개발을 위해 NDK를 설치할 필요는 없습니다. NDK는 앱의 일부분을 C 및 C++와 같은 네이티브 코드 언어를 사용하여 개발해야 하는 경우에만 사용됩니다.

### <a name="tools-tab"></a>도구 탭

**도구** 탭에는 _도구_ 및 _추가 기능_ 목록이 표시됩니다. 이 탭을 사용하여 Android SDK 도구, 플랫폼 도구 및 빌드 도구를 설치할 수 있습니다.
또한 Android Emulator, 하위 수준 디버거(LLDB), NDK, HAXM 가속화 및 Google Play 라이브러리를 설치할 수 있습니다.

예를 들어 Google Android Emulator 패키지를 다운로드하려면 **Android Emulator** 옆에 있는 확인 표시를 클릭하고 **변경 내용 적용** 단추를 클릭합니다.

[![도구 탭에서 Android Emulator 설치](android-sdk-images/mac/04-tools-tab-m75-sml.png)](android-sdk-images/mac/04-tools-tab-m75.png#lightbox)

메시지와 함께 대화 상자가 표시될 수 있습니다. _다음 패키지를 설치하려면 설치하기 전에 사용 조건에 동의해야 합니다_.

[![라이선스 승인 화면](android-sdk-images/mac/05-license-acceptance-m75-sml.png)](android-sdk-images/mac/05-license-acceptance-m75.png#lightbox)

사용 약관에 동의하려면 **동의**를 클릭합니다. 창 아래쪽에 다운로드 및 설치 진행률을 나타내는 진행률 표시줄이 표시됩니다. 설치가 완료되면 **도구** 탭에 선택한 도구 및 추가 기능이 설치된 것으로 표시됩니다.


### <a name="platforms-tab"></a>플랫폼 탭

**플랫폼** 탭에는 각 플랫폼을 위한 다른 리소스(예: 시스템 이미지)와 플랫폼 SDK 버전의 목록이 표시됩니다.

[![플랫폼 창의 스크린샷](android-sdk-images/mac/06-platforms-tab-m75-sml.png)](android-sdk-images/mac/06-platforms-tab-m75.png#lightbox)

이 화면에는 Android 버전(예: **Android 8.1**), 코드 이름(**Oreo**), API 수준(예: **27**), 해당 플랫폼의 구성 요소 크기(예: **1GB**)가 나열됩니다. **플랫폼** 탭을 사용하여 대상으로 지정하려는 Android API 수준에 대한 구성 요소를 설치합니다. Android 버전 및 API 수준에 대한 자세한 내용은 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조하세요.

플랫폼의 모든 구성 요소가 설치된 경우 플랫폼 이름 옆에 확인 표시가 나타납니다. 플랫폼의 일부 구성 요소가 설치되지 않은 경우 해당 플랫폼에 대한 상자가 채워집니다. 플랫폼의 왼쪽에 있는 **화살표**를 클릭하면 플랫폼을 확장하여 구성 요소(및 설치된 구성 요소)를 볼 수 있습니다.
플랫폼의 구성 요소 목록 확장을 취소하려면 **아래쪽 화살표**를 클릭합니다.

SDK에 다른 플랫폼을 추가하려면 확인 표시가 나타날 때까지 플랫폼 옆의 상자를 클릭하여 모든 구성 요소를 설치한 후 **변경 내용 적용**을 클릭합니다.

[![플랫폼의 모든 구성 요소를 추가하는 예제](android-sdk-images/mac/07-install-all-m75-sml.png)](android-sdk-images/mac/07-install-all-m75.png#lightbox)

일부 구성 요소만 설치하려면 플랫폼 옆에 있는 상자를 한 번 클릭합니다. 그런 다음, 필요한 모든 개별 구성 요소를 선택할 수 있습니다.

[![일부 구성 요소를 추가하는 예제](android-sdk-images/mac/08-individual-components-m75-sml.png)](android-sdk-images/mac/08-individual-components-m75.png#lightbox)

**변경 내용 적용** 단추 옆에 설치할 구성 요소 수가 표시됩니다. 앞에 표시된 대로 **변경 내용 적용** 단추를 클릭하면 **라이선스 승인** 화면이 표시됩니다.
사용 약관에 동의하려면 **동의**를 클릭합니다. 설치할 구성 요소가 여러 개 있는 경우 이 대화 상자가 두 번 이상 표시될 수 있습니다. 창 아래쪽에 다운로드 및 설치 진행률을 나타내는 진행률 표시줄이 표시됩니다. 다운로드 및 설치 프로세스가 완료되면(다운로드해야 하는 구성 요소 수에 따라 수 분이 소요될 수 있음) 추가된 구성 요소에 확인 표시와 **설치됨**이 표시됩니다.

### <a name="respository-selection"></a>리포지토리 선택 영역

기본적으로 Android SDK Manager는 Microsoft 관리 리포지토리에서 플랫폼 구성 요소 및 도구를 다운로드합니다. Microsoft 리포지토리에서 아직 사용할 수 없는 실험적 알파/베타 플랫폼 및 도구에 액세스해야 하는 경우 Google의 리포지토리를 사용하도록 SDK Manager를 전환할 수 있습니다. 이렇게 전환하려면 오른쪽 아래 모서리에서 기어 아이콘을 클릭하고 **리포지토리 > Google(지원되지 않음)** 을 선택합니다.

[![Google의 리포지토리 선택](android-sdk-images/mac/09-google-repo-m75-sml.png)](android-sdk-images/mac/09-google-repo-m75.png#lightbox)

Google 리포지토리를 선택하면 추가 패키지가 이전에 지원되지 않던 **플랫폼** 탭에 표시될 수 있습니다. (위의 스크린 샷에서 Google 리포지토리로 전환하여 **Android SDK 플랫폼 28**이 추가되었습니다.) Google 리포지토리를 사용하도록 지원되지 않으며 따라서 일상적인 개발에 권장되지 않습니다.

플랫폼 및 도구에서 지원되는 리포지토리를 다시 전환하려면 **Microsoft(권장)** 를 클릭합니다. 그러면 패키지 및 도구 목록을 기본 선택 영역으로 복원합니다.

-----

 
## <a name="summary"></a>요약

이 가이드에서는 Visual Studio 및 Mac용 Visual Studio에서 Xamarin Android SDK Manager 도구를 설치하고 사용하는 방법을 설명했습니다.


## <a name="related-links"></a>관련 링크

- [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)
- [Android SDK Tools에 대한 변경 내용](~/android/troubleshooting/sdk-cli-tooling-changes.md)
