---
title: Android API 수준 이해
description: Xamarin.ios에는 여러 버전의 Android와 앱의 호환성을 결정 하는 몇 가지 Android API 수준 설정이 있습니다. 이 가이드에서는 이러한 설정의 의미, 구성 방법 및 런타임에 응용 프로그램에 미치는 영향에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 58CB7B34-3140-4BEB-BE2E-209928C1878C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/21/2018
ms.openlocfilehash: 188cc5b2ffba4540766edf0403d3a55228a21d6d
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84568986"
---
# <a name="understanding-android-api-levels"></a>Android API 수준 이해

_Xamarin.ios에는 여러 버전의 Android와 앱의 호환성을 결정 하는 몇 가지 Android API 수준 설정이 있습니다. 이 가이드에서는 이러한 설정의 의미, 구성 방법 및 런타임에 응용 프로그램에 미치는 영향에 대해 설명 합니다._

## <a name="quick-start"></a>빠른 시작

Xamarin Android는 다음과 같은 세 가지 Android API 수준 프로젝트 설정을 노출 합니다.

- [대상 프레임 워크](#framework) &ndash; 응용 프로그램을 빌드하는 데 사용할 프레임 워크를 지정 합니다. 이 API 수준은 *컴파일* 시 Xamarin. Android에서 사용 됩니다.

- [최소 Android 버전](#minimum) &ndash; 앱에서 지원 하려는 가장 오래 된 Android 버전을 지정 합니다. 이 API 수준은 Android에서 *런타임에* 사용 됩니다.

- [대상 Android 버전](#target) &ndash; 앱이 실행 되는 Android 버전을 지정 합니다. 이 API 수준은 Android에서 *런타임에* 사용 됩니다.

프로젝트에 대 한 API 수준을 구성 하려면 먼저 해당 API 수준에 대 한 SDK 플랫폼 구성 요소를 설치 해야 합니다. Android SDK 구성 요소를 다운로드 하 고 설치 하는 방법에 대 한 자세한 내용은 [Android SDK 설치](~/android/get-started/installation/android-sdk.md)를 참조 하세요.

> [!NOTE]
> 2018 년 8 월부터 Google Play 콘솔에 새 앱의 대상 API 수준 26 (Android 8.0) 이상이 필요 합니다.
기존 앱은 11 월 2018부터 API 레벨 26 이상을 대상으로 해야 합니다. 자세한 내용은 [Google Play 년 동안 앱 보안 및 성능 향상](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html)을 참조 하세요.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

일반적으로 세 가지 Xamarin Android API 수준은 모두 동일한 값으로 설정 됩니다. **응용 프로그램** 페이지에서 **Android 버전 (대상 프레임 워크)을 사용 하 여 컴파일을** 안정적인 최신 API 버전으로 설정 하거나 최소한 필요한 모든 기능이 포함 된 android 버전으로 설정 합니다.
다음 스크린샷에서 대상 프레임 워크는 **Android 7.1 (API 수준 25-Nougat)** 로 설정 됩니다.

[![Android 버전을 사용 하 여 컴파일하기 위한 대상 프레임 워크 버전 기본값](android-api-levels-images/vs-defaults-sml.png)](android-api-levels-images/vs-defaults.png#lightbox)

**Android 매니페스트** 페이지에서 **SDK 버전을 사용 하 여 컴파일을 사용** 하도록 최소 android 버전을 설정 하 고 대상 Android 버전을 대상 프레임 워크 버전과 동일한 값으로 설정 합니다 (다음 스크린샷에서 대상 android Framework는 **android 7.1 (Nougat)** 로 설정 됨).

[![대상 프레임 워크 버전으로 설정 되는 최소 및 대상 Android 버전](android-api-levels-images/vs-manifest-defaults-sml.png)](android-api-levels-images/vs-manifest-defaults.png#lightbox)

이전 버전의 Android와 이전 버전과의 호환성을 유지 하려면 앱에서 지원할 가장 오래 된 Android 버전을 **대상으로 하도록 최소 android 버전** 을 설정 합니다. (API 수준 14는 [Google Play services 및 Firebase 지원](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html)에 필요한 최소 api 수준입니다.) 다음 예제 구성에서는 API 수준 14부터 API 수준 25 까지의 Android 버전을 지원 합니다.

[![API 수준 25 Nougat를 사용 하 여 컴파일, 최소 Android 버전을 API 수준 14로 설정](android-api-levels-images/vs-minimum-sml.png)](android-api-levels-images/vs-minimum.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

일반적으로 세 가지 Xamarin Android API 수준은 모두 동일한 값으로 설정 됩니다. **대상 프레임 워크** 를 안정적인 최신 API 버전으로 설정 하거나 최소한 필요한 기능을 모두 포함 하는 Android 버전으로 설정 합니다. **대상 프레임 워크**를 설정 하려면 **프로젝트 옵션**에서 **빌드 > 일반** 으로 이동 합니다. 다음 스크린샷에는 대상 프레임 워크가 **최신 설치 된 플랫폼 (8.0)을 사용**하도록 설정 되어 있습니다.

[![대상 프레임 워크는 기본적으로 설치 된 최신 플랫폼을 사용 합니다.](android-api-levels-images/xs-default-target-sml.png)](android-api-levels-images/xs-default-target.png#lightbox)

최소 및 대상 Android 버전 설정은 **프로젝트 옵션**의 **> Android 응용 프로그램 빌드** 에서 찾을 수 있습니다. 최소 Android 버전을 **자동 사용 대상 프레임 워크 버전** 으로 설정 하 고 대상 android 버전을 대상 프레임 워크 버전과 동일한 값으로 설정 합니다. 다음 스크린샷에서 대상 Android Framework는 위의 대상 프레임 워크 설정과 일치 하도록 **android 8.0 (API 수준 26)** 로 설정 됩니다.

[![프로젝트 옵션에서 대상 및 프레임 워크 수준 설정](android-api-levels-images/xs-default-app-sml.png)](android-api-levels-images/xs-default-app.png#lightbox)

이전 버전의 Android와 이전 버전과의 호환성을 유지 하려는 경우, 앱에서 지원할 가장 오래 된 Android 버전으로 **최소 android 버전** 을 변경 합니다. API 수준 14는 [Google Play services 및 Firebase 지원](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html)에 필요한 최소 api 수준입니다.
예를 들어 다음 구성은 API 수준 14의 초기에 Android 버전을 지원 합니다.

[![최소 및 대상 버전이 자동 사용 대상 프레임 워크 버전으로 설정 되어 있습니다.](android-api-levels-images/xs-minimum-sml.png)](android-api-levels-images/xs-minimum.png#lightbox)

-----

앱이 여러 Android 버전을 지 원하는 경우 코드에 런타임 검사를 포함 하 여 앱이 최소 Android 버전 설정으로 작동 하는지 확인 해야 합니다 (자세한 내용은 아래의 [Android 버전에 대 한 런타임 검사](#runtimechecks) 참조). 라이브러리를 사용 하거나 만드는 경우 라이브러리에 대 한 API 수준 설정 구성의 모범 사례는 아래의 [API 수준 및 라이브러리](#libraries) 를 참조 하세요.

## <a name="android-versions-and-api-levels"></a>Android 버전 및 API 수준

Android 플랫폼이 진화 하 고 새로운 Android 버전이 출시 됨에 따라 각 Android 버전에는 *API 수준*이라는 고유한 정수 식별자가 할당 됩니다. 따라서 각 Android 버전은 단일 Android API 수준에 해당 합니다. 사용자가 최신 버전의 Android 뿐만 아니라 이전 버전의 앱을 설치 하기 때문에, 실제 Android 앱은 여러 Android API 수준에서 작동 하도록 설계 되어야 합니다.

### <a name="android-versions"></a>Android 버전

Android의 각 릴리스는 여러 이름으로 이동 합니다.

- Android 버전 (예: **android 9.0** )
- _원형_ 같은 코드 (또는 후 식) 이름
- **Api 수준 28** 과 같은 해당 api 수준

Android 코드 이름은 다음 표에 표시 된 것 처럼 여러 버전 및 API 수준에 해당할 수 있지만 각 Android 버전은 정확히 하나의 API 수준에 해당 합니다.

또한 Xamarin.ios는 현재 알려진 Android API 수준에 매핑되는 *빌드 버전 코드* 를 정의 합니다. 다음 표를 참조 하 여 API 수준, Android 버전, 코드 이름, Xamarin. Android 빌드 버전 코드 (빌드 버전 코드는 네임 스페이스에 정의 됨)를 변환할 수 있습니다. `Android.OS`

[!include[](~/android/includes/api-levels.md)]

이 표에 나와 있는 것 처럼 새 Android 버전은 &ndash; 종종 연간 릴리스를 두 개 이상 릴리스 합니다. 따라서 앱을 실행할 수 있는 Android 장치에는 다양 한 이전 버전 및 최신 Android 버전이 포함 됩니다. 앱이 여러 다른 버전의 Android에서 일관 되 고 안정적으로 실행 되도록 보장 하려면 어떻게 해야 하나요? Android의 API 수준은이 문제를 관리 하는 데 도움이 될 수 있습니다.

### <a name="android-api-levels"></a>Android API 수준

각 Android 장치는 정확히 *하나의* api 수준에서 실행 됩니다 &ndash; .이 api 수준은 Android 플랫폼 버전에 따라 고유 하 게 보장 됩니다. API 수준은 앱이 호출할 수 있는 API 집합의 버전을 정확 하 게 식별 합니다. 개발자로 코딩 하는 매니페스트 요소, 권한 등의 조합을 식별 합니다. Android의 API 수준 시스템은 장치에 응용 프로그램을 설치 하기 전에 android에서 응용 프로그램이 Android 시스템 이미지와 호환 되는지 확인 하는 데 도움이 됩니다.

응용 프로그램을 빌드할 때 다음과 같은 API 수준 정보가 포함 됩니다.

- 앱이 실행 되도록 빌드된 Android의 *대상* API 수준입니다.

- Android 장치에서 앱을 실행 하는 데 필요한 *최소* android API 수준입니다. 

이러한 설정은 설치 시 Android 장치에서 앱을 올바르게 실행 하는 데 필요한 기능을 사용할 수 있는지 확인 하는 데 사용 됩니다. 그렇지 않은 경우 해당 장치에서 앱이 실행 되지 않도록 차단 됩니다. 예를 들어 Android 장치의 API 수준이 앱에 대해 지정한 최소 API 수준 보다 낮으면 Android 장치에서 사용자가 앱을 설치 하지 못하게 됩니다.

## <a name="project-api-level-settings"></a>프로젝트 API 수준 설정

다음 섹션에서는 SDK Manager를 사용 하 여 대상으로 지정할 API 수준에 대 한 개발 환경을 준비 하는 방법을 설명 하 고, Xamarin.ios에서 *대상 프레임 워크*, *최소 Android 버전*및 *대상 android 버전* 설정을 구성 하는 방법에 대 한 자세한 설명을 설명 합니다.

### <a name="android-sdk-platforms"></a>Android SDK 플랫폼

Xamarin.ios에서 대상 또는 최소 API 수준을 선택 하려면 먼저 해당 API 수준에 해당 하는 Android SDK 플랫폼 버전을 설치 해야 합니다. 대상 프레임 워크, 최소 Android 버전 및 대상 Android 버전에 대해 사용할 수 있는 옵션의 범위는 설치한 Android SDK 버전의 범위로 제한 됩니다. SDK Manager를 사용 하 여 필요한 Android SDK 버전이 설치 되어 있는지 확인 하 고이를 사용 하 여 앱에 필요한 새 API 수준을 추가할 수 있습니다. API 수준을 설치 하는 방법에 익숙하지 않은 경우 [Android SDK 설정](~/android/get-started/installation/android-sdk.md)을 참조 하세요.

<a name="framework"></a>

### <a name="target-framework"></a>대상 프레임워크

*대상 프레임 워크* (라고도 함 `compileSdkVersion` )는 빌드할 때 앱이 컴파일되는 특정 Android FRAMEWORK 버전 (API 수준)입니다. 이 설정은 앱을 실행할 때 앱에서 사용 해야 하는 Api를 지정 하지만, 앱이 설치 될 *때 앱에서* 실제로 사용할 수 있는 api에는 영향을 주지 않습니다. 따라서 대상 프레임 워크 설정을 변경 해도 런타임 동작은 변경 되지 않습니다.

대상 프레임 워크는 응용 프로그램이 연결 된 라이브러리 버전을 식별 하 여 &ndash; 앱에서 사용할 수 있는 api를 결정 합니다. 예를 들어 Android 5.0 롤리팝에서 도입 된 [Notificationbuilder. SetCategory](xref:Android.App.Notification.Builder.SetCategory*) 메서드를 사용 하려면 대상 프레임 워크를 **API 수준 21 (롤리팝)** 이상으로 설정 해야 합니다. 프로젝트의 대상 프레임 워크를 **Api 수준 19 (KitKat)** 와 같은 api 수준으로 설정 하 고 코드에서 메서드를 호출 하려고 하면 `SetCategory` 컴파일 오류가 발생 합니다.

항상 사용 가능한 *최신* 대상 프레임 워크 버전으로 컴파일하는 것이 좋습니다. 이렇게 하면 코드에서 호출할 수 있는 더 이상 사용 되지 않는 Api에 대 한 유용한 경고 메시지를 제공 합니다. 최신 지원 라이브러리 릴리스를 사용 하는 경우 최신 대상 프레임 워크 버전을 사용 하는 것이 특히 중요 &ndash; 합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio에서 대상 프레임 워크 설정에 액세스 하려면 **솔루션 탐색기** 에서 프로젝트 속성을 열고 **응용 프로그램** 페이지를 선택 합니다.

[![프로젝트 속성의 응용 프로그램 페이지](android-api-levels-images/vs-target-framework-sml.png)](android-api-levels-images/vs-target-framework.png#lightbox)

위에 표시 된 것 처럼 **Android 버전을 사용 하 여 컴파일** 아래의 드롭다운 메뉴에서 API 수준을 선택 하 여 대상 프레임 워크를 설정 합니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

Mac용 Visual Studio에서 대상 프레임 워크 설정에 액세스 하려면 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **옵션**을 선택 합니다. **프로젝트 옵션** 대화 상자가 열립니다. 이 대화 상자에서 다음과 같이 **빌드 > 일반** 으로 이동 합니다.

[![프로젝트 옵션 페이지의 일반 빌드 섹션](android-api-levels-images/xs-target-framework-sml.png)](android-api-levels-images/xs-target-framework.png#lightbox)

위에 표시 된 것 처럼 **대상 프레임 워크** 의 오른쪽에 있는 드롭다운 메뉴에서 API 수준을 선택 하 여 대상 프레임 워크를 설정 합니다.

-----

<a name="minimum"></a>

### <a name="minimum-android-version"></a>최소 Android 버전

*최소 android 버전* (라고도 함)은 `minSdkVersion` 응용 프로그램을 설치 하 고 실행할 수 있는 가장 오래 된 버전의 android OS (즉, 가장 낮은 API 수준)입니다. 기본적으로 앱은 대상 프레임 워크 설정과 일치 하는 장치에만 설치할 수 있습니다. 최소 Android 버전 설정이 대상 프레임 워크 설정 보다 *낮으면* 앱이 이전 버전의 android에서 실행 될 수도 있습니다. 예를 들어 대상 프레임 워크를 **android 7.1 (Nougat)** 로 설정 하 고 최소 android 버전을 **Android 4.0.3 (Ice)** 로 설정 하는 경우 API 수준 15에서 api 수준 25 (포함)까지 모든 플랫폼에 앱을 설치할 수 있습니다.

앱이이 범위 내에서 성공적으로 빌드 및 설치 될 수 있지만 이러한 플랫폼 모두에서 성공적으로 *실행* 되는 것은 아닙니다. 예를 들어 앱이 **android 5.0 (롤리팝)** 에 설치 되어 있고 코드가 **Android 7.1 (Nougat)** 이상 에서만 사용할 수 있는 API를 호출 하는 경우 앱에서 런타임 오류가 발생 하 고 충돌이 발생할 수 있습니다. 따라서 코드는 &ndash; 런타임에 &ndash; 실행 중인 Android 장치에서 지 원하는 api만 호출 하는지 확인 해야 합니다. 즉, 앱에서 최신 Api를 지원 하기에 충분 한 장치에 대해서만 최신 Api를 사용 하도록 코드에 명시적 런타임 검사를 포함 해야 합니다.
이 가이드의 뒷부분에 나오는 [Android 버전에 대 한 런타임 검사](#runtimechecks)에서는 이러한 런타임 검사를 코드에 추가 하는 방법을 설명 합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio의 최소 Android 버전 설정에 액세스 하려면 **솔루션 탐색기** 에서 프로젝트 속성을 열고 **Android 매니페스트** 페이지를 선택 합니다. **최소 android 버전** 아래의 드롭다운 메뉴에서 응용 프로그램에 대 한 최소 android 버전을 선택할 수 있습니다.

[![SDK 버전을 사용 하 여 컴파일하도록 최소 Android 대상 옵션 설정](android-api-levels-images/vs-minimum-version-sml.png)](android-api-levels-images/vs-minimum-version.png#lightbox)

**SDK 버전을 사용 하 여 컴파일 사용**을 선택 하는 경우 최소 Android 버전은 대상 프레임 워크 설정과 동일 합니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

Mac용 Visual Studio에서 최소 Android 버전에 액세스 하려면 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **옵션**을 선택 합니다. **프로젝트 옵션** 대화 상자가 열립니다. **빌드 > Android 응용 프로그램**으로 이동 합니다.
**최소 android 버전**의 오른쪽에 있는 드롭다운 메뉴를 사용 하 여 응용 프로그램에 대 한 최소 android 버전을 설정할 수 있습니다.

[![자동 사용 대상 프레임 워크 버전으로 설정 되는 최소 Android 버전](android-api-levels-images/xs-minimum-version-sml.png)](android-api-levels-images/xs-minimum-version.png#lightbox)

**자동 &ndash; 사용 대상 프레임 워크 버전**을 선택 하는 경우 최소 Android 버전은 대상 프레임 워크 설정과 동일 합니다.

-----

<a name="target"></a>

### <a name="target-android-version"></a>대상 Android 버전

*대상 Android 버전* (라고도 함)은 `targetSdkVersion` 앱이 실행 될 것으로 예상 하는 android 장치의 API 수준입니다. Android는이 설정을 사용 하 여 호환성 동작을 사용할지 여부를 결정 &ndash; 합니다. 이렇게 하면 앱이 계속 해 서 필요한 방식으로 작동 합니다. Android는 앱의 대상 Android 버전 설정을 사용 하 여 응용 프로그램에 적용 될 수 있는 동작 변경 내용 (Android에서 이전 버전과의 호환성을 제공 하는 방식)을 파악 합니다.

대상 프레임 워크와 대상 Android 버전은 매우 유사 하지만 동일한 이름이 아닙니다. 대상 Android 버전은 *컴파일 타임*에 사용 하기 위해 대상 api 수준 정보를 xamarin.ios에 전달 하 고, 대상 android 버전은 *런타임에* 사용 하기 위해 대상 Api 수준 정보를 android에 전달 합니다 (앱이 장치에 설치 되어 실행 되는 경우).

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio에서이 설정에 액세스 하려면 **솔루션 탐색기** 에서 프로젝트 속성을 열고 **Android 매니페스트** 페이지를 선택 합니다. **대상 android 버전** 아래의 드롭다운 메뉴에서 응용 프로그램에 대 한 대상 android 버전을 선택할 수 있습니다.

[![SDK 버전을 사용 하 여 컴파일하도록 설정 대상 Android 버전](android-api-levels-images/vs-target-version-sml.png)](android-api-levels-images/vs-target-version.png#lightbox)

앱을 테스트 하는 데 사용 하는 Android의 최신 버전으로 대상 Android 버전을 명시적으로 설정 하는 것이 좋습니다. 이상적으로는 최신 Android SDK 버전으로 설정 해야 합니다 &ndash; . 이렇게 하면 동작 변경 작업을 수행 하기 전에 새 api를 사용할 수 있습니다. 대부분의 개발자는 **SDK 버전을 사용 하 여 컴파일을 사용**하도록 대상 Android 버전을 설정 *하지* 않는 것이 좋습니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

Mac용 Visual Studio에서이 설정에 액세스 하려면 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **옵션**을 선택 합니다. **프로젝트 옵션** 대화 상자가 열립니다. **빌드 > Android 응용 프로그램**으로 이동 합니다. **대상 android 버전**의 오른쪽에 있는 드롭다운 메뉴를 사용 하 여 응용 프로그램에 대 한 대상 android 버전을 설정할 수 있습니다.

[![대상 Android 버전이 자동으로 사용 되는 대상 프레임 워크 버전으로 설정 되어 있습니다.](android-api-levels-images/xs-target-version-sml.png)](android-api-levels-images/xs-target-version.png#lightbox)

앱을 테스트 하는 데 사용 하는 Android의 최신 버전으로 대상 Android 버전을 명시적으로 설정 하는 것이 좋습니다. 이상적으로는 사용 가능한 최신 Android SDK 버전으로 설정 해야 합니다 &ndash; . 이렇게 하면 동작 변경 작업을 수행 하기 전에 새 api를 사용할 수 있습니다. 대부분의 개발자는 대상 Android 버전을 **자동 사용 대상 프레임 워크 버전**으로 설정 하지 않는 것이 좋습니다.

-----

일반적으로 대상 Android 버전은 최소 Android 버전 및 대상 프레임 워크로 제한 되어야 합니다. 구체적인 요건은 다음과 같습니다.

**최소 Android 버전 <= 대상 Android 버전 <= 대상 프레임 워크**

SDK 수준에 대 한 자세한 내용은 Android 개발자 [사용-sdk](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html) 설명서를 참조 하세요.

<a name="runtimechecks"></a>

## <a name="runtime-checks-for-android-versions"></a>Android 버전에 대 한 런타임 검사

Android의 새 버전이 릴리스되면 프레임 워크 API는 새로운 기능 또는 대체 기능을 제공 하도록 업데이트 됩니다. 몇 가지 예외를 제외 하 고 이전 Android 버전의 API 기능을 수정 하지 않고 최신 Android 버전으로 전달 합니다. 따라서 앱이 특정 Android API 수준에서 실행 되는 경우 일반적으로 이후 Android API 수준에서 수정 하지 않고 실행할 수 있습니다. 그러나 이전 버전의 Android에서 앱을 실행 하려는 경우는 어떻게 되나요?

대상 프레임 워크 설정 보다 *낮은* 최소 Android 버전을 선택 하는 경우 런타임에 응용 프로그램에서 일부 api를 사용 하지 못할 수 있습니다. 그러나 앱은 여전히 이전 장치에서 실행할 수 있지만 기능이 축소 됩니다. 최소 Android 버전 설정에 해당 하는 Android 플랫폼에서 사용할 수 없는 각 API에 대해 코드에서 속성 값을 명시적으로 확인 `Android.OS.Build.VERSION.SdkInt` 하 여 앱이 실행 되는 플랫폼의 API 수준을 결정 해야 합니다. API 수준이 호출 하려는 API를 지 원하는 최소 Android 버전 보다 *낮은* 경우 코드에서이 api를 호출 하지 않고 정상적으로 작동 하는 방법을 찾아야 합니다.

예를 들어, **android 5.0 롤리팝** (이상)에서 실행 될 때 [Notificationbuilder. setcategory](xref:Android.App.Notification.Builder.SetCategory*) 메서드를 사용 하 여 알림을 범주화 하지만 **android 4.1 Jelly Bean** 과 같은 이전 버전의 android에서 앱을 실행 하려는 경우 ( `SetCategory` 를 사용할 수 없는 경우) 이 가이드의 시작 부분에 있는 Android 버전 표를 참조 하 여 **android 5.0 롤리팝** 빌드 버전 코드는입니다 `Android.OS.BuildVersionCodes.Lollipop` . 을 사용할 수 없는 이전 버전의 Android를 지원 하기 위해 `SetCategory` 코드는 런타임에 api 수준을 검색 하 고 `SetCategory` Api 수준이 롤리팝 빌드 버전 코드 보다 크거나 같은 경우에만 조건적으로 호출할 수 있습니다.

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    builder.SetCategory(Notification.CategoryEmail);
}
```

이 예제에서는 앱의 대상 프레임 워크가 **android 5.0 (Api 수준 21)** 로 설정 되 고 최소 Android 버전이 **ANDROID 4.1 (api 수준 16)** 로 설정 됩니다. `SetCategory`는 api 수준 이상에서 사용할 수 있기 때문에 `Android.OS.BuildVersionCodes.Lollipop` 이 예제 코드는 `SetCategory` 실제로 사용할 수 있는 경우에만를 호출 합니다 .이 코드는 &ndash; *not* `SetCategory` api 수준이 16, 17, 18, 19 또는 20 인 경우 호출을 시도 하지 않습니다. 이러한 이전 Android 버전에서는 알림이 제대로 정렬 되지 않은 상태 (유형별로 분류 되지 않았기 때문)에 대해서만이 기능이 축소 되어 있지만 사용자에 게 경고를 보내도록 알림이 게시 됩니다. 앱은 계속 작동 하지만 해당 기능이 약간 감소 합니다.

일반적으로 빌드 버전 검사를 사용 하면 코드를 런타임에 결정 하는 데 도움이 됩니다. 예를 들면 다음과 같습니다.

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

하나 이상의 Api가 없는 이전 버전의 Android 버전에서 앱을 실행할 때 앱 기능을 줄이거나 수정 하는 방법을 설명 하는 빠르고 간단한 규칙이 없습니다. 일부 경우 ( `SetCategory` 위 예제에서) API 호출을 사용할 수 없는 경우에는이 호출을 생략 해도 충분 합니다. 그러나 `Android.OS.Build.VERSION.SdkInt` 응용 프로그램에서 최적의 환경을 제공 하는 데 필요한 API 수준 보다 작은 것으로 검색 되는 경우에 대 한 대체 기능을 구현 해야 하는 경우도 있습니다.

<a name="libraries"></a>

## <a name="api-levels-and-libraries"></a>API 수준 및 라이브러리

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Xamarin Android 라이브러리 프로젝트를 만드는 경우 (예: 클래스 라이브러리 또는 바인딩 라이브러리) 대상 프레임 워크 설정만 구성할 수 있습니다 &ndash; . 최소 android 버전 및 대상 Android 버전 설정은 사용할 수 없습니다. **Android 매니페스트** 페이지가 없기 때문입니다.

[![Android 버전을 사용한 컴파일 옵션만 사용할 수 있습니다.](android-api-levels-images/vs-library-options-sml.png)](android-api-levels-images/vs-library-options.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

Xamarin Android 라이브러리 프로젝트를 만들 때 최소 android 버전 및 대상 Android 버전을 구성할 수 있는 **Android 응용 프로그램** 페이지는 &ndash; 최소 Android 버전 및 대상 android 버전 설정을 사용할 수 없습니다.
이는 **빌드 > Android 응용 프로그램** 페이지가 없기 때문입니다.

[![최소 및 대상 버전 옵션을 사용 하지 않고 일반 페이지 빌드](android-api-levels-images/xs-library-options-sml.png)](android-api-levels-images/xs-library-options.png#lightbox)

-----

결과로 생성 되는 라이브러리가 독립 실행형 앱이 아니기 때문에 최소 Android 버전 및 대상 Android 버전 설정을 사용할 수 없습니다 &ndash; .이 라이브러리는 패키지 된 앱에 따라 android 버전에서 실행 될 수 있습니다. 라이브러리를 *컴파일하*는 방법을 지정할 수 있지만 라이브러리가 실행 될 플랫폼 API 수준을 예측할 수는 없습니다. 이 점을 염두에 두면 라이브러리를 사용 하거나 만들 때 다음과 같은 모범 사례를 준수 해야 합니다.

- **Android 라이브러리** &ndash; 를 사용 하는 경우 응용 프로그램에서 Android 라이브러리를 사용 하는 경우 응용 프로그램의 대상 프레임 워크 설정을 라이브러리의 대상 프레임 워크 설정 보다 *적어도 높은* API 수준으로 설정 해야 합니다.

- **Android 라이브러리** &ndash; 를 만드는 경우 다른 응용 프로그램에서 사용할 Android 라이브러리를 만드는 경우 대상 프레임 워크 설정을 컴파일하는 데 필요한 최소 API 수준으로 설정 해야 합니다.

이러한 모범 사례는 라이브러리에서 런타임에 사용할 수 없는 API를 호출 하려고 하는 상황을 방지 하는 데 도움이 됩니다 (응용 프로그램의 작동이 중단 될 수 있음). 라이브러리 개발자는 API 호출 사용을 전체 API 노출 영역의 작고 잘 구성 된 하위 집합으로 제한 하도록 노력 해야 합니다. 이렇게 하면 더 광범위 한 Android 버전에서 라이브러리를 안전 하 게 사용할 수 있습니다.

## <a name="summary"></a>요약

이 가이드에서는 Android API 수준을 사용 하 여 여러 버전의 Android에서 앱 호환성을 관리 하는 방법을 설명 했습니다. Xamarin Android *대상 프레임 워크*, *최소 Android 버전*및 *대상 android 버전* 프로젝트 설정을 구성 하는 자세한 단계를 제공 했습니다. Android SDK Manager를 사용 하 여 SDK 패키지를 설치 하는 방법에 대 한 지침을 제공 하 고, 런타임에 다른 API 수준을 처리 하는 코드를 작성 하는 방법에 대 한 예제를 제공 하며, Android 라이브러리를 만들거나 사용할 때 API 수준을 관리 하는 방법을 설명 했습니다. 또한 API 수준을 Android 버전 번호 (예: Android 4.4), Android 버전 이름 (예: Kitkat) 및 Xamarin Android 빌드 버전 코드와 관련 된 포괄적인 목록을 제공 합니다.

## <a name="related-links"></a>관련 링크

- [Android SDK 설정](~/android/get-started/installation/android-sdk.md)
- [SDK CLI 도구 변경 내용](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [CompileSdkVersion, 매니페스트의 minsdkversion 및 targetSdkVersion 선택](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd)
- [API 수준 이란?](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- [하기 위해 채택한, 태그 및 빌드 번호](https://source.android.com/source/build-numbers)
