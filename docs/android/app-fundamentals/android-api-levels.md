---
title: Android API 수준 이해
description: Xamarin.Android에 여러 버전의 Android 사용 하 여 앱의 호환성을 확인 하는 여러 Android API 수준 설정이 있습니다. 이 가이드에서는 이러한 설정의 역할 구성 하는 방법 및 어떤 영향을 설명 런타임 시 앱에 해당 합니다.
ms.prod: xamarin
ms.assetid: 58CB7B34-3140-4BEB-BE2E-209928C1878C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2018
ms.openlocfilehash: aa522e5226d78c1b43bb52b97991b989491d251f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120062"
---
# <a name="understanding-android-api-levels"></a>Android API 수준 이해

_Xamarin.Android에 여러 버전의 Android 사용 하 여 앱의 호환성을 확인 하는 여러 Android API 수준 설정이 있습니다. 이 가이드에서는 이러한 설정의 역할 구성 하는 방법 및 어떤 영향을 설명 런타임 시 앱에 해당 합니다._


## <a name="quick-start"></a>빠른 시작

Xamarin.Android는 세 개의 Android API 수준 프로젝트 설정을 노출합니다.

-   [대상 프레임 워크](#framework) &ndash; 응용 프로그램 구축에 사용할 수 있는 프레임 워크를 지정 합니다. 이 API 수준에서 사용 됩니다 *컴파일* Xamarin.Android에서 시간입니다.

-   [최소 Android 버전](#minimum) &ndash; 를 지원 하도록 앱 하려는 가장 오래 된 Android 버전을 지정 합니다. 이 API 수준에서 사용 됩니다 *실행* Android에서 시간입니다.

-   [대상 Android 버전](#target) &ndash; 앱이 Android의 버전 실행 되도록 지정 합니다. 이 API 수준에서 사용 됩니다 *실행* Android에서 시간입니다.

프로젝트에 대 한 API 수준 구성 하려면, 먼저 해당 API 수준 SDK 플랫폼 구성 요소를 설치 해야 합니다. 다운로드 하 고 Android SDK 구성 요소를 설치 하는 방법에 대 한 자세한 내용은 참조 하세요. [Android SDK 설치](~/android/get-started/installation/android-sdk.md)합니다.

> [!NOTE]
> 2018 년 8 월부터, Google Play 콘솔을 해야는 새 앱의 대상 API 수준 26 (Android 8.0) 이상.
기존 앱 API 수준 26 또는 2018 년 11 월부터 더 높은 대상으로 해야 합니다. 자세한 내용은 [앱 보안 및 야 오랫동안 Google Play에서 성능 향상](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html)합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

일반적으로 세 Xamarin.Android API 수준 모두 동일한 값으로 설정 됩니다. 에 **응용 프로그램** 페이지에서 설정 **Android 버전 (대상 프레임 워크)를 사용 하 여 컴파일** 최신 안정적인 API 버전 (또는 모든 필요한 기능을 Android 버전 이상).
대상 프레임 워크를로 다음 스크린 샷에서 **Android 7.1 (API 레벨 25-Nougat)**:

[![대상 프레임 워크 버전 기본값으로 Android 버전을 사용 하 여 컴파일](android-api-levels-images/vs-defaults-sml.png)](android-api-levels-images/vs-defaults.png#lightbox)

에 **Android 매니페스트** 페이지에서 최소 Android 버전을 설정 합니다 **사용 하 여 SDK 버전을 사용 하 여 컴파일** 대상 Android 버전 (다음에에서 대상 프레임 워크 버전으로 동일한 값으로 설정 하 고 스크린 샷, 대상 Android 프레임 워크 설정할지 **Android 7.1 (Nougat)**):

[![대상 프레임 워크 버전을 최소 및 대상 Android 버전 설정](android-api-levels-images/vs-manifest-defaults-sml.png)](android-api-levels-images/vs-manifest-defaults.png#lightbox)

이전 버전의 Android 사용 하 여 이전 버전과 호환성을 유지 하려는 경우 설정할 **대상으로 하는 최소 Android 버전** 가장 오래 된 버전의 Android 앱을 지원 한다고 합니다. (API 수준 14에 필요한 최소 API 수준에는 [Google Play 서비스 및 Firebase 지원](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).) 다음 예제에서는 구성을 통해 API 레벨 25 API 수준 14에서 Android 버전을 지원합니다.

[![API 레벨 25를 사용 하 여 컴파일 Nougat, API 수준 14로 설정 하는 최소 Android 버전](android-api-levels-images/vs-minimum-sml.png)](android-api-levels-images/vs-minimum.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

일반적으로 세 Xamarin.Android API 수준 모두 동일한 값으로 설정 됩니다. 설정할 **대상 프레임 워크** 최신 안정적인 API 버전 (또는 모든 필요한 기능을 Android 버전 이상). 설정 하는 **대상 프레임 워크**, 이동할 **빌드 > 일반** 에 **프로젝트 옵션**합니다. 대상 프레임 워크를로 다음 스크린 샷에서 **가장 최근에 설치 된 플랫폼 (8.0)를 사용 하 여**:

[![대상 프레임 워크를 사용 하 여 가장 최근에 설치 된 플랫폼에 기본 설정](android-api-levels-images/xs-default-target-sml.png)](android-api-levels-images/xs-default-target.png#lightbox)

최소 및 대상 Android 버전 설정에서 찾을 수 있습니다 **빌드 > Android 응용 프로그램** 에 **프로젝트 옵션**합니다. 최소 Android 버전을 설정 합니다 **자동으로 사용 하 여 대상 프레임 워크 버전** 대상 Android 버전은 대상 프레임 워크 버전 같은 값으로 설정 합니다. 대상 Android 프레임 워크를로 다음 스크린 샷에서 **Android 8.0 (API 레벨 26)** 위의 대상 프레임 워크 설정과 일치 시킵니다.

[![프로젝트 옵션에서 대상 및 프레임 워크 수준 설정](android-api-levels-images/xs-default-app-sml.png)](android-api-levels-images/xs-default-app.png#lightbox)

이전 버전의 Android 사용 하 여 이전 버전과 호환성을 유지 하려는 경우 변경할 **최소 Android 버전** 가장 오래 된 버전의 Android 앱을 지원 한다고 합니다. API 수준 14에 필요한 최소 API 수준에는 [Google Play 서비스 및 Firebase 지원](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html)합니다.
예를 들어, 다음 구성을 Android 버전 API 수준 14 빨리를 지원합니다.

[![-자동으로 설정 대상 버전과 최소 대상 프레임 워크 버전을 사용 합니다.](android-api-levels-images/xs-minimum-sml.png)](android-api-levels-images/xs-minimum.png#lightbox)

-----


앱에서 여러 Android 버전을 지 원하는 코드 최소 Android 버전 설정을 사용 하 여 앱이 작동 하도록 런타임 검사를 포함 해야 합니다 (참조 [Android 버전에 대 한 런타임 검사](#runtimechecks) 아래 세부 정보에 대 한). 사용 하거나 라이브러리 만들기를 참조 하세요 [API 수준 및 라이브러리](#libraries) 아래 API를 구성 하는 모범 사례에 대 한 수준 라이브러리에 대 한 설정입니다.



## <a name="android-versions-and-api-levels"></a>Android 버전 및 API 수준

각 Android 버전 이라는 고유 정수 식별자를 할당 된 Android 플랫폼 발전와 새 Android 버전이 출시 되는 *API 수준*합니다. 따라서 각 Android 버전 단일 Android API 수준에 해당합니다. 사용자가 이전도 가장 최근 버전의 Android에서 앱을 설치 하기 때문에 실제 Android 앱 여러 Android API 수준을 사용 하 여 작동 하도록 디자인 되어야 합니다.


### <a name="android-versions"></a>Android 버전

Android의 각 릴리스 여러 이름으로 이동 합니다.

-   Android 버전을 같은 **Android 9.0**
-   코드 (또는 후 식) 이름, 같은 _원형_
-   해당 API 수준에 같은 **API 수준 28**

여러 버전 및 API 수준 (처럼 아래 표에서)에 Android 코드명 해당할 수 있습니다 하지만 각 Android 버전은 정확히 하나의 API 수준에 해당 합니다.

또한 Xamarin.Android 정의 *버전 코드를 빌드* 는 현재 알려진된 Android API 수준에 매핑되는 합니다. 다음 표에서 도움이 될 수 있습니다 API 수준, Android 버전, 코드 이름 및 Xamarin.Android 빌드 버전 코드 사이 변환 (빌드 버전 코드에 정의 된는 `Android.OS` 네임 스페이스):

[!include[](~/android/includes/api-levels.md)]

새 Android 버전은 자주 출시이 표에 나와 있듯이 &ndash; 연간 종종 둘 이상의 릴리스 합니다. 결과적으로, 앱이 실행 될 수 있는 Android 장치의 universe의 이전 및 최신 Android 버전의 다양 한 포함 되어 있습니다. 앱에서 실행 됩니다 일관 되 고 안정적으로 많은 다양 한 버전의 Android 어떻게 보장할 수 있습니다? Android의 API 수준을이 문제를 관리할 수 있습니다.


### <a name="android-api-levels"></a>Android API 수준

각 Android 장치에서 실행에 정확 하 게 *하나* API 수준 &ndash; 이 API 수준 Android 플랫폼 버전 당 고유 하 게 보장 됩니다. API 수준 앱;으로 호출할 수 있는 API 집합의 버전을 정확 하 게 식별 개발자로 서 매니페스트 요소, 사용 권한에 대 한 코드는 등의 조합을 식별합니다. API 수준의 android의 시스템을 사용 하면 Android 응용 프로그램을 장치에 응용 프로그램을 설치 하기 전에 Android 시스템 이미지와 호환 되는지 확인 합니다.

응용 프로그램 작성 되 면 다음 API 수준 정보를 포함 합니다.

-   합니다 *대상* 앱에서 실행 되도록 빌드되는 android API 레벨입니다.

-   합니다 *최소* 앱을 실행 하려면 Android 장치를 포함 해야 하는 Android API 레벨입니다. 

설치 시 앱을 올바르게 실행 하는 데 필요한 기능을 Android 장치에서 사용할 수 있도록 하려면 이러한 설정이 사용 됩니다. 그렇지 않은 경우 앱은 해당 장치에서 실행에서 차단 됩니다. 예를 들어 Android 장치의 API 레벨 앱에 대 한 지정 된 최소 API 수준 보다 낮은 경우 Android 장치는 앱 설치에서 사용자를 하지 것입니다.


## <a name="project-api-level-settings"></a>API 프로젝트 수준 설정

다음 섹션에서는 SDK Manager를 사용 하 여 뒤에 구성 하는 방법에 대 한 세부 정보를 대상으로 하려는 API 수준에 대 한 개발 환경을 준비 하는 방법에 설명 *대상 프레임 워크*, *최소 Android 버전*, 및 *대상 Android 버전* Xamarin.Android에서 설정 합니다.


### <a name="android-sdk-platforms"></a>Android SDK 플랫폼

Xamarin.android에서를 대상 또는 최소 API 수준을 선택할 수 있습니다, API 수준에 해당 하는 Android SDK 플랫폼 버전을 설치 해야 합니다. 대상 프레임 워크, 최소 Android 버전 및 대상 Android 버전에 대 한 사용 가능한 선택 항목의 범위는 설치 된 Android SDK의 범위 버전으로 제한 됩니다. 필요한 Android SDK 버전을 설치할 되었고 앱에 필요한 모든 새 API 수준을 추가 하려면 사용할 수 있는지 확인 하려면 SDK Manager를 사용할 수 있습니다. API 수준을 설치 하는 방법을 잘 모르는 경우 [Android SDK 설치](~/android/get-started/installation/android-sdk.md)합니다.

<a name="framework" />

### <a name="target-framework"></a>대상 프레임워크

합니다 *대상 프레임 워크* (라고도 `compileSdkVersion`)는 특정 Android 프레임 워크 버전 (API 레벨) 빌드 시 앱에 대해 컴파일되는입니다. 이 설정은 Api 앱 *예상* 데 실행 되었지만 Api에는 앱에 실제로 사용 가능한 설치 될 때 영향을 주지 않습니다. 결과적으로, 대상 프레임 워크 설정을 변경 해도 런타임 동작은 변경 되지 않습니다.

응용 프로그램에 대해 연결 된 라이브러리 버전을 식별 하는 대상 프레임 워크 &ndash; 이 설정은 앱에서 사용할 수 있는 Api를 결정 합니다. 예를 들어 사용 하려는 경우는 [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) Android 5.0 Lollipop에 도입 된 메서드를 대상 프레임 워크를 설정 해야 **API 수준 21 (Lollipop)** 이상. 프로젝트의 대상 프레임 워크 API를 같은 수준으로 설정 하면 **API 수준 19 (KitKat)** 호출을 시도 하 고는 `SetCategory` 코드에서 메서드를 컴파일 오류가 발생 합니다.

사용 하 여 항상 컴파일하는 것이 좋습니다 합니다 *최신* 대상 프레임 워크 버전을 사용할 수 있습니다. 이렇게 코드에서 호출할 수 있는 모든 사용 되지 않는 Api에 대 한 유용한 경고 메시지를 제공 합니다. 최신 대상 프레임 워크 버전을 사용 하는 최신 지원 라이브러리 버전을 사용 하는 경우 특히 중요 &ndash; 각 라이브러리는 앱을 지원 라이브러리의 최소 API 수준에서 컴파일된 이상 이어야 합니다. 


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서 대상 프레임 워크 설정에 액세스 하려면 프로젝트 속성에서 엽니다 **솔루션 탐색기** 선택 합니다 **응용 프로그램** 페이지:

[![프로젝트 속성의 응용 프로그램 페이지](android-api-levels-images/vs-target-framework-sml.png)](android-api-levels-images/vs-target-framework.png#lightbox)

API 수준 아래에 있는 드롭다운 메뉴에서 선택 하 여 대상 프레임 워크를 설정 **Android 버전을 사용 하 여 컴파일** 위와 같이 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac 용 Visual Studio에서 대상 프레임 워크 설정에 액세스 하려면 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **옵션**이 열립니다 합니다 **프로젝트 옵션** 대화 합니다. 이 대화 상자에서 이동 **빌드 > 일반** 다음과 같이 합니다.

[![빌드 프로젝트 옵션 페이지의 일반 섹션](android-api-levels-images/xs-target-framework-sml.png)](android-api-levels-images/xs-target-framework.png#lightbox)

API 수준 오른쪽의 드롭다운 메뉴에서 선택 하 여 대상 프레임 워크를 설정 **대상 프레임 워크** 위와 같이 합니다.

-----


<a name="minimum" />

### <a name="minimum-android-version"></a>최소 Android 버전

합니다 *최소 Android 버전* (라고도 `minSdkVersion`) 설치 하 고 응용 프로그램을 실행할 수 있는 Android OS (즉, 가장 낮은 API 레벨)의 가장 오래 된 버전입니다. 기본적으로 앱만 수 이상 대상 프레임 워크 설정이 일치 하는 장치에 설치 최소 Android 버전 설정이 *낮은* 대상 프레임 워크 설정 보다 앱 이전 버전의 Android에서 실행할 수도 있습니다. 예를 들어, 대상 프레임 워크를 설정 하면 **Android 7.1 (Nougat)** 최소 Android 버전을 설정 하 고 **Android 4.0.3 (Ice Cream Sandwich)**, API 수준 15에서에서 모든 플랫폼에서 앱을 설치할 수 있습니다 API 수준 25 포함 합니다.

성공적으로 앱 빌드하고이 다양 한 플랫폼에 설치할 수 있습니다, 있지만 이렇게 해도 성공적으로 됩니다 *실행* 이러한 플랫폼의 모든. 예를 들어, 앱에 설치 되어 있으면 **Android 5.0 (Lollipop)** 코드 에서만 사용할 수 있는 API를 호출 하 고 **Android 7.1 (Nougat)** 이상에서는 앱 되며 런타임 오류가 발생할 수 있는 충돌 합니다. 따라서 코드 확인 해야 합니다 &ndash; 런타임 시 &ndash; 에서 실행 되는 Android 장치에서 지원 되는 이러한 Api를 호출 하는 것입니다. 즉, 코드 앱 최신 지원 되는 장치에만 새 Api를 사용 하도록 명시적 런타임 검사를 포함 해야 합니다.
[Android 버전에 대 한 런타임 검사](#runtimechecks)이 가이드의 뒷부분에 나오는 코드에 이러한 런타임 검사를 추가 하는 방법에 설명 합니다.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio의 최소 Android 버전 설정에 액세스 하려면 프로젝트 속성에서 엽니다 **솔루션 탐색기** 선택 합니다 **Android 매니페스트** 페이지입니다. 드롭다운 메뉴에서 **최소 Android 버전** 응용 프로그램에 대 한 최소 Android 버전을 선택할 수 있습니다.

[![최소 Android SDK 버전을 사용 하 여 컴파일로 설정 하는 대상 옵션](android-api-levels-images/vs-minimum-version-sml.png)](android-api-levels-images/vs-minimum-version.png#lightbox)

선택 하는 경우 **사용 하 여 SDK 버전을 사용 하 여 컴파일**, 최소 Android 버전 대상 프레임 워크와 동일 하 게 됩니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac 용 Visual Studio의 최소 Android 버전에 액세스 하려면 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **옵션**이 열립니다 합니다 **프로젝트 옵션** 대화 합니다. 이동할 **빌드 > Android 응용 프로그램**합니다.
오른쪽의 드롭다운 메뉴를 사용 하 여 **최소 Android 버전**, 응용 프로그램에 대 한 최소 Android 버전을 설정할 수 있습니다.

[![자동으로 사용 하 여 대상 프레임 워크 버전으로 설정 하는 최소 Android 버전](android-api-levels-images/xs-minimum-version-sml.png)](android-api-levels-images/xs-minimum-version.png#lightbox)

선택 하는 경우 **자동 &ndash; 대상 프레임 워크 버전을 사용 하 여**, 최소 Android 버전 대상 프레임 워크와 동일 하 게 됩니다.

-----


<a name="target" />

### <a name="target-android-version"></a>대상 Android 버전

합니다 *대상 Android 버전* (라고도 `targetSdkVersion`)는 API 수준 Android 장치에 앱 실행에 예상 되는 위치입니다. Android를 호환성 동작을 사용할지 여부를 확인 하려면이 설정을 사용 &ndash; 앱 계속 예상 대로 작동 하는지는 것이 이렇게 합니다. Android 앱의 대상 Android 버전 설정을 사용 하 여 (이 Android 버전과 호환성을 제공 하는 방법) 없이 앱에 적용할 수 있는 동작 변경 내용 파악.

대상 프레임 워크 및 이름이 매우 유사한 하면서 대상 Android 버전, 동일 하지 않습니다. 대상 프레임 워크 설정을 대상 API 수준 정보 xamarin.android에서 사용 하 여 통신 *컴파일 타임*대상 Android 버전에 사용할 android 대상 API 수준 정보를 통신 하는 동안,  *런타임에* (앱의 장치에 설치 및 실행).

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서이 설정에 액세스 하려면 프로젝트 속성에서 엽니다 **솔루션 탐색기** 선택 합니다 **Android 매니페스트** 페이지입니다. 드롭다운 메뉴에서 **대상 Android 버전** 응용 프로그램에 대 한 대상 Android 버전을 선택할 수 있습니다.

[![SDK 버전을 사용 하 여 컴파일로 설정 하는 대상 Android 버전](android-api-levels-images/vs-target-version-sml.png)](android-api-levels-images/vs-target-version.png#lightbox)

명시적으로 설정 하는 대상 Android 버전의 Android 앱을 테스트 하는 데 사용 하는 최신 버전으로 유지 하는 것이 좋습니다. 이상적으로 최신 Android SDK 버전으로 설정 해야 &ndash; 동작 변경 내용 진행 하기 전에 새 Api를 사용할 수 있습니다. 대부분의 개발자를 위한 것 *하지 않습니다* 대상 Android 버전을 설정 하는 것이 좋습니다 **사용 하 여 SDK 버전을 사용 하 여 컴파일**합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac 용 Visual Studio에서이 설정에 액세스 하려면 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **옵션**이 열립니다 합니다 **프로젝트 옵션** 대화 합니다. 이동할 **빌드 > Android 응용 프로그램**합니다. 오른쪽의 드롭다운 메뉴를 사용 하 여 **대상 Android 버전**, 응용 프로그램에 대 한 대상 Android 버전을 설정할 수 있습니다.

[![대상 Android 버전 자동으로 사용 하 여 대상 프레임 워크 버전으로 설정](android-api-levels-images/xs-target-version-sml.png)](android-api-levels-images/xs-target-version.png#lightbox)

명시적으로 설정 하는 대상 Android 버전의 Android 앱을 테스트 하는 데 사용 하는 최신 버전으로 유지 하는 것이 좋습니다. 이상적으로 사용 가능한 최신 Android SDK 버전으로 설정 해야 &ndash; 동작 변경 내용 진행 하기 전에 새 Api를 사용할 수 있습니다. 대부분의 개발자를 위한 권장 하지는 않습니다 대상 Android 버전을 설정 **자동으로 사용 하 여 대상 프레임 워크 버전**합니다.

-----

일반적으로 최소 Android 버전 및 대상 프레임 워크 대상 Android 버전 묶어야 합니다. 구체적인 요건은 다음과 같습니다.

**최소 Android 버전 < 대상 Android 버전 = < = 대상 프레임 워크**

SDK 수준에 대 한 자세한 내용은 Android 개발자 참조 [사용 하 여 sdk](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html) 설명서.


<a name="runtimechecks" />

## <a name="runtime-checks-for-android-versions"></a>Android 버전에 대 한 런타임 검사

Android의 각 새 버전이 출시 되는 프레임 워크 API 새 수 있도록 업데이트 됩니다 또는 대체 기능입니다. 몇 가지 예외를 제외 하 고, 이전 Android 버전의 API 기능을 수정 하지 않고 최신 Android 버전에 앞으로 수행 됩니다. 결과적으로, 앱을 특정 Android API 수준에서 실행 하는 경우 일반적으로 됩니다 수정 하지 않고 최신 Android API 수준에서 실행할 수 있습니다. 그러나 이전 버전의 Android에서 앱을 실행 하려면 또한 원하는 경우에 어떻게?

최소 Android 버전을 선택 하는 경우 *낮은* 에 대상 프레임 워크 설정 보다 몇 가지 Api를 사용 하지 못할 런타임 시 앱에 있습니다. 그러나 앱 이전 장치에서 하지만 기능이 축소 된 상태로 계속 실행할 수 있습니다. 최소 Android 버전 설정에 해당 하는 Android 플랫폼에서 사용할 수 없는 각 API에 대 한 코드 명시적으로 확인 해야의 값을 `Android.OS.Build.VERSION.SdkInt` 앱에서 실행 되는 플랫폼의 API 수준을 결정 하는 속성입니다. API 수준이 *낮은* 를 호출 하려는 API를 지 원하는 최소 Android 버전 보다 다음 코드는이 API를 호출 하지 않고 제대로 작동 하는 방법을 찾아야 합니다.

예를 들어, 가정 사용 하고자 합니다 [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) 메서드를 실행 하는 경우 알림을 분류 **Android 5.0 Lollipop** (이상), 앱을 계속 하려고 하지만 와 같은 이전 버전의 Android에서 실행 **Android 4.1 Jelly Bean** (여기서 `SetCategory` 를 사용할 수 없습니다). Android 버전은이 가이드의 시작 부분을 참조 했습니다 보면에 대 한 빌드 버전 코드 **Android 5.0 Lollipop** 는 `Android.OS.BuildVersionCodes.Lollipop`합니다. Android의 이전 버전을 지원 하도록 `SetCategory` 는 사용할 수 없는 코드 런타임에 API 레벨을 검색 하 고 조건에 따라 호출할 수 `SetCategory` 상태일 때에 API 수준 보다 크거나 같음 롤리팝 빌드 버전 코드:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    builder.SetCategory(Notification.CategoryEmail);
}
```

이 예제에서는 앱의 대상 프레임 워크 설정할지 **Android 5.0 (API 수준 21)** 최소 Android 버전으로 설정 됩니다 **Android 4.1 (API 레벨 16)** 합니다. 때문에 `SetCategory` 는 API 수준에서 제공 됩니다 `Android.OS.BuildVersionCodes.Lollipop` 이 예제 코드를 호출 하는 나중 `SetCategory` 경우에 실제로 사용 가능한 &ndash; 됩니다 *하지* 호출 하려고 `SetCategory` 때 API 수준은 16, 17, 18, 19, 또는 20입니다. 이러한 이전 Android 버전에만 하는 알림을 제대로 (때문에 형식으로 분류 되지 됩니다) 정렬 되지 않은 아직 사용자 경고를 발생 시 알림을 여전히 게시 되어 기능이 줄어듭니다. 앱은 여전히 작동 하지만 해당 기능은 약간 감소 합니다.

일반적으로 빌드 버전 검사를 수행한 이전 방식으로 비교 하는 새로운 방법 간에 런타임 시 결정 코드를 수 있습니다. 예를 들어:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    // Do things the Lollipop way
}
else
{
    // Do things the pre-Lollipop way
}
```

규칙은 없습니다 빠르고 간단한 줄이거나 부족 하나 이상의 Api는 이전 Android 버전에서 실행 될 때 앱의 기능을 수정 하는 방법을 설명 합니다. 일부 경우에 (예:는 `SetCategory` 위의 예제)를 사용할 수 없는 경우 API 호출을 생략 하는 것이 부족 합니다. 그러나 다른 경우에 해야 시기에 대 한 대체 기능을 구현 `Android.OS.Build.VERSION.SdkInt` 보다 작아야 API 앱의 최적의 환경을 제공 해야 하는 수준으로 감지 되 면 합니다.

<a name="libraries" />

## <a name="api-levels-and-libraries"></a>API 수준 및 라이브러리

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

대상 프레임 워크 설정에만 구성할 수 있습니다 (예: 클래스 라이브러리 또는 바인딩 라이브러리)는 Xamarin.Android 라이브러리 프로젝트를 만들면 &ndash; 최소 Android 버전 및 대상 Android 버전 설정이 지원 되지 않습니다. 있기 때문에 이것이 없습니다 **Android 매니페스트** 페이지:

[![Android 버전 옵션을 사용 하 여 컴파일만 사용할 수 있습니다](android-api-levels-images/vs-library-options-sml.png)](android-api-levels-images/vs-library-options.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Xamarin.Android 라이브러리 프로젝트를 만들면 방법이 없는 **Android 응용 프로그램** 최소 Android 버전 및 대상 Android 버전을 구성할 수 있는 페이지 &ndash; 최소 Android 버전 및 대상 Android 버전 설정은 사용할 수 있습니다.
있기 때문에 이것이 없습니다 **빌드 > Android 응용 프로그램** 페이지:

[![빌드 대상 및 최소 버전 옵션 없이 일반 페이지](android-api-levels-images/xs-library-options-sml.png)](android-api-levels-images/xs-library-options.png#lightbox)

-----

최소 Android 버전 및 대상 Android 버전 설정이 사용할 수 없는 결과 라이브러리는 독립 실행형 앱 아니므로 &ndash; 라이브러리를 사용 하 여 패키지 된 앱에 따라 Android 버전에서 실행할 수 있습니다. 라이브러리를 될 하는 하는 방법을 지정할 수 있습니다 *컴파일된*, 어떤 플랫폼이 API 수준 라이브러리에서 실행할 수는 예측 수 없습니다. 이 점을 염두에서를 사용 하 여 사용 하거나 라이브러리를 만들 때 다음 모범 사례를 관찰 해야 합니다.

-   **Android 라이브러리를 사용 하는 경우** &ndash; 응용 프로그램에서 Android 라이브러리를 사용 하는 경우 설정 해야 하는 API 수준 설정 즉 앱의 대상 프레임 워크 *이상으로 높게* 대상 라이브러리의 프레임 워크 설정입니다.

-   **Android 라이브러리를 만들 때** &ndash; 다른 응용 프로그램에서 사용 하기 위해 Android 라이브러리를 만들려는 경우 사용할 컴파일하기 위해 필요한 최소 API 수준으로 해당 대상 프레임 워크 설정 해야 합니다.

라이브러리 (응용 프로그램에서 충돌을 발생할 수 있습니다)는 런타임에 사용할 수 없는 API를 호출 하려고 하는 상황을 방지 하는 데 이러한 모범 사례를 사용 하는 것이 좋습니다. 라이브러리 개발자 인 경우에 총 API 노출 영역이 작고 잘 구성 된 하위 집합에 대 한 API 호출의 사용량을 제한 하도록 해야 합니다. 따라서 수행 하는 라이브러리를 사용할 수 있도록 안전 하 게는 더 광범위 한 Android 버전 간에 수 있습니다.


## <a name="summary"></a>요약

이 가이드에서는 Android API 수준에서 서로 다른 버전의 Android 앱 호환성 관리를 사용 하는 방법 설명 했습니다. Xamarin.Android를 구성 하기 위한 단계를 자세히 제공 *대상 프레임 워크*를 *최소 Android 버전*, 및 *대상 Android 버전* 프로젝트 설정 합니다. SDK 패키지를 설치 하려면 Android SDK Manager를 사용 하는 지침 런타임 시 다른 API 수준으로 처리 하기 위한 코드를 작성 하는 방법의 예가 포함 되어 및 API 수준을 만들거나 Android 라이브러리를 사용 하는 경우를 관리 하는 방법을 설명 제공. API 수준 (예: Android 4.4) Android 버전 번호, Android 버전 이름 (예: Kitkat) 및 Xamarin.Android 빌드 버전 코드와 관련 된 포괄적인 목록을 제공 합니다.


## <a name="related-links"></a>관련 링크

- [Android SDK 설정](~/android/get-started/installation/android-sdk.md)
- [SDK CLI 도구 변경](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [고 compileSdkVersion, minSdkVersion, targetSdkVersion 선택](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd)
- [API 수준 이란?](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- [코드, 태그 및 빌드 번호](https://source.android.com/source/build-numbers)
