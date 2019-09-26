---
title: Android Wear 소개
description: Google의 Android 마모가 도입 되면서 뛰어난 Android 앱을 개발 하는 경우 휴대폰 및 태블릿으로 더 이상 국한 되지 않습니다. Android 용 Xamarin 지원 덕분에 손목에서 코드를 실행할 C# 수 있습니다. 이 소개에서는 Android에서 제공 하는 기본 개요를 제공 하 고, 주요 기능에 대해 설명 하며, Android 출시 2.0에서 사용 가능한 기능에 대 한 개요를 제공 합니다. 이 문서에는 널리 사용 되는 Android 마모 장치 중 일부가 나열 되어 있으며, 추가 정보를 제공 하기 위해 필수 Google Android 마모 된 설명서의 링크를 제공 합니다.
ms.prod: xamarin
ms.assetid: EAEF99F0-8FBE-47E4-8644-E7244CFAF464
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 80c24765022a916fa36e97aaf47b36435b3f7a7b
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "70758494"
---
# <a name="introduction-to-android-wear"></a>Android Wear 소개

_Google의 Android 마모가 도입 되면서 뛰어난 Android 앱을 개발 하는 경우 휴대폰 및 태블릿으로 더 이상 국한 되지 않습니다. Android 용 Xamarin 지원 덕분에 손목에서 코드를 실행할 C# 수 있습니다. 이 소개에서는 Android에서 제공 하는 기본 개요를 제공 하 고, 주요 기능에 대해 설명 하며, Android 출시 2.0에서 사용 가능한 기능에 대 한 개요를 제공 합니다. 이 문서에는 널리 사용 되는 Android 마모 장치 중 일부가 나열 되어 있으며, 추가 정보를 제공 하기 위해 필수 Google Android 마모 된 설명서의 링크를 제공 합니다._

## <a name="overview"></a>개요

Android 마모는 최초의 차세대 Motorola 360, LG의 G watch 및 Samsung 기어 라이브를 비롯 한 다양 한 장치에서 실행 됩니다. 기본 제공 GPS 및 오프 라인 음악 재생을 비롯 한 추가 기능을 사용 하 여 두 번째 세대 (Sony의 SmartWatch 3 포함)도 출시 되었습니다. Android 이상 2.0의 경우 Google은 LG Watch 스포츠와 LG Watch 스타일 이라는 두 가지 새 조사식에 대해 LG를 사용 하 여 팀을 만들었습니다.

![Android 마모 2.0 장치](intro-to-wear-images/hero-image.png "Android 마모 2.0 장치 예")

Xamarin Android 5.0 이상 버전에서는 Android 4.4 W (API 20) 지원 및 추가 마모 별 UI 컨트롤을 추가 하는 NuGet 패키지를 통해 Android를 사용할 수 있습니다. Xamarin Android 5.0 이상에는 마모 앱을 패키징하는 기능도 포함 되어 있습니다. NuGet 패키지는이 가이드의 뒷부분에 설명 된 대로 Android 마모 2.0에도 사용할 수 있습니다.

## <a name="android-wear-basics"></a>Android 마모 기본 사항

Android 마모에는 Android 핸드헬드 앱과 다른 사용자 인터페이스 패러다임이 있습니다. 첫 번째 마모 된 앱은 다른 방식으로 도우미 핸드헬드 앱을 확장 하도록 설계 되었지만 Android 마모 2.0부터 시작 하 여 앱을 독립 실행형으로 사용할 수 있습니다. 마모 된 앱을 배포 하는 경우이 앱은 도우미 핸드헬드 앱과 함께 패키지 됩니다. 대부분의 마모 된 앱은 핸드헬드 앱을 기반으로 하기 때문에 핸드헬드 앱과 통신 하는 방법이 필요 합니다. 다음 섹션에서는 이러한 사용 시나리오를 설명 하 고 필수 Android 마모 기능을 간략하게 설명 합니다. 

