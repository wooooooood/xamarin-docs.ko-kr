---
title: Android API 수준 이해
description: Xamarin.Android에 여러 버전의 Android 앱의 호환성을 결정 하는 몇 가지 Android API 수준 설정이 있습니다. 이러한 설정은 의미를 구성 하는 방법 및 영향을 잘 모르겠으면이 가이드에서는 설명 앱 실행 시가지고 있습니다.
ms.prod: xamarin
ms.assetid: 58CB7B34-3140-4BEB-BE2E-209928C1878C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 8f284fefd260764c6f09d78d2518bfd115782cd2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="understanding-android-api-levels"></a>Android API 수준 이해

_Xamarin.Android에 여러 버전의 Android 앱의 호환성을 결정 하는 몇 가지 Android API 수준 설정이 있습니다. 이러한 설정은 의미를 구성 하는 방법 및 영향을 잘 모르겠으면이 가이드에서는 설명 앱 실행 시가지고 있습니다._


## <a name="quick-start"></a>빠른 시작

Xamarin.Android 세 Android API 수준 프로젝트 설정을 노출합니다.

-   [대상 프레임 워크](#framework) &ndash; 응용 프로그램을 만드는 데 사용할는 프레임 워크를 지정 합니다. 이 API 수준이에 사용 되는 *컴파일* Xamarin.Android 하 여 시간입니다.

-   [최소 Android 버전](#minimum) &ndash; 지원 하도록 앱을 사용 하지 않겠다고 가장 오래 된 Android 버전을 지정 합니다. 이 API 수준이에 사용 되는 *실행* Android에서 시간입니다.

-   [Android 버전을 대상](#target) &ndash; 에서 실행 하기 위해 앱이 Android 버전을 지정 합니다. 이 API 수준이에 사용 되는 *실행* Android에서 시간입니다.

프로젝트에 대 한 API 레벨을 구성할 수 있습니다, 전에 해당 API 수준에 대 한 SDK 플랫폼 구성 요소를 설치 해야 합니다. 다운로드 하 고 Android SDK 구성 요소를 설치 하는 방법에 대 한 자세한 내용은 참조 [Android SDK 설치](~/android/get-started/installation/android-sdk.md)합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

일반적으로 세 Xamarin.Android API 수준 모두 동일한 값으로 설정 됩니다. 에 **응용 프로그램** 페이지에서 설정 **Android 버전 (대상 프레임 워크)를 사용 하 여 컴파일** 를 안정적인 최신 API 버전 (또는 여기에 Android 버전의 모든 필요한 기능을 하는 최소한).
대상 프레임 워크로 설정 되어 다음 스크린 샷에 **Android 7.1 (API 수준 25-Nougat)**:

[![대상 프레임 워크 버전 기본값으로 Android 버전을 사용 하 여 컴파일](android-api-levels-images/vs-defaults-sml.png)](android-api-levels-images/vs-defaults.png#lightbox)

에 **Android 매니페스트** 페이지에서 최소 Android 버전을 설정 **사용 하 여 SDK 버전을 사용 하 여 컴파일** Android 대상 버전 (다음에에서 대상 프레임 워크 버전으로 동일한 값을 설정 하 고 스크린 샷, Android 대상 프레임 워크로 설정 된 **Android 7.1 (Nougat)**):

[![대상 프레임 워크 버전으로 최소 및 대상 Android 버전 설정](android-api-levels-images/vs-manifest-defaults-sml.png)](android-api-levels-images/vs-manifest-defaults.png#lightbox)

이전 버전의 Android와 이전 버전과 호환성을 유지 하려면 설정 **최소 Android 버전을 대상** 가장 오래 된 버전의 Android 앱이 지원 되도록 합니다. (API 수준 14에 필요한 최소 API 수준은 [하려면 Google Play 서비스 및 Firebase 지원](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).) 다음 예제에서는 구성을 통해 API 수준 25 API 수준 14에서 Android 버전을 지원합니다.

[![API 수준 25를 사용 하 여 컴파일 Nougat, API 레벨 14로 설정 하는 최소 Android 버전](android-api-levels-images/vs-minimum-sml.png)](android-api-levels-images/vs-minimum.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

일반적으로 세 Xamarin.Android API 수준 모두 동일한 값으로 설정 됩니다. 설정 **대상 프레임 워크** 를 안정적인 최신 API 버전 (또는 여기에 Android 버전의 모든 필요한 기능을 하는 최소한). 설정 하는 **대상 프레임 워크**로 이동 **빌드 > 일반** 에 **프로젝트 옵션**합니다. 대상 프레임 워크로 설정 되어 다음 스크린 샷에 **최신 설치 된 플랫폼 (8.0)를 사용 하 여**:

[![대상 프레임 워크를 사용 하 여 최신 설치 플랫폼 기본값으로 설정](android-api-levels-images/xs-default-target-sml.png)](android-api-levels-images/xs-default-target.png#lightbox)

최소 및 대상 Android 버전 설정을 확인할 수 있습니다 **빌드 > Android 응용 프로그램** 에 **프로젝트 옵션**합니다. 최소 Android 버전을 설정 **자동으로 사용 하 여 대상 프레임 워크 버전** 대상 프레임 워크 버전으로 동일한 값으로 대상 Android 버전을 설정 합니다. 다음 스크린 샷에서 Android 대상 프레임 워크로 설정 되어 **Android 8.0 (API 수준 26)** 위의 대상 프레임 워크 설정과 일치 시킵니다.

[![프로젝트 옵션에서 대상 및 프레임 워크 수준 설정](android-api-levels-images/xs-default-app-sml.png)](android-api-levels-images/xs-default-app.png#lightbox)

이전 버전의 Android와 이전 버전과 호환성을 유지 하려면 변경 **최소 Android 버전** 가장 오래 된 버전의 Android 앱이 지원 되도록 합니다. API 수준 14에 필요한 최소 API 수준은 [하려면 Google Play 서비스 및 Firebase 지원](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html)합니다.
예를 들어 다음 구성은 초기 API 수준 14에 Android 버전이 지원 됩니다.

[![-자동으로 설정 하는 대상 버전 및 최소 대상 프레임 워크 버전을 사용 하 여](android-api-levels-images/xs-minimum-sml.png)](android-api-levels-images/xs-minimum.png#lightbox)

-----


코드 최소 Android 버전 설정으로 앱이 작동 하도록 하려면 런타임 검사를 포함 해야 앱이 여러 Android 버전을 지 원하는 경우 (참조 [Android 버전에 대 한 런타임 검사](#runtimechecks) 아래의 세부 정보에 대 한). 사용 하거나 라이브러리를 만드는 하는 경우 참조 [API 수준 및 라이브러리](#libraries) 아래 API 구성에서 모범 사례에 대 한 수준 라이브러리에 대 한 설정입니다.



## <a name="android-versions-and-api-levels"></a>Android 버전 및 API 레벨

각 Android 버전 라는 고유 정수 식별자는 할당 된 Android 플랫폼 진화 함에 따라와 새 Android 버전이 출시 되는 *API 레벨*합니다. 따라서 각 Android 버전 단일 Android API 수준에 해당합니다. 사용자가 이전도 가장 최근 버전의 Android에서 앱을 설치 하기 때문에 실제 Android 응용 프로그램을 여러 Android API 수준에 맞게 디자인 되어야 합니다.


### <a name="android-versions"></a>Android 버전

Android의 각 릴리스 여러 이름으로 진행 됩니다.

-   Android 버전와 같은 **Android 7.1**
-   같은 이름, A을 코딩할 _Nougat_
-   해당 API와 같은 수준 **25 API 레벨**

여러 버전 및 API 수준 (처럼 아래 목록에서)에 Android 코드명 해당할 수 있습니다 하지만 정확히 하나의 API 수준에 해당 하는 각 Android 버전.

또한 Xamarin.Android 정의 *빌드 버전 코드가* 현재 알려진된 Android API 수준에 매핑되는 합니다. 다음 목록에는 API 수준, Android 버전, 코드 이름 및 Xamarin.Android 빌드 버전 코드 사이 변환 하면 유용 합니다.

-   **API 27 (Android 8.1)** &ndash; _Oreo_2017 년 12 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.OMr1`

-   **API 26 (Android 8.0)** &ndash; _Oreo_2017 년 8 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.O`

-   **API 25 (Android 7.1)** &ndash; _Nougat_2016 년 12 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.NMr1`

-   **API 24 (Android 7.0)** &ndash; _Nougat_2016 년 8 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.N`

-   **API 23 (Android 6.0)** &ndash; _Marshmallow_2015 년 8 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.M`

-   **API 22 (Android 5.1)** &ndash; _롤리팝_2015 년 3 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.LollipopMr1`

-   **API 21 (Android 5.0)** &ndash; _롤리팝_2014 년 11 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.Lollipop`

-   **API 20 (Android 4.4W)** &ndash; _Kitkat 조사식_2014 년 6 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.KitKatWatch`

-   **API 19 (Android 4.4)** &ndash; _Kitkat_2013 년 10 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.KitKat`

-   **API 18 (Android 4.3)** &ndash; _젤리 Bean_2013 년 7 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.JellyBeanMr2`

-   **API 17 (Android 4.2 4.2.2)** &ndash; _젤리 Bean_2012 년 11 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.JellyBeanMr1`

-   **API 16 (Android 4.1-4.1.1)** &ndash; _젤리 Bean_2012 년 6 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.JellyBean`

-   **API 15 (Android 4.0.3-4.0.4)** &ndash; _아이스크림 샌드위치_2011 년 12 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.IceCreamSandwichMr1`

-   **API 14 (Android 4.0 4.0.2)** &ndash; _아이스크림 샌드위치_2011 년 10 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.IceCreamSandwich`

-   **(Android 3.2) API 13** &ndash; _Honeycomb_2011 년 6 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.HoneyCombMr2`

-   **API 12 (Android 3.1.x)** &ndash; _Honeycomb_2011 년 5 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.HoneyCombMr1`

-   **API 11 (Android 3.0. x)** &ndash; _Honeycomb_2011 년 2 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.HoneyComb`

-   **API 10 (Android 2.3.3-2.3.4)** &ndash; _인형_2011 년 2 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.GingerBreadMr1`

-   **API 9 (Android 2.3 2.3.2)** &ndash; _인형_2010 년 11 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.GingerBread`

-   **API 8 (Android 2.2. x)** &ndash; _Froyo_2010 년 6 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.Froyo`

-   **API 7 (Android 2.1. x)** &ndash; _Eclair_2010 년 1 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.EclairMr1`

-   **API 6 (Android 2.0.1)** &ndash; _Eclair_2009 12 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.Eclair01`

-   **API 5 (Android 2.0)** &ndash; _Eclair_2009 년 11 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.Eclair`

-   **API 4 (Android 1.6)** &ndash; _도넛_2009 년 9 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.Donut`

-   **API 3 (Android 1.5)** &ndash; _Cupcake_2009 년 5 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.Cupcake`

-   **API 2 (Android 1.1)** &ndash; _자료_2009 년 2 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.Base11`

-   **API 1 (Android 1.0)** &ndash; _자료_2008 년 10 월 릴리스. 버전 코드 빌드 `Android.OS.BuildVersionCodes.Base`


새 Android 버전 자주 릴리스될이 목록에 표시 대로 &ndash; 때로는 여러 릴리스 1 년입니다. 결과적으로, 다양 한 Android 버전을 이전 버전과 새 버전의 앱이 실행 될 수 있는 Android 장치 universe 포함 되어 있습니다. 앱에서 실행 됩니다 일관 되 고 안정적으로 많은 다른 버전의 Android 어떻게 보장할 수 있습니까? Android의 API 수준은이 문제를 관리할 수 있습니다.


### <a name="android-api-levels"></a>Android API 수준

각 Android 장치에 정확 하 게 실행 *하나의* API 수준 &ndash; 이 API 레벨 Android 플랫폼 버전 별로 고유 하 게 보장 됩니다. API 수준 앱;를 호출할 수 있는 API 집합의 버전을 정확 하 게 식별 개발자는 매니페스트 요소, 사용 권한에 대해 코드 해당 등의 조합을 식별 합니다. API 수준 android의 시스템을 사용 하면 Android 응용 프로그램을 장치에 응용 프로그램을 설치 하기 전에 Android 시스템 이미지와 호환 되는지 확인 합니다.

응용 프로그램을 작성할 때 다음과 같은 API 수준 정보가 포함 되어 있습니다.

-   *대상* 응용 프로그램에서 실행 하는 기본 제공 되는 android API 수준입니다.

-   *최소* Android 장치 해야 앱을 실행 해야 하는 Android API 수준입니다. 

이러한 설정은 응용 프로그램을 올바르게 실행 하는 데 필요한 기능을 설치 시에는 Android 장치에서 사용할 수 있도록 하는 데 사용 됩니다. 그렇지 않은 경우 응용 프로그램은 해당 장치에서 실행에서 차단 됩니다. 예를 들어, Android 장치 API 수준의 응용 프로그램에 대해 지정 하는 최소 API 수준 보다 낮은 경우 Android 장치는 설치 응용 프로그램에서 사용자를 하지 것입니다.


## <a name="project-api-level-settings"></a>프로젝트 API 수준 설정

다음 섹션으로 구성 하는 방법에 대 한 세부 정보를 대상으로 지정할 API 레벨을 위한 개발 환경을 준비 하려면 SDK 관리자를 사용 하는 방법을 설명 *대상 프레임 워크*, *최소 Android 버전*, 및 *대상 Android 버전* Xamarin.Android에서 설정 합니다.


### <a name="android-sdk-platforms"></a>Android SDK 플랫폼

대상 또는 최소 API 수준 Xamarin.Android에서를 선택 하려면 먼저 해당 API 레벨 해당 하는 Android SDK 플랫폼 버전을 설치 해야 합니다. 대상 프레임 워크, 최소 Android 버전 및 대상 Android 버전에 대 한 사용 가능한 선택 항목의 범위는 설치 된 Android SDK의 범위 버전으로 제한 합니다. 필요한 Android SDK 버전을 설치할 되었고 응용 프로그램에 필요한 모든 새 API 수준을 추가 하는 데 사용할 수 있는지 확인 하려면 SDK Manager를 사용할 수 있습니다. API 수준을 설치 하는 방법을 모르는 경우 참조 [Android SDK 설치](~/android/get-started/installation/android-sdk.md)합니다.

<a name="framework" />

### <a name="target-framework"></a>대상 프레임워크

*대상 프레임 워크* (라고도 `compileSdkVersion`)은 응용 프로그램에 대 한 빌드 시 컴파일된 Android 특정 프레임 워크 버전 (API 수준). 이 설정은 지정 어떤 Api 앱 *예상* 실행 될 때 영향을 주지 않습니다에 Api는 실제로 앱에서 사용 가능한 설치 될 때 사용 하도록 합니다. 결과적으로, 대상 프레임 워크 설정을 변경 해도 런타임 동작이 변경 되지 않습니다.

응용 프로그램에 대해 연결 된 라이브러리 버전을 식별 하는 대상 프레임 워크 &ndash; 응용 프로그램에서 사용할 수 있는 Api를 결정 합니다. 예를 들어, 사용 하려는 경우는 [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) Android 5.0 롤리팝에 도입 된 메서드를 대상 프레임 워크를 설정 해야 **API 수준 21 (롤리팝)** 이상. 프로젝트의 대상 프레임 워크 api와 같은 수준으로 설정 하면 **API 수준 19 (KitKat)** 호출 하려고 하 고는 `SetCategory` 코드에서 메서드를 컴파일 오류가 발생 합니다.

사용 하 여 항상 컴파일하는 것이 좋습니다는 *최신* 대상 프레임 워크 버전을 사용할 수 있습니다. 이렇게 하면 코드에서 호출할 수 있는 사용 되지 않는 Api에 대 한 유용한 경고 메시지와 함께 제공 합니다. 최신 대상 프레임 워크 버전을 사용 하는 최신 지원 라이브러리 릴리스를 사용 하는 경우 특히 중요 &ndash; 각 라이브러리에서는 해당 지원 라이브러리의 최소 API 수준에서 컴파일된 클 수 있는 응용 프로그램입니다. 

> [!NOTE]
> 2018 년 8 월부터, Google 재생 콘솔을 새 응용 프로그램 대상 API 레벨 26 (Android 8.0) 해야 합니다 또는 이상.
기존 응용 프로그램 API 수준 26 또는 2018 년 11 월에서에서 시작 하는 더 높은 대상으로 해야 합니다. 자세한 내용은 참조 [앱 보안 및 년에 대 한 상태가 될 때까지 Google Play에서 성능 향상](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html)합니다.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio에서 대상 프레임 워크 설정에 액세스 하려면 프로젝트 속성에서 열고 **솔루션 탐색기** 선택 하 고는 **응용 프로그램** 페이지:

[![프로젝트 속성의 응용 프로그램 페이지](android-api-levels-images/vs-target-framework-sml.png)](android-api-levels-images/vs-target-framework.png#lightbox)

API 수준 아래에 있는 드롭 다운 메뉴에서 선택 하 여 대상 프레임 워크를 설정 **Android 버전을 사용 하 여 컴파일** 위와 같이 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac에 대 한 Visual Studio에서 대상 프레임 워크 설정에 액세스 하려면 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **옵션**;이 열립니다는 **프로젝트 옵션** 대화 상자. 이 대화 상자에서로 이동 **빌드 > 일반** 다음과 같이 합니다.

[![프로젝트 옵션 페이지의 일반 섹션을 빌드](android-api-levels-images/xs-target-framework-sml.png)](android-api-levels-images/xs-target-framework.png#lightbox)

오른쪽의 드롭다운 메뉴에서 API 레벨을 선택 하 여 대상 프레임 워크를 설정 **대상 프레임 워크** 위와 같이 합니다.

-----


<a name="minimum" />

### <a name="minimum-android-version"></a>최소 Android 버전

*최소 Android 버전* (라고도 `minSdkVersion`)를 설치 하 고 응용 프로그램을 실행할 수 있는 Android OS (즉, 가장 낮은 API 수준)의 가장 오래 된 버전입니다. 기본적으로 응용 프로그램 또는 가능 대상 프레임 워크와 일치 하는 장치에 설치 되어 더 높은; 최소 Android 버전 설정이 *낮은* 대상 프레임 워크 설정 보다 앱 이전 버전의 Android에서 실행할 수도 있습니다. 예를 들어, 대상 프레임 워크를 설정 하면 **Android 7.1 (Nougat)** 최소 Android 버전을 설정 하 고 **Android 4.0.3 (아이스크림 샌드위치)**, API 수준 15에서에서 모든 플랫폼에서 앱을 설치할 수 있습니다 API 수준 25 까지입니다.

앱 성공적으로 빌드 하 고이 다양 한 플랫폼에 설치할 수 있지만 이렇게 해도 성공적으로 됩니다 *실행* 모든 이러한 플랫폼에 있습니다. 예를 들어, 앱에 설치 된 경우 **Android 5.0 (롤리팝)** 코드 에서만 사용할 수 있는 API를 호출 하 고 **Android 7.1 (Nougat)** 이상 버전에서는 응용 프로그램 런타임 오류가 표시 되며 작동이 중단 되었을 수 있습니다. 따라서 코드 확인 해야 &ndash; 런타임에 &ndash; 에서 실행 되는 Android 장치에서 지원 되는 Api만을 호출 하는 것입니다. 즉, 코드를 응용 프로그램은 원하는 수 있을 만큼 최신 장치에만 새 Api를 사용 하도록 명시적 런타임 검사를 포함 해야 합니다.
[Android 버전에 대 한 런타임 검사](#runtimechecks)이 가이드의 뒷부분에 나오는 이러한 런타임 검사 코드를 추가 하는 방법에 설명 합니다.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio에서 최소 Android 버전 설정에 액세스 하려면 프로젝트 속성에서 열고 **솔루션 탐색기** 선택 하 고는 **Android 매니페스트** 페이지. 아래에 있는 드롭 다운 메뉴에서 **최소 Android 버전** 응용 프로그램에 대 한 최소 Android 버전을 선택할 수 있습니다.

[![최소 Android SDK 버전을 사용 하 여 컴파일로 설정 하는 대상 옵션](android-api-levels-images/vs-minimum-version-sml.png)](android-api-levels-images/vs-minimum-version.png#lightbox)

선택 하는 경우 **사용 하 여 SDK 버전을 사용 하 여 컴파일**, 최소 Android 버전 대상 프레임 워크와 동일 하 게 됩니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac에 대 한 Visual Studio에서 대상 프레임 워크 설정에 액세스 하려면 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **옵션**;이 열립니다는 **프로젝트 옵션** 대화 상자. 로 이동 **빌드 > Android 응용 프로그램**합니다.
오른쪽의 드롭다운 메뉴를 사용 하 여 **최소 Android 버전**, 응용 프로그램에 대 한 최소 Android 버전을 설정할 수 있습니다.

[![최소 Android 버전을 자동으로 사용 하 여 대상 프레임 워크 버전으로 설정](android-api-levels-images/xs-minimum-version-sml.png)](android-api-levels-images/xs-minimum-version.png#lightbox)

선택 하는 경우 **자동 &ndash; 대상 프레임 워크 버전을 사용 하 여**, 최소 Android 버전 대상 프레임 워크와 동일 하 게 됩니다.

-----


<a name="target" />

### <a name="target-android-version"></a>Android 버전 대상

*대상 Android 버전* (라고도 `targetSdkVersion`) API 수준 Android 장치에 있는 응용 프로그램은 해야 하는 실행 되도록 합니다. Android이이 설정을 사용 하 여 모든 호환성 동작을 사용할 것인지를 결정 &ndash; 이렇게 하면 예상 대로 작동 하도록 앱을 계속 되도록 합니다. Android 앱의 대상 Android 버전 설정을 사용 하 여 (이 Android 버전과 호환성을 제공 하는 방법) 충돌 없이 앱에 적용할 수 있는 동작 변경 내용을 알아보세요.

대상 프레임 워크 및 대상 Android 버전 이름이 매우 유사한 않고 동일한 작업 하지 않습니다. 대상 프레임 워크 대상 API 수준 정보를 전달 Xamarin.Android에 사용 하기 위해 *컴파일 타임*대상 Android 버전에 사용 하기 위해 Android에는 대상 API 수준 정보를 전달 하는 반면,  *실행 시간* (때 앱이 장치에 설치 및 실행).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio에서이 설정에 액세스 하려면 프로젝트 속성에서 열고 **솔루션 탐색기** 선택 하 고는 **Android 매니페스트** 페이지. 아래에 있는 드롭 다운 메뉴에서 **대상 Android 버전** 응용 프로그램에 대 한 대상 Android 버전을 선택할 수 있습니다.

[![SDK 버전을 사용 하 여 컴파일로 설정 하는 Android 대상 버전](android-api-levels-images/vs-target-version-sml.png)](android-api-levels-images/vs-target-version.png#lightbox)

Android 응용 프로그램을 테스트 하는 데 사용할 수 있는 최신 버전으로 대상 Android 버전을 명시적으로 설정 하는 것이 좋습니다. 최신 Android SDK 버전으로 설정 해야 이상적으로 &ndash; 동작 변경 내용을 통해 작업을 수행 하기 전에 새로운 Api를 사용할 수 있습니다. 대부분의 개발자에 대 한 우리 *없는* 대상 Android 버전 설정 하는 것 **사용 하 여 SDK 버전을 사용 하 여 컴파일**합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac에 대 한 Visual Studio에서 대상 프레임 워크 설정에 액세스 하려면 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **옵션**;이 열립니다는 **프로젝트 옵션** 대화 상자. 로 이동 **빌드 > Android 응용 프로그램**합니다.
오른쪽의 드롭다운 메뉴를 사용 하 여 **대상 Android 버전**, 응용 프로그램에 대 한 대상 Android 버전을 설정할 수 있습니다.

[![자동으로 사용 하 여 대상 프레임 워크 버전으로 대상 Android 버전 설정](android-api-levels-images/xs-target-version-sml.png)](android-api-levels-images/xs-target-version.png#lightbox)

Android 응용 프로그램을 테스트 하는 데 사용할 수 있는 최신 버전으로 대상 Android 버전을 명시적으로 설정 하는 것이 좋습니다. 사용 가능한 최신 Android SDK 버전을 이상적으로 설정 해야 &ndash; 동작 변경 내용을 통해 작업을 수행 하기 전에 새로운 Api를 사용할 수 있습니다. 대부분의 개발자에 대 한 권장 하지는 않습니다 대상 Android 버전을 설정 **자동으로 사용 하 여 대상 프레임 워크 버전**합니다.

-----

일반적으로 대상 Android 버전 최소 Android 버전 및 대상 프레임 워크도 제한 되어야 합니다. 구체적인 요건은 다음과 같습니다.

**최소 Android 버전 < = 대상 Android 버전 < = 대상 프레임 워크**

SDK 수준에 대 한 자세한 내용은 Android 개발자 참조 [사용 하 여 sdk](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html) 설명서입니다.


<a name="runtimechecks" />

## <a name="runtime-checks-for-android-versions"></a>Android 버전에 대 한 런타임 검사

Android의 각 새 버전이 출시 되 면 프레임 워크 API 업데이트 되어 새로운 제공 하거나 대체 기능입니다. 몇 가지 예외를 제외 하 고, 이전 Android 버전의 API 기능을 수정 하지 않고 새 Android 버전에 앞으로 수행 됩니다. 결과적으로, 응용 프로그램에서는 특정 Android API 수준에서 실행 하는 경우 일반적으로 됩니다 수정 하지 않고 이후 Android API 수준에서 실행할 수 있습니다. 하지만 경우에 어떻게 하려는 이전 버전의 Android에서 앱 실행?

최소 Android 버전을 선택 하는 경우 *낮은* 대상 프레임 워크 설정 보다 일부 Api 사용 하지 못할 런타임에 응용 프로그램에 있습니다. 그러나 기능 제한 하면서도 이전 장치에서 앱 실행 여전히 수 있습니다. 최소 Android 버전 설정에 해당 하는 Android 플랫폼에서 사용할 수 없는 각 API에 대 한 코드 값을 확인 명시적으로 해야 합니다는 `Android.OS.Build.VERSION.SdkInt` 응용 프로그램에서 실행 되는 플랫폼의 API 수준을 결정 하는 속성입니다. API 수준이 *낮은* 를 호출 하려는 API를 지 원하는 최소 Android 버전 보다 다음 코드는이 API를 호출 하지 않고 제대로 작동 하는 방법을 찾아야 합니다.

예를 들어 가정 하겠습니다를 사용 하 여 [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) 에서 실행할 때 알림을 분류 하는 메서드 **Android 5.0 롤리팝** (이상), 앱을 원하는 하지만 이전 버전의 Android와 같은 실행 **Android 4.1 젤리 Bean** (여기서 `SetCategory` 를 사용할 수 없습니다). 이 가이드의 시작 부분에서 Android 버전 테이블을 참조할 것을 확인할에 대 한 빌드 버전 코드 **Android 5.0 롤리팝** 은 `Android.OS.BuildVersionCodes.Lollipop`합니다. Android where의 이전 버전을 지원 하기 위해 `SetCategory` 은 사용할 수 없는 코드 API 수준에서 런타임에 검색 하 고 조건에 따라 호출할 수 `SetCategory` 은 경우에 API 수준 롤리팝 빌드 버전 코드 보다 크거나 같은 경우:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    builder.SetCategory(Notification.CategoryEmail);
}
```

이 예제에서는 앱의 대상 프레임 워크로 설정 되어 **Android 5.0 (API 수준 21)** 로 설정 된 최소 Android 버전 및 **Android 4.1 (API 수준 16)**합니다. 때문에 `SetCategory` 는 API 수준에서 사용할 수 `Android.OS.BuildVersionCodes.Lollipop` 이상 버전에서는이 예제 코드를 호출 합니다 `SetCategory` 경우에 사용할 수 있는 상태인 &ndash; 됩니다 *하지* 호출 하려고 `SetCategory` 때 API 수준은 16, 17, 18, 19, 또는 20입니다. 만 하는 알림을 제대로 (때문에 이러한 형식에서 같은 분류 되지 않은) 정렬 되지, 아직는 알림을 사용자에 게 경고할 여전히 게시 되어 이러한 이전 Android 버전에서 기능이 줄어듭니다. 앱 작동 하지만 해당 기능은 약간 떨어집니다.

일반적으로 빌드 버전을 검사 하는 코드에서 이전 방식으로 비교 새로운 방식으로 작업을 수행한 간의 런타임에 결정 수 있습니다. 예를 들어:

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

줄이거나 하나 이상의 Api 부족 이전 Android 버전에서 실행 될 때 앱의 기능을 수정 하는 방법을 설명 하는 빠르고 단순 규칙이 있습니다. 경우에 따라 (에서 같이 이러한는 `SetCategory` 위 예제)을 사용할 수 없는 경우 단순히 API 호출을 생략 해도 됩니다. 그러나 경우에 따라 해야 시기에 대 한 대체 기능을 구현 `Android.OS.Build.VERSION.SdkInt` 응용 프로그램의 최적 경험을 제공 해야 함을 수준 API 보다 작을 것으로 검색 합니다.

<a name="libraries" />

## <a name="api-levels-and-libraries"></a>API 수준 및 라이브러리

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

대상 프레임 워크 설정에만 구성할 수 있습니다 (예: 클래스 라이브러리 또는 바인딩 라이브러리) Xamarin.Android 라이브러리 프로젝트를 만들 때 &ndash; 최소 Android 버전 및 대상 Android 버전 설정을 사용할 수 없는 합니다. 있기 때문에는 없는 **Android 매니페스트** 페이지:

[![Android 버전 옵션을 사용 하 여 컴파일만을 사용할 수](android-api-levels-images/vs-library-options-sml.png)](android-api-levels-images/vs-library-options.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Xamarin.Android 라이브러리 프로젝트를 만들 때 없습니다 **Android 응용 프로그램** 최소 Android 버전 및 대상 Android 버전을 구성할 수 있는 페이지 &ndash; 최소 Android 버전 및 대상 Android 버전 설정은 사용할 수 없습니다.
있기 때문에는 없는 **빌드 > Android 응용 프로그램** 페이지):

[![빌드 최소값 및 대상 버전 옵션 없이 일반 페이지](android-api-levels-images/xs-library-options-sml.png)](android-api-levels-images/xs-library-options.png#lightbox)

-----

최소 Android 버전 및 대상 Android 버전 설정을 사용할 수 없는 결과 라이브러리는 독립 실행형 앱 아니므로 &ndash; 라이브러리와 패키지 된 앱에 따라 모든 Android 버전에서 실행할 수 있습니다. 라이브러리 되어야 하는 방법을 지정할 수 있습니다 *컴파일된*, 있지만 플랫폼 API 레벨을 라이브러리에서 실행 될 예측할 수 없습니다. 이 점을 고려 소비 하거나 라이브러리를 만들 때 다음 모범 사례를 구현 해야 합니다.

-   **Android 라이브러리를 사용할 때** &ndash; 응용 프로그램에서 Android 라이브러리를 사용 하는 경우 설정 해야 하는 API 수준 설정 즉 앱의 대상 프레임 워크 *으로 높은* 대상 라이브러리의 프레임 워크 설정입니다.

-   **Android 라이브러리를 만들 때** &ndash; 다른 응용 프로그램에서 사용 하기 위해 Android 라이브러리를 만드는 경우 사용할 대상 프레임 워크 설정에 따라 컴파일하는 데 필요한 최소 API 수준으로 설정 해야 합니다.

라이브러리 여기서 (응용 프로그램에서 충돌이 발생할 수 있습니다)는 런타임 시 사용할 수 없는 API를 호출 하려고 하는 상황을 방지 하려면 이러한 모범 사례를 사용 하는 것이 좋습니다. 라이브러리 개발자 인 경우에 총 API 노출 영역 작고 잘 설정 된 하위 집합에 대 한 API 호출의 사용량을 제한 하도록 해야 합니다. 이렇게 하면 더 넓은 범위의의 Android 버전 간에 라이브러리 안전 하 게 사용 될 수 있도록 하는 데 도움이 됩니다.


## <a name="summary"></a>요약

이 가이드는 Android API 수준에서 서로 다른 버전의 Android 응용 프로그램 호환성 관리에 사용 되는 방법을 설명 합니다. 이 제공 되는 자세한 단계는 Xamarin.Android를 구성 하기 위한 *대상 프레임 워크*, *최소 Android 버전*, 및 *대상 Android 버전* 프로젝트 설정 합니다. Android SDK Manager를 사용 하 여 SDK 패키지를 설치 하기 위한 지침 런타임에 다른 API 수준은를 처리 하도록 코드를 작성 하는 방법의 예를 포함 하 고 API 수준 Android 라이브러리를 사용 하거나 구성할 때 관리 하는 방법을 설명를 제공 합니다. API 수준 Android 버전 번호 (예: Android 4.4), Android 버전 이름 (예: Kitkat) 및 Xamarin.Android 빌드 버전 코드와 관련 된 포괄적인 목록을 제공 합니다.


## <a name="related-links"></a>관련 링크

- [Android SDK 설정](~/android/get-started/installation/android-sdk.md)
- [SDK CLI 도구 변경](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [CompileSdkVersion, minSdkVersion, 및 targetSdkVersion 선택](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd)
- [API 수준 이란?](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- [코드, 태그 및 빌드 번호](https://source.android.com/source/build-numbers)
