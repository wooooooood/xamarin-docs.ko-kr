---
title: Android Wear 소개
description: Google의 Android Wear의 도입으로 더 이상으로 제한 됩니다 휴대폰과 태블릿에서 유용한 Android 앱을 개발 하는 데 있어. Android Wear 용 Xamarin.Android의 지원 있게 실행할 수 있습니다 C# 에 코드! 이 소개 Android Wear의 기본적인 개요를 제공 하 고 해당 키 기능을 설명 하며 Android Wear 2.0에서 사용할 수 있는 기능의 개요를 제공 합니다. 더 인기 있는 Android Wear 장치 중 일부를 나열 하 고 추가 정보에 대 한 중요 한 Google Android Wear 문서에 대 한 링크를 제공 합니다.
ms.prod: xamarin
ms.assetid: EAEF99F0-8FBE-47E4-8644-E7244CFAF464
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: a35cb82f4f6d20e91f45a782c73d3ef811947c3a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117787"
---
# <a name="introduction-to-android-wear"></a>Android Wear 소개

_Google의 Android Wear의 도입으로 더 이상으로 제한 됩니다 휴대폰과 태블릿에서 유용한 Android 앱을 개발 하는 데 있어. Android Wear 용 Xamarin.Android의 지원 있게 실행할 수 있습니다 C# 에 코드! 이 소개 Android Wear의 기본적인 개요를 제공 하 고 해당 키 기능을 설명 하며 Android Wear 2.0에서 사용할 수 있는 기능의 개요를 제공 합니다. 더 인기 있는 Android Wear 장치 중 일부를 나열 하 고 추가 정보에 대 한 중요 한 Google Android Wear 문서에 대 한 링크를 제공 합니다._


## <a name="overview"></a>개요

Android Wear 다양 한 항목인 1 세대 Motorola 360, LG의 G 조사식 및 Samsung 기어 Live를 포함 하 여 장치에서 실행 됩니다. Sony의 SmartWatch 3을 포함 하 여 두 번째 세대에 기본 제공 GPS 등 오프 라인 음악 재생 하는 추가 기능을 사용 하 여도 릴리스 되었습니다. Android Wear 2.0에 대 한 Google에 제휴 LG를 사용 하 여 두 개의 새 감시에 대 한: LG 조사식 스포츠와 LG 조사식 스타일입니다.

![Android Wear 2.0 장치](intro-to-wear-images/hero-image.png "예제 Android Wear 2.0 장치")

NuGet 패키지 추가 Wear-특정 UI 컨트롤 및 우리의 Android 4.4W (API 20)를 통해 Xamarin.Android 5.0 이상에서는 Android Wear 지원. Xamarin.Android 5.0 이상 Wear 앱 패키징 기능을 포함 합니다. NuGet 패키지도이 가이드의 뒷부분에 설명 된 대로 Android Wear 2.0에 대해 사용할 수 있습니다.


## <a name="android-wear-basics"></a>Android Wear 기본 사항

Android Wear 핸드헬드 Android 앱의 다른 사용자 인터페이스 패러다임을 있습니다. Wear 앱의 첫 번째 유입과 함께 사용을 확장 하도록 설계 된 핸드헬드 장치 앱에서 몇 가지 방식으로 Android Wear 2.0부터, Wear 앱에는 독립 실행형으로 사용된 될 수 있습니다. Wear 앱을 배포할 때 도우미 핸드헬드 장치 앱을 사용 하 여 패키지에 포함 되어 있습니다. 대부분 Wear 앱 핸드헬드 도우미 앱에 종속 있기 때문에 핸드헬드 장치 앱과 통신 하는 방법이 있습니다. 다음 섹션에서는 이러한 시나리오를 설명 하 고 필수 Android Wear 기능을 간략하게 설명 합니다. 



### <a name="usage-scenarios"></a>사용 시나리오

