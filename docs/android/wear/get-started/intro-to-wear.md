---
title: "Android 마모 소개"
description: "Google Android 착용의 도입으로 더 이상으로 제한 됩니다 휴대폰과 태블릿 유용한 Android 앱을 개발 하는 데 있어 합니다. Xamarin.Android의 Android 쓰는 유형에서는 지원 되기 때문에 프로그램 손목에서 C# 코드를 실행할 수 있습니다! 이 소개 Android 착용의 기본적인 개요를 제공 하 고 해당 핵심 기능에 설명에서는 Android 착용 2.0에서 사용할 수 있는 기능 개요를 제공 합니다. 일부 더 인기가 쓰는 유형 Android 장치를 나열 하 고 추가 정보에 대 한 중요 한 Google Android 착용 문서에 대 한 링크를 제공 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: EAEF99F0-8FBE-47E4-8644-E7244CFAF464
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: c334e78793f90b4f349f87e12e6b0093fe5cacf8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-android-wear"></a>Android 마모 소개

_Google Android 착용의 도입으로 더 이상으로 제한 됩니다 휴대폰과 태블릿 유용한 Android 앱을 개발 하는 데 있어 합니다. Xamarin.Android의 Android 쓰는 유형에서는 지원 되기 때문에 프로그램 손목에서 C# 코드를 실행할 수 있습니다! 이 소개 Android 착용의 기본적인 개요를 제공 하 고 해당 핵심 기능에 설명에서는 Android 착용 2.0에서 사용할 수 있는 기능 개요를 제공 합니다. 일부 더 인기가 쓰는 유형 Android 장치를 나열 하 고 추가 정보에 대 한 중요 한 Google Android 착용 문서에 대 한 링크를 제공 합니다._

<a name="overview" />

## <a name="overview"></a>개요

다양 한 초기 Motorola 360, LG의 G 시청 하 고 삼성 기어 Live 등의 장치에서 android 마모 실행 됩니다. 기본 제공 GPS 및 오프 라인 음악 재생을 포함 하 여 추가 기능이 포함 된 Sony의 SmartWatch 3을 포함 하는 2 세대도 릴리스 되었습니다. Android 착용 2.0에 대 한 Google에는 팀을 구성 LG 두 개의 새 감시에 대 한: LG 조사식 스포츠 및 LG 조사식 스타일입니다.

![Android 착용 2.0 장치](intro-to-wear-images/hero-image.png "예제 Android 착용 2.0 장치")

추가 추가 하는 NuGet 패키지 마모 특정 UI 컨트롤 및 우리의 Android 4.4W (API 20)를 통해 Android 착용 Xamarin.Android 5.0 이상은 지원 지원 합니다. Xamarin.Android 5.0 이상은 기능 마모 앱 패키징을 포함 합니다. NuGet 패키지는이 가이드의 뒷부분에 설명 된 대로 Android 착용 2.0에 대해 사용할 수 있습니다.


<a name="basics" />

## <a name="android-wear-basics"></a>Android 마모 기본 사항

Android 마모 핸드헬드 Android 앱의 다른 사용자 인터페이스 패러다임을 있습니다. 마모 앱의 첫 번째 유입과 함께 사용을 확장 하도록 설계 된 몇 가지 방법으로 있지만 Android 마모 2.0 부터는 핸드헬드 앱, 마모 앱 사용 되는 독립 실행형 수 있습니다. 마모 앱을 배포 하면 도우미 핸드헬드 장치 앱과 함께 패키지 됩니다. 대부분 착용 하므로 앱 핸드헬드 도우미 응용 프로그램에 따라 달라 집니다, 그리고 핸드헬드 장치 앱과 통신 하는 방법이 필요 합니다. 다음 섹션에서는 이러한 사용 시나리오에 설명 하 고 필수 Android 착용 기능에 간략하게 설명 합니다. 


<a name="scenarios" />

### <a name="usage-scenarios"></a>사용 시나리오