### <a name="usage-scenarios"></a>사용 시나리오

Android의 첫 번째 버전은 현재 hpc 응용 프로그램을 향상 된 알림으로 확장 하 고 핸드헬드 앱과 wearable 앱 간에 데이터를 동기화 하는 데 주로 초점을 두었습니다. 따라서 이러한 시나리오는 비교적 간단 하 게 구현할 수 있습니다.

#### <a name="wearable-notifications"></a>Wearable 알림

Android 마모를 지 원하는 가장 간단한 방법은 핸드헬드 장치와 wearable 장치 간에 알림의 공유 특성을 활용 하는 것입니다. Support v4 notification API 및 `WearableExtender` 클래스 ( [Xamarin Android 지원 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)에서 사용 가능)를 사용 하 여 수신함 스타일 카드 또는 음성 입력과 같은 플랫폼의 기본 기능을 활용할 수 있습니다. [RecipeAssistant](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-recipeassistant) 샘플은 Android 마모 장치에 알림 목록을 보내는 방법을 보여 주는 예제 코드를 제공 합니다. 

#### <a name="companion-applications"></a>도우미 응용 프로그램

또 다른 전략은 wearable 장치에서 고유 하 게 실행 되는 완전 한 응용 프로그램을 만들고 도우미 핸드헬드 앱과 쌍을 만드는 것입니다. 이 방법의 좋은 예로는 핸드헬드 장치에서 실행 되 고 wearable 장치에서 퀴즈 질문을 요청 하는 퀴즈를 만드는 방법을 보여 주는 [퀴즈](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-quiz) 샘플 앱이 있습니다. 

### <a name="user-interface"></a>사용자 인터페이스

마모를 위한 기본 탐색 패턴은 세로로 정렬 된 일련의 카드입니다. 이러한 각 카드에는 동일한 행에 계층화 된 관련 작업이 있을 수 있습니다. 클래스 `GridViewPager` 는이 기능을 제공 하며와 `ListView`동일한 어댑터 개념을 준수 합니다. 일반적으로를 각 `GridViewPager` 행과 `FragmentGridPagerAdaptor` 열 셀 `GridPagerAdaptor`을 `Fragment`로 나타낼 수 있는 (또는)와 연결 합니다. 