Android Wear의 첫 번째 버전에 집중 되었으며 주로 향상 된 알림으로 핸드헬드 현재 응용 프로그램을 확장 하 고 핸드헬드 장치 앱 및 착용 식 앱 간에 데이터를 동기화 합니다. 따라서 이러한 시나리오는 비교적 간단 하 게 구현 합니다.


#### <a name="wearable-notifications"></a>착용 식 알림

Android Wear를 지원 하기 위해 가장 간단한 방법은 공유 핸드헬드 장치 및 착용 식 장치 간에 알림 특성을 활용 하도록 합니다. 지원 v4 알림 API를 사용 하 여 및 `WearableExtender` 클래스 (에서 사용할 수 있는 합니다 [Xamarin Android 지원 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)), 입력 음성 하거나 수신함 스타일 카드와 같은 플랫폼의 기본 기능을 활용할 수 있습니다. 합니다 [RecipeAssistant](https://developer.xamarin.com/samples/monodroid/wear/RecipeAssistant/) 샘플 Android Wear 장치에 알림 목록을 전송 하는 방법을 보여 주는 예제 코드를 제공 합니다. 



#### <a name="companion-applications"></a>도우미 응용 프로그램

또 다른 전략 착용 식 장치에서 고유 하 게 실행 하는 도우미 핸드헬드 장치 앱을 사용 하 여 쌍 완전 한 응용 프로그램을 만드는 것입니다. 이 방법의 좋은 예는 [퀴즈](https://developer.xamarin.com/samples/monodroid/wear/Quiz/) 핸드헬드 장치에서 실행 되는 착용 식 장치에서 몇 가지 퀴즈 질문 퀴즈를 만드는 방법을 보여 주는 샘플 앱입니다. 



### <a name="user-interface"></a>사용자 인터페이스

Wear 기본 탐색 패턴은 일련의 카드 세로로 정렬 합니다. 이러한 카드의 각각 같은 행에서 계층에 있는 작업에 연결할 수 있습니다. 합니다 `GridViewPager` 이 기능을 제공 하는 클래스와 동일한 어댑터 개념을 따를; `ListView`합니다. 일반적으로 연결 합니다 `GridViewPager` 사용 하 여를 `FragmentGridPagerAdaptor` (또는 `GridPagerAdaptor`)으로 각 행 및 열 셀을 나타내는 수를 `Fragment`: 

[![탐색 wear](intro-to-wear-images/2d-picker-sml.png "Wear 탐색")](intro-to-wear-images/2d-picker.png#lightbox)

Wear는 많은 구성 된 작업 단추를 사용 (위의 그림 참조)으로 아래에 있는 작은 설명 텍스트를 사용 하 여 원 색 지정도 합니다.  합니다 [GridViewPager](https://developer.xamarin.com/samples/monodroid/wear/GridViewPager/) 샘플을 사용 하는 방법에 설명 `GridViewPager` 고 `GridPagerAdapter` Wear 앱에서.

Android Wear 2.0 Wear 사용자 인터페이스 탐색 서랍, 작업 서랍 및 인라인 실행 단추를 추가합니다. Android Wear 2.0 사용자 인터페이스 요소에 대 한 자세한 내용은 참조는 Android [분석](https://www.google.com/design/spec-wear/system-overview/anatomy.html) 항목입니다. 



### <a name="communications"></a>통신

Android Wear는 두 개의 서로 다른 통신을 착용 식 앱 및 관련 핸드헬드 앱 간의 통신을 용이 하 게 Api를 제공 합니다. 

**데이터 API** &ndash; 이 API는 착용 식 장치 및 핸드헬드 장치 간의 동기화 된 데이터 저장소와 비슷합니다. 이렇게 하는 데 가장 적합 한 경우 착용 식 및 핸드헬드 장치 간의 변경 내용을 전파 android 담당 합니다. 착용 식의 범위를 벗어난 경우 큐에 나중에 동기화 합니다. 이 API에 대 한 주 진입점은 `WearableClass.DataApi`합니다. 이 API에 대 한 자세한 내용은 참조는 Android [동기화 데이터 항목](https://developer.android.com/training/wearables/data-layer/data-items.html) 항목입니다. 

**API 메시지** &ndash; 이 API를 사용 하면 하위 수준 통신 경로 사용할 수 있습니다: 작은 페이로드는 휴대용 및 착용 식 앱 간에 동기화 하지 않고 단방향 전송 됩니다.
이 API에 대 한 주 진입점은 `WearableClass.MessageApi`합니다.
이 API에 대 한 자세한 내용은 참조는 Android [보내기 및 메시지 수신을](https://developer.android.com/training/wearables/data-layer/messages.html) 항목입니다.

각 API 수신기 인터페이스를 통해 해당 메시지를 수신 하기 위한 콜백을 등록, 또는에서 파생 되는 앱에서 서비스를 구현 하도록 선택할 수 있습니다 `WearableListenerService`합니다.
Android Wear 여이 서비스를 자동으로 인스턴스화할 수 됩니다.
합니다 [FindMyPhone](https://developer.xamarin.com/samples/monodroid/wear/FindMyPhoneSample/) 샘플에서는 구현 하는 방법을 보여 줍니다.는 `WearableListenerService`합니다.



### <a name="deployment"></a>배포

각 착용 식 앱은 주 응용 프로그램이 APK 내에 포함 된 APK 파일 자체를 사용 하 여 배포 됩니다. 이 패키징 Xamarin.Android 5.0 이상에서는 자동으로 처리 됩니다 있지만 버전 5.0 이전 버전의 Xamarin.Android는 수동으로 수행 해야 합니다. 
[패키징 작업](~/android/wear/deploy-test/packaging.md) 자세히 배포에 설명 합니다. 



## <a name="going-further"></a>계속 진행 

Android Wear 익숙해지기 위해 빌드하고 첫 번째 앱을 테스트 하는 가장 좋은 방법은 합니다. 다음은 신속 하 게 속도 높일 수 있도록 하려면을 읽어보면 좋은 자료를 제공 합니다.

1.  [설정 및 설치](~/android/wear/get-started/installation.md) 설치 하 고 Xamarin.Android Wear 앱을 빌드하기 위한 개발 환경 구성에 대 한 자세한 지침을 제공 합니다. 

2.  필요한 패키지를 설치 하면 에뮬레이터 또는 장치 구성 후 볼 [안녕하세요, Wear](~/android/wear/get-started/hello-wear.md) 핸들 단추는 작은 Android Wear 프로젝트를 만드는 방법을 설명 하는 단계별 지침은 다음을 클릭 하 고 표시를 Wear 장치에서 카운터를 클릭 합니다. 

3.  [배포 및 테스트](~/android/wear/deploy-test/index.md) 보다 자세한 구성 하 고 에뮬레이터 및 Bluetooth 통한 Wear 장치에 앱을 배포 하는 방법에 대 한 지침을 포함 하 여 장치에 배포 하는 방법에 대 한 정보를 제공 합니다.

4.  [화면 크기를 사용 하 여 작업](~/android/wear/screen-sizes.md) 미리 Wear 장치에서 다양 한 사용 가능한 화면 크기에 대 한 사용자 인터페이스를 최적화 하는 방법을 설명 합니다. 

5.  [패키징 작업](~/android/wear/deploy-test/packaging.md) Google Play에서 배포를 위해 Wear 앱을 수동으로 패키징하는 단계를 설명 합니다.

첫 번째 Wear 앱을 만든 후에 Android Wear 용 사용자 지정 시계 모드 빌드를 시도 하는 것이 좋습니다. 
[시계 모드 만들기](~/android/wear/platform/creating-a-watchface.md) 디지털 조사식 얼굴 서비스를 추가 기능을 사용 하 여는 아날로그 방식의 watch 화면을 향상 시키는 더 많은 코드 뒤에 단지 개발 하기 위한 단계별 지침 및 예제 코드를 제공 합니다. 



## <a name="android-wear-20"></a>Android Wear 2.0

Android Wear 2.0와 같은 다양 한 새로운 기능 및 기능을 도입 되었습니다 *복잡성*, 레이아웃, 탐색 및 작업 서랍 알림과 확장 된 곡선입니다. 또한 Wear 2.0으로 handheld 앱 독립적으로 작동 하는 독립 실행형 앱을 빌드할 수 있습니다. 새 *손목 제스처* 기능을 사용 하면 앱을 사용 하 여 드는 상호 작용 합니다. 다음 섹션에서는 이러한 기능을 강조 하 고 앱에서 사용 하기 시작 하는 데는 링크를 제공 합니다.



### <a name="install-wear-20-packages"></a>설치 Wear 2.0 패키지

Xamarin.Android 사용 하 여 2.0 Wear 앱을 작성 하려면 추가 해야 합니다 **Xamarin.Android.Wear v2.0** 패키지를 프로젝트 (클릭 합니다 **찾아보기 탭**):

[![Xamarin.Android.Wear v2.0](intro-to-wear-images/wear-nuget-2.0-sml.png "Xamarin.Android.Wear v2.0 NuGet 설치")](intro-to-wear-images/wear-nuget-2.0.png#lightbox)

이 NuGet 패키지에는 Android 지원 착용 식 및 호환성 Wear 라이브러리에 대 한 바인딩을 포함합니다.

외에 **Xamarin.Android.Wear**를 설치 하는 것이 좋습니다 합니다 **Xamarin.GooglePlayServices.Wearable** NuGet: 

[![Xamarin.GooglePlayServices.Wearable](intro-to-wear-images/gpsw-nuget-sml.png "Xamarin.GooglePlayServices.Wearable NuGet 설치")](intro-to-wear-images/gpsw-nuget.png#lightbox)


### <a name="key-features-of-wear-20"></a>Wear 2.0의 주요 기능

Android Wear 2.0 2014의 초기 출시 이후 대대적으로 Android Wear 업데이트 됩니다. 다음 섹션에서는 Android Wear 2.0의 주요 기능을 강조 표시 하는 데 대 한 링크가 제공 됩니다 및 앱에서 이러한 새 기능을 사용 하기 시작 합니다. 


#### <a name="complications"></a>복잡성

*복잡성* 얼굴 위젯 watch 화면을 살짝 필요 없이 한눈에 볼 수 있는 작은 조사식 됩니다. 복잡성은 데스크톱 스타일 대시보드 위젯;에 대해 비슷합니다. 날씨, 배터리, 일정 이벤트, 및 적합성에 대 한 앱 통계 등의 정보 표시: 

![복잡성 예제](intro-to-wear-images/complications.png "복잡성 예제")

복잡성에 대 한 자세한 내용은 참조는 Android [조사식 얼굴 복잡성](https://developer.android.com/wear/preview/features/complications.html) 항목입니다. 



#### <a name="navigation-and-action-drawers"></a>탐색 및 작업 서랍 작업 

두 명의 새 서랍 Wear 2.0에 포함 됩니다. 합니다 *탐색 서랍*, 왼쪽 아래과 같이 앱 뷰 간에 탐색할 수 있습니다 화면의 위쪽에 표시 되는 합니다. 합니다 *동작 서랍*, 작업 목록에서 선택할 수 있습니다 (오른쪽에 표시 됨)으로 화면 맨 아래에 나타나는 합니다. 

![탐색 및 작업 서랍](intro-to-wear-images/drawers.png "탐색 및 작업 서랍 작업")

이러한 두 명의 새 대화형 서랍 작업에 대 한 자세한 내용은 참조는 Android [Wear 탐색 및 작업](https://developer.android.com/wear/preview/features/ui-nav-actions.html) 항목입니다. 



#### <a name="curved-layouts"></a>곡선된 레이아웃 

Wear 2.0에는 곡선된 레이아웃 반올림 Wear 장치에서 표시 하는 것에 대 한 새 기능이 도입 되었습니다. 특히, 새 `WearableRecyclerView` 클래스 round 디스플레이에서 세로 항목의 목록을 표시 하기 위해 최적화 됩니다. 

![곡선 레이아웃 예제](intro-to-wear-images/curved-layout.png "곡선 레이아웃 예제")

`WearableRecyclerView` 확장 된 `RecyclerView` 곡선 레이아웃과 순환 스크롤 제스처를 지원 하기 위해 클래스입니다. 자세한 내용은 참조는 Android [WearableRecyclerView](https://developer.android.com/reference/android/support/wearable/view/WearableRecyclerView.html) API 설명서입니다. 



#### <a name="standalone-apps"></a>독립 실행형 앱 

Android Wear 2.0 앱 핸드헬드 앱 독립적으로 작업할 수 있습니다. 즉, 예를 들어, 스마트 워치 수 계속 포함 핸드헬드 장치는 꺼져 있거나 멀리 착용 식 장치에서 하는 경우에 전체 기능을 제공 합니다. 이 기능에 대 한 자세한 내용은 참조는 Android [독립 실행형 앱](https://developer.android.com/wear/preview/features/standalone-apps.html) 항목입니다.



#### <a name="wrist-gestures"></a>손목 제스처 

손목 제스처 수 있도록 사용자가 터치 스크린을 사용 하지 않고 앱과 상호 작용할 &ndash; 단일 손 모양으로 앱에 응답할 수 있습니다. 두 손목 제스처 지원 됩니다. 

-   긋기 손목 아웃
-   긋기 손목의

자세한 내용은 참조는 Android [손목 제스처](https://developer.android.com/wear/preview/features/gestures.html) 항목입니다. 


많은 Wear 2.0 기능 인라인 작업, 스마트 회신, 원격 입력, 확장 된 알림 및 알림에 대 한 새로운 브리징 모드와 같은 경우 새 Wear 2.0 기능에 대 한 자세한 내용은 참조는 Android [API 개요](https://developer.android.com/wear/preview/api-overview.html)합니다. 



## <a name="devices"></a>장치

Android Wear를 실행할 수 있는 장치의 몇 가지 예는 다음과 같습니다.

* [Motorola 360](https://moto360.motorola.com/)
* [LG G 시청](http://www.lg.com/us/smart-watches/lg-W100-g-watch)
* [LG G 조사식 R](http://www.lg.com/us/smartwatch/g-watch-r)
* [Samsung 기어 라이브](http://www.samsung.com/global/microsite/gear/gearlive_design.html)
* [Sony SmartWatch 3](http://www.sonymobile.com/global-en/products/smartwear/smartwatch-3-swr50/)
* [ASU ZenWatch](http://www.asus.com/us/Phones/ASUS_ZenWatch_WI500Q/)



## <a name="further-reading"></a>추가 정보

Google Android Wear 설명서를 확인해 보십시오.

* [Android Wear에 대 한](http://www.android.com/wear/)
* [Android Wear 앱 디자인](https://developer.android.com/design/wear/index.html)
* [android.support.wearable library ](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
* [Android Wear 2.0](https://developer.android.com/wear/preview/index.html)



## <a name="summary"></a>요약

이 소개 Android Wear의 개요를 제공 합니다. Android Wear의 기본 기능을 설명 하 고 Android Wear 2.0에 도입 된 기능의 개요를 포함 합니다. 개발자가 Xamarin.Android Wear 개발을 시작 하는 데 필수적인 읽기에 대 한 링크가 제공 및 시장에서 현재 Android Wear 장치 중 일부를 나열 하는 것입니다.


## <a name="related-links"></a>관련 링크

- [설치 및 설정](~/android/wear/get-started/installation.md)
- [시작](~/android/wear/get-started/index.md)