Android 착용의 첫 번째 버전 주로 향상 된 알림 핸드헬드 현재 응용 프로그램을 확장 하 고 핸드헬드 앱와 착용 식 응용 프로그램 간에 데이터를 동기화 하는 중에 집중 되어 있었습니다. 따라서 이러한 시나리오는 비교적 간단 하 게 구현 합니다.

<a name="notifications" />

#### <a name="wearable-notifications"></a>착용 식 알림

Android 착용 지원 하기 위해 알림 핸드헬드 장치 및 착용 식 장치 사이 공유 특성을 활용 하는 것이 쉽습니다. 지원 v4 알림 API를 사용 하 여 및 `WearableExtender` 클래스 (에서 사용할 수는 [Xamarin Android 지원 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)), 음성 입력 하거나 받은 편지함 스타일 카드와 같은 플랫폼의 기본 기능을 활용할 수 있습니다. [RecipeAssistant](https://developer.xamarin.com/samples/monodroid/wear/RecipeAssistant/) 샘플 알림 목록을 쓰는 유형 Android 장치에 전송 하는 방법을 보여 주는 예제 코드를 제공 합니다. 


<a name="companion" />

#### <a name="companion-applications"></a>도우미 응용 프로그램

또 다른 전략 착용 식 장치에서 고유 하 게 실행 되며 도우미 핸드헬드 장치 앱과 쌍을 만드는 것입니다. 이 방법의 좋은 예로 [퀴즈](https://developer.xamarin.com/samples/monodroid/wear/Quiz/) 샘플 응용 프로그램을 만드는 퀴즈 핸드헬드 장치에서 실행 되 고 착용 식 장치에서 몇 가지 퀴즈 질문 하는 방법을 보여 줍니다. 


<a name="ui" />

### <a name="user-interface"></a>사용자 인터페이스

마모 주요 탐색 패턴은 일련의 카드 세로로 정렬 합니다. 이러한 카드 각각 동일한 행에 계층에 있는 동작을 연결할 수 있습니다. `GridViewPager` 클래스가이 기능을 제공 합니다; 동일한 어댑터 개념을 따를 `ListView`합니다. 일반적으로 연결 된 `GridViewPager` 와 `FragmentGridPagerAdaptor` (또는 `GridPagerAdaptor`)으로 각 행 및 열 셀을 나타낼 수 있도록 하는 `Fragment`: 

[ ![탐색 착용](intro-to-wear-images/2d-picker-sml.png "탐색 쓰는 유형")](intro-to-wear-images/2d-picker.png)

큰는 구성 된 작업 단추 사용 (같이 위에) 아래에 있는 작은 설명 텍스트가 있는 원 색이 지정 되지 않습니다.  [GridViewPager](https://developer.xamarin.com/samples/monodroid/wear/GridViewPager/) 샘플에서는 사용 하는 방법을 보여 줍니다. `GridViewPager` 및 `GridPagerAdapter` 마모 응용 프로그램에서입니다.

Android 착용 2.0 마모 사용자 인터페이스를 탐색 서랍, 동작 서랍 및 인라인 실행 단추를 추가합니다. Android 착용 2.0 사용자 인터페이스 요소에 대 한 자세한 참조 Android [Anatomy](https://www.google.com/design/spec-wear/system-overview/anatomy.html) 항목입니다. 


<a name="comm" />

### <a name="communications"></a>통신

Android 마모 두 개의 서로 다른 통신을 Api 착용 식 앱과 함께 핸드헬드 사이의 원활한 통신을 제공 합니다. 

**데이터 API** &ndash; 이 API는 착용 식 장치와 핸드헬드 장치 간에 동기화 된 데이터 저장소와 비슷합니다. 이렇게 하려면 최적이 경우 착용 식과 핸드헬드 사이의 변경 내용을 전파 android 담당 합니다. 착용 식 범위를 벗어나면 나중에 대 한 동기화를 큐 합니다. 이 API에 대 한 주 진입점은 `WearableClass.DataApi`합니다. 이 API에 대 한 자세한 내용은 참조는 Android [동기화 하는 데이터 항목](https://developer.android.com/training/wearables/data-layer/data-items.html) 항목입니다. 

**API 메시지** &ndash; 이 API를 사용 하면 하위 수준 통신 경로 사용할 수 있습니다: 작은 페이로드 핸드헬드 장치 및 착용 식 응용 프로그램 간의 동기화 하지 않고 단방향 전송 됩니다.
이 API에 대 한 주 진입점은 `WearableClass.MessageApi`합니다.
이 API에 대 한 자세한 내용은 참조는 Android [송신 및 수신 메시지](https://developer.android.com/training/wearables/data-layer/messages.html) 항목입니다.

각 API 수신기 인터페이스를 통해 해당 메시지를 받기 위한 콜백을 등록 하거나 또는에서 파생 되는 응용 프로그램에서 서비스를 구현 하도록 선택할 수 있습니다 `WearableListenerService`합니다.
Android 착용 여이 서비스를 자동으로 인스턴스화할 수 됩니다.
[FindMyPhone](https://developer.xamarin.com/samples/monodroid/wear/FindMyPhoneSample/) 샘플에서는 구현 하는 방법을 보여 줍니다.는 `WearableListenerService`합니다.


<a name="deploy" />

### <a name="deployment"></a>배포

각 착용 식 응용 프로그램은 주 응용 프로그램 APK 안에 포함 된 자체 APK 파일 함께 배포 됩니다. 이 패키징 Xamarin.Android 5.0 이상에서는 자동으로 처리 하지만 버전 5.0 이전 버전의 Xamarin.Android 수동으로 수행 해야 합니다. 
[패키징 작업](~/android/wear/deploy-test/packaging.md) 자세히 배포에 설명 합니다. 


<a name="further" />

## <a name="going-further"></a>계속 진행 

Android 착용 세요 빌드하고 첫 응용 프로그램을 테스트 하는 가장 좋은 방법은 합니다. 다음 목록은 속도 빠르게 액세스할 수 있도록 권장된 읽기 순서를 보여 줍니다.

1.  [설치 프로그램 & 설치](~/android/wear/get-started/installation.md) 설치 Xamarin.Android 마모 앱을 빌드하기 위한 개발 환경을 구성 하기 위한 자세한 지침을 제공 합니다. 

2.  필요한 패키지를 설치 하는 에뮬레이터 또는 장치 구성 후 참조 [안녕하세요, 마모](~/android/wear/get-started/hello-wear.md) 해당 핸들 단추 작은 쓰는 유형 Android 프로젝트를 만드는 방법을 설명 하는 단계별 지침에 대 한 클릭 하 고 표시 하는 마모 장치에서 카운터를 클릭 합니다. 

3.  [배포 및 테스트](~/android/wear/deploy-test/index.md) 보다 자세한 구성 하 고 에뮬레이터 및 장치에서 Bluetooth 통해 마모 장치에 앱을 배포 하는 방법에 대 한 지침을 포함 하 여를 배포 하는 방법에 대 한 정보를 제공 합니다.

4.  [화면 크기와 작업](~/android/wear/screen-sizes.md) 를 미리 보고 마모 장치에서 다양 한 사용 가능한 화면 크기에 대 한 사용자 인터페이스를 최적화 하는 방법에 설명 합니다. 

5.  [패키징 작업](~/android/wear/deploy-test/packaging.md) 수동으로 Google Play에서 배포에 대 한 마모 앱을 패키징하는 단계를 설명 합니다.

첫 번째 마모 앱을 만든 후에 Android 착용에 대 한 사용자 지정 watch 화면을 구축해 보고 하는 것이 좋습니다. 
[Watch 화면 만들기](~/android/wear/platform/creating-a-watchface.md) 훼손는 디지털 조사식 얼굴 서비스, 뒤에 추가 기능으로는 아날로그 스타일 watch 화면을 개선 하는 더 많은 코드를 개발 하기 위한 단계별 지침 및 예제 코드를 제공 합니다. 


<a name="wear2" />

## <a name="android-wear-20"></a>Android 마모 2.0

Android 착용 2.0와 같은 다양 한 새로운 기능 및 기능을 도입 되었습니다 *복잡성*, 레이아웃, 탐색 및 작업 서랍 및 확장 된 알림에 곡선입니다. 또한 착용 2.0을 사용 하면 핸드헬드 앱 독립적으로 작동 하는 독립 실행형 앱을 빌드할 수 있습니다. 새 *손목 제스처* 기능을 사용 하면 앱과 함께 드는 상호 작용 합니다. 다음 섹션에서는 이러한 기능을 강조 표시 하 고 응용 프로그램에서 사용 하 여 시작 데 유용한 링크를 제공 합니다.


<a name="install2" />

### <a name="install-wear-20-packages"></a>쓰는 2.0 패키지 유형 설치

추가 해야 Xamarin.Android로 착용 2.0 앱을 빌드하려면는 **Xamarin.Android.Wear v2.0** 패키지를 프로젝트 (클릭는 **찾아보기 탭**):

[![Xamarin.Android.Wear v2.0](intro-to-wear-images/wear-nuget-2.0-sml.png "Xamarin.Android.Wear v2.0 NuGet 설치")](intro-to-wear-images/wear-nuget-2.0.png)

이 NuGet 패키지에는 Android 지원 착용 식과 호환 가능 쓰는 유형 라이브러리에 대 한 바인딩을 포함합니다.

외에 **Xamarin.Android.Wear**, 설치 하는 것이 좋습니다는 **Xamarin.GooglePlayServices.Wearable** NuGet: 

[![Xamarin.GooglePlayServices.Wearable](intro-to-wear-images/gpsw-nuget-sml.png "Xamarin.GooglePlayServices.Wearable NuGet 설치")](intro-to-wear-images/gpsw-nuget.png)

<a name="wear2feat" />

### <a name="key-features-of-wear-20"></a>마모 2.0의 주요 기능

Android 착용 2.0은 2014에서 초기 서비스를 시작한 Android 착용는 가장 큰 업데이트입니다. 다음 섹션에서는 Android 착용 2.0의 주요 기능을 강조 표시 및 연결을 위해 제공 됩니다 응용 프로그램에서 이러한 새 기능을 사용 하 여 시작 합니다. 

<a name="compl" />

#### <a name="complications"></a>복잡성

*복잡성* 작은 조사식 시계 화면의 통과 시키는 필요 없이 한 눈에 볼 수 있는 표면 위젯 됩니다. 복잡성은 데스크톱 스타일 대시보드 위젯에;에 대해 비슷합니다. 날씨, 배터리 수명, 일정 이벤트 및 적합성에 대 한 응용 프로그램 통계 등의 정보 표시: 

![복잡성 예제](intro-to-wear-images/complications.png "복잡성 예제")

복잡성에 대 한 자세한 내용은 Android [조사식 얼굴 복잡성](https://developer.android.com/wear/preview/features/complications.html) 항목입니다. 


<a name="drawers" />

#### <a name="navigation-and-action-drawers"></a>탐색 및 작업 서랍 

두 명의 새로운 서랍 착용 2.0에 포함 됩니다. *탐색 서랍*, (의 왼쪽 아래에 표시 됨) 처럼 응용 프로그램 뷰 간에 탐색할 수 있습니다 화면 위쪽에 나타나는 합니다. *동작 서랍*, 작업 목록에서 선택할 수 있습니다 (오른쪽에 표시 됨) 처럼 화면 맨 아래에 나타나는 합니다. 

![탐색 및 작업 서랍](intro-to-wear-images/drawers.png "탐색 및 작업 서랍")

이러한 두 명의 새 대화형 서랍에 대 한 자세한 내용은 참조는 Android [착용 탐색 및 작업](https://developer.android.com/wear/preview/features/ui-nav-actions.html) 항목입니다. 


<a name="curved" />

#### <a name="curved-layouts"></a>곡선된 레이아웃 

마모 2.0에는 곡선된 레이아웃 라운드 마모 장치에 표시 하기 위한 새로운 기능이 도입 되었습니다. 특히, 새 `WearableRecyclerView` 클래스 세로 항목 목록이 라운드 디스플레이에 표시 하기 위해 최적화 됩니다. 

![곡선 레이아웃 예제](intro-to-wear-images/curved-layout.png "곡선 레이아웃 예제")

`WearableRecyclerView` 확장 된 `RecyclerView` 곡선 레이아웃과 순환 스크롤 제스처를 지원 하기 위해 클래스입니다. 자세한 내용은 참조는 Android [WearableRecyclerView](https://developer.android.com/reference/android/support/wearable/view/WearableRecyclerView.html) API 설명서입니다. 


<a name="standalone" />

#### <a name="standalone-apps"></a>독립 실행형 응용 프로그램 

Android 착용 2.0 앱 핸드헬드 앱 독립적으로 작업할 수 있습니다. 즉, 예를 들어 스마트 조사식 계속 수 포함 핸드헬드 장치 꺼져 있거나 멀리 떨어져 착용 식 장치에서 경우에 전체 기능을 제공 합니다. 이 기능에 대 한 자세한 내용은 참조는 Android [독립 실행형 앱](https://developer.android.com/wear/preview/features/standalone-apps.html) 항목입니다.


<a name="wrist" />

#### <a name="wrist-gestures"></a>손목 제스처 

손목 제스처 원활 하 게 사용자가 터치 스크린을 사용 하지 않고 앱과 상호 작용할 수 &ndash; 단일 손으로 응용 프로그램에 사용자가 응답할 수 있습니다. 두 개의 손목 제스처 지원 됩니다. 

-   긋기 손목 아웃
-   제스처 손목

자세한 내용은 참조는 Android [손목 제스처](https://developer.android.com/wear/preview/features/gestures.html) 항목입니다. 


인라인 작업, 스마트 회신, 원격 입력, 확장 된 알림 및 알림에 대 한 새로운 브리징 모드와 같은 더 많은 착용 2.0 기능이 많이 있습니다. 새 착용 2.0 기능에 대 한 자세한 내용은 참조는 Android [API 개요](https://developer.android.com/wear/preview/api-overview.html)합니다. 


<a name="devices" />

## <a name="devices"></a>장치

다음은 Android 착용 실행할 수 있는 장치의 몇 가지 예입니다.

* [Motorola 360](https://moto360.motorola.com/)
* [LG G 조사식](http://www.lg.com/us/smart-watches/lg-W100-g-watch)
* [LG G Watch R](http://www.lg.com/us/smartwatch/g-watch-r)
* [라이브 삼성 기어](http://www.samsung.com/global/microsite/gear/gearlive_design.html)
* [Sony SmartWatch 3](http://www.sonymobile.com/global-en/products/smartwear/smartwatch-3-swr50/)
* [ASU ZenWatch](http://www.asus.com/us/Phones/ASUS_ZenWatch_WI500Q/)


<a name="reading" />

## <a name="further-reading"></a>추가 정보

Google Android 착용 설명서 확인해 보세요.

* [Android 마모에 대 한](http://www.android.com/wear/)
* [Android 마모 응용 프로그램 디자인](https://developer.android.com/design/wear/index.html)
* [android.support.wearable library ](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
* [Android 마모 2.0](https://developer.android.com/wear/preview/index.html)


<a name="summary" />

## <a name="summary"></a>요약

이 소개 Android 착용의 개요를 제공 합니다. Android 착용의 기본 기능을 설명 하 고 Android 착용 2.0에서 도입 된 기능의 개요를 포함 합니다. Xamarin.Android 마모 개발을 시작 하는 개발자가 필수 읽기에 대 한 링크를 제공 및 시장에 현재 Android 착용 장치 중 일부를 나열 합니다.


## <a name="related-links"></a>관련 링크

- [설치 및 설정](~/android/wear/get-started/installation.md)
- [시작](~/android/wear/get-started/index.md)