[![마모 탐색](intro-to-wear-images/2d-picker-sml.png "마모 탐색")](intro-to-wear-images/2d-picker.png#lightbox)

또한 아래와 같이 작은 설명 텍스트를 포함 하는 큰 색 원으로 구성 된 작업 단추를 사용 합니다.  [GridViewPager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-gridviewpager) 샘플에서는 마모 된 앱에서 `GridViewPager` 및 `GridPagerAdapter` 를 사용 하는 방법을 보여 줍니다.

Android 마모 2.0은 마모 된 사용자 인터페이스에 탐색 서랍, 작업 서랍 및 인라인 작업 단추를 추가 합니다. Android 마모 2.0 사용자 인터페이스 요소에 대 한 자세한 내용은 Android [분석](https://www.google.com/design/spec-wear/system-overview/anatomy.html) 항목을 참조 하세요. 

### <a name="communications"></a>통신

Android wearable는 두 가지 통신 Api를 제공 하 여 apps와 도우미 핸드헬드 앱 간의 통신을 용이 하 게 합니다. 

**데이터 API** &ndash; 이 API는 wearable 장치와 핸드헬드 장치 간에 동기화 된 데이터 저장소와 비슷합니다. Android는이 작업을 수행 하는 것이 최적이 면 wearable와 핸드헬드 간의 변경 내용을 전파 합니다. Wearable가 범위를 벗어나는 경우 동기화는 나중에 큐에 대기 합니다. 이 API `WearableClass.DataApi`에 대 한 주 진입점은입니다. 이 API에 대 한 자세한 내용은 Android [데이터 항목 동기화](https://developer.android.com/training/wearables/data-layer/data-items.html) 항목을 참조 하세요. 

**메시지 API** &ndash; 이 API를 통해 하위 수준 통신 경로를 사용할 수 있습니다. 소형 페이로드는 핸드헬드 앱과 wearable 앱 간의 동기화 없이 단방향으로 전송 됩니다.
이 API `WearableClass.MessageApi`에 대 한 주 진입점은입니다.
이 API에 대 한 자세한 내용은 Android [메시지 보내기 및 받기](https://developer.android.com/training/wearables/data-layer/messages.html) 항목을 참조 하세요.

각 API 수신기 인터페이스를 통해 이러한 메시지를 수신 하기 위한 콜백을 등록 하거나에서 `WearableListenerService`파생 되는 응용 프로그램에서 서비스를 구현할 수 있습니다.
이 서비스는 Android 마모에 의해 자동으로 인스턴스화됩니다.
[FindMyPhone](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-findmyphonesample) 샘플에서는을 `WearableListenerService`구현 하는 방법을 보여 줍니다.

### <a name="deployment"></a>배포

각 wearable 앱은 기본 응용 프로그램 APK 내에 포함 된 자체 APK 파일을 사용 하 여 배포 됩니다. 이 패키징은 Xamarin Android 5.0 이상에서 자동으로 처리 되지만 버전 5.0 이전 버전의 Xamarin.ios의 경우 수동으로 수행 해야 합니다. 
[패키징 작업](~/android/wear/deploy-test/packaging.md) 은 배포에 대해 자세히 설명 합니다. 

## <a name="going-further"></a>자세히 살펴보기 

Android 마모에 익숙해지는 가장 좋은 방법은 첫 번째 앱을 빌드하고 테스트 하는 것입니다. 다음 목록에서는 빠른 속도를 제공 하는 데 도움이 되는 권장 읽기 순서를 제공 합니다.

1. 설치 [& 설치](~/android/wear/get-started/installation.md) 에서는 xamarin.ios 앱을 빌드하기 위한 개발 환경을 설치 하 고 구성 하는 방법에 대 한 자세한 지침을 제공 합니다. 

2. 필수 패키지를 설치 하 고 에뮬레이터 또는 장치를 구성한 후에는 단추 클릭을 처리 하 고 마모 된 클릭 카운터를 표시 하는 작은 Android 마모 프로젝트를 만드는 방법을 설명 하는 단계별 지침에 대 한 [Hello, 마모](~/android/wear/get-started/hello-wear.md) 를 참조 하세요. 장치. 

3. [배포 & 테스트](~/android/wear/deploy-test/index.md) 에서는 Bluetooth를 통해 장치에 앱을 배포 하는 방법에 대 한 지침을 비롯 하 여 에뮬레이터 및 장치에 대 한 구성 및 배포에 대 한 자세한 정보를 제공 합니다.

4. [화면 크기 작업](~/android/wear/screen-sizes.md) 에서는 마모 된 장치에서 사용 가능한 다양 한 화면 크기에 맞게 사용자 인터페이스를 미리 보고 최적화 하는 방법을 설명 합니다. 

5. [패키징 작업](~/android/wear/deploy-test/packaging.md) 은 Google Play에서 배포용 앱을 수동으로 패키징하는 단계에 대해 설명 합니다.

첫 번째 마모 된 앱을 만든 후에는 Android 마모를 위한 사용자 지정 시계 얼굴을 빌드 해 볼 수 있습니다. 
[Watch 얼굴을 만드는](~/android/wear/platform/creating-a-watchface.md) 방법에 대 한 단계별 지침과 예제 코드를 제공 하 고, 추가 기능을 제공 하는 아날로그 스타일의 조사식에 더 많은 코드를 추가 하 여 제거 된 digital Watch face 서비스를 개발 합니다. 

## <a name="android-wear-20"></a>Android 마모 2.0

Android 마모 2.0에는 *복잡*한 레이아웃, 탐색 및 작업 서랍, 확장 된 알림과 같은 다양 한 새로운 기능 및 기능이 도입 되었습니다. 또한, 2.0을 사용 하면 핸드헬드 앱과 독립적으로 작동 하는 독립 실행형 앱을 빌드할 수 있습니다. 새 *손목 제스처* 기능을 사용 하면 앱과의 단방향 상호 작용을 사용할 수 있습니다. 다음 섹션에서는 이러한 기능을 강조 하 고 앱에서 사용을 시작 하는 데 도움이 되는 링크를 제공 합니다.

### <a name="install-wear-20-packages"></a>마모 2.0 패키지 설치

Xamarin.ios를 사용 하 여 마모 된 2.0 앱을 빌드하려면 프로젝트에 **xamarin.ios** 패키지를 추가 해야 합니다 ( **찾아보기 탭**클릭).

[![Xamarin. Android.](intro-to-wear-images/wear-nuget-2.0-sml.png "Xamarin.ios를 설치") 합니다.](intro-to-wear-images/wear-nuget-2.0.png#lightbox)

이 NuGet 패키지에는 Android Support Wearable 및 마모 된 호환 라이브러리 모두에 대 한 바인딩이 포함 되어 있습니다.

**Xamarin.ios**뿐만 아니라 **Xamarin.googleplayservices.base. Wearable** NuGet을 설치 하는 것이 좋습니다. 

[![Xamarin.googleplayservices.base. Wearable](intro-to-wear-images/gpsw-nuget-sml.png "Xamarin.googleplayservices.base. Wearable NuGet을 설치 합니다.")](intro-to-wear-images/gpsw-nuget.png#lightbox)

### <a name="key-features-of-wear-20"></a>2\.0의 주요 기능

Android 마모 2.0은 2014에서 초기 출시 이후 Android 출시에 대 한 가장 큰 업데이트입니다. 다음 섹션에서는 Android 용 2.0의 주요 기능을 강조 하 고 앱에서 이러한 새 기능을 사용 하 여 시작 하는 데 도움이 되는 링크를 제공 합니다. 

#### <a name="complications"></a>컴플리케이션

조사식 얼굴을 살짝 밀어 표시 하지 않고 한눈에 볼 수 있는 작은 시청 얼굴 위젯을 사용 하는 것이 매우 *복잡* 합니다. 데스크톱 스타일 대시보드 위젯과 유사 합니다. 날씨, 배터리 수명, 일정 이벤트 및 적합성 앱 통계와 같은 정보를 표시 합니다. 

![복잡 한 예](intro-to-wear-images/complications.png "복잡 한 예")

복잡 한 상황에 대 한 자세한 내용은 Android [Watch 얼굴](https://developer.android.com/wear/preview/features/complications.html) 상황에 맞는 항목을 참조 하세요. 

#### <a name="navigation-and-action-drawers"></a>탐색 및 작업 서랍 

새 서랍 2 개가 착용 2.0에 포함 되어 있습니다. 화면 맨 위에 표시 되는 *탐색 서랍*을 사용 하면 사용자가 아래 왼쪽에 표시 된 것 처럼 앱 보기 간을 탐색할 수 있습니다. 화면 맨 아래에 표시 되는 *작업 서랍*(오른쪽에 표시 됨)은 사용자가 작업 목록에서 선택할 수 있도록 합니다. 

![탐색 및 작업 서랍](intro-to-wear-images/drawers.png "탐색 및 작업 서랍")

이러한 두 개의 새로운 대화형 서랍에 대 한 자세한 내용은 Android [마모 된 탐색 및 작업](https://developer.android.com/wear/preview/features/ui-nav-actions.html) 항목을 참조 하세요. 

#### <a name="curved-layouts"></a>곡선 레이아웃 

마모 2.0은 라운드 마모 장치에서 곡선 레이아웃을 표시 하기 위한 새로운 기능을 소개 합니다. 특히 새 `WearableRecyclerView` 클래스는 라운드 표시의 세로 항목 목록을 표시 하는 데 최적화 되어 있습니다. 

![곡선 레이아웃 예](intro-to-wear-images/curved-layout.png "곡선 레이아웃 예")

`WearableRecyclerView`곡선 레이아웃 및 원형 스크롤 제스처를 지원 하도록 클래스를확장합니다.`RecyclerView` 자세한 내용은 Android [WearableRecyclerView](https://developer.android.com/reference/android/support/wearable/view/WearableRecyclerView.html) API 설명서를 참조 하세요. 

#### <a name="standalone-apps"></a>독립 실행형 앱 

Android 마모 2.0 앱은 핸드헬드 앱과 독립적으로 작동할 수 있습니다. 즉, 예를 들어, wearable 장치에서 동반 되는 핸드헬드 장치가 꺼져 있거나 멀리 떨어져 있는 경우에도 스마트 watch를 통해 전체 기능을 계속 제공할 수 있습니다. 이 기능에 대 한 자세한 내용은 Android [독립 실행형 앱](https://developer.android.com/wear/preview/features/standalone-apps.html) 항목을 참조 하세요.

#### <a name="wrist-gestures"></a>손목 제스처 

손목 제스처를 사용 하면 사용자가 터치 스크린 &ndash; 을 사용 하지 않고 앱과 상호 작용할 수 있으므로 사용자는 단일 손으로 앱에 응답할 수 있습니다. 지원 되는 두 가지 손목 제스처는 다음과 같습니다. 

- 긋기 손목
- 긋기 손목

자세한 내용은 Android [손목 제스처](https://developer.android.com/wear/preview/features/gestures.html) 항목을 참조 하세요. 

인라인 동작, 스마트 회신, 원격 입력, 확장 된 알림 및 알림에 대 한 새로운 브리징 모드와 같은 더 많은 마모 된 2.0 기능이 있습니다. 새 마모 된 2.0 기능에 대 한 자세한 내용은 Android [API 개요](https://developer.android.com/wear/preview/api-overview.html)를 참조 하세요. 

## <a name="devices"></a>디바이스

Android를 실행할 수 있는 장치의 몇 가지 예는 다음과 같습니다.

- [Motorola 360](https://moto360.motorola.com/)
- [LG G Watch](http://www.lg.com/us/smart-watches/lg-W100-g-watch)
- [LG G Watch R](http://www.lg.com/us/smartwatch/g-watch-r)
- [Samsung 기어 라이브](http://www.samsung.com/global/microsite/gear/gearlive_design.html)
- [Sony SmartWatch 3](http://www.sonymobile.com/global-en/products/smartwear/smartwatch-3-swr50/)
- [ASUS ZenWatch](http://www.asus.com/us/Phones/ASUS_ZenWatch_WI500Q/)

## <a name="further-reading"></a>추가 정보

Google의 Android 마모 설명서를 확인 하세요.

- [Android 마모 정보](http://www.android.com/wear/)
- [Android 마모 앱 디자인](https://developer.android.com/design/wear/index.html)
- [wearable 라이브러리](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [Android 마모 2.0](https://developer.android.com/wear/preview/index.html)

## <a name="summary"></a>요약

이 소개에서는 Android 마모의 개요를 제공 했습니다. Android에서 제공 하는 기본 기능에 대해 간략하게 설명 하 고 Android 출시 2.0에 도입 된 기능에 대 한 개요를 제공 했습니다. 개발자가 Xamarin Android 마모 된 개발을 시작 하는 데 도움이 되는 필수 읽기의 링크를 제공 했으며, 현재 시장에서 제공 되는 일부 Android 장치에 대 한 예제를 소개 했습니다.

## <a name="related-links"></a>관련 링크

- [설치 및 설정](~/android/wear/get-started/installation.md)
- [시작](~/android/wear/get-started/index.md)
