---
title: ViewPager
description: ViewPager는 gestural 탐색을 구현할 수 있도록 하는 레이아웃 관리자입니다. Gestural 탐색을 사용 하면 사용자가 왼쪽 및 오른쪽으로 이동 하 여 데이터 페이지를 단계별로 이동할 수 있습니다. 이 가이드에서는 조각이 있거나 없는 ViewPager를 사용 하 여 gestural 탐색을 구현 하는 방법을 설명 합니다. PagerTitleStrip 및 PagerTabStrip를 사용 하 여 페이지 표시기를 추가 하는 방법에 대해서도 설명 합니다.
ms.prod: xamarin
ms.assetid: D42896C0-DE7C-4818-B171-CB2D5E5DD46A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 600a94a0ee9eb5bcf06dc19d95cf9e77132a2e81
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029060"
---
# <a name="viewpager"></a>ViewPager

_ViewPager는 gestural 탐색을 구현할 수 있도록 하는 레이아웃 관리자입니다. Gestural 탐색을 사용 하면 사용자가 왼쪽 및 오른쪽으로 이동 하 여 데이터 페이지를 단계별로 이동할 수 있습니다. 이 가이드에서는 조각이 있거나 없는 ViewPager를 사용 하 여 gestural 탐색을 구현 하는 방법을 설명 합니다. PagerTitleStrip 및 PagerTabStrip를 사용 하 여 페이지 표시기를 추가 하는 방법에 대해서도 설명 합니다._

## <a name="overview"></a>개요

앱 개발에서 일반적인 시나리오는 사용자에 게 형제 보기 간에 gestural 탐색을 제공 해야 하는 경우입니다. 이 방법에서는 사용자가 왼쪽 또는 오른쪽으로 swipes 콘텐츠 페이지 (예: 설치 마법사 또는 슬라이드 쇼)에 액세스할 수 있습니다. [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)에서 사용할 수 있는 `ViewPager` 위젯을 사용 하 여 이러한 살짝 밀기 보기를 만들 수 있습니다. `ViewPager`는 각 자식 뷰가 레이아웃의 페이지를 구성 하는 여러 자식 뷰로 구성 된 레이아웃 위젯입니다. 

[수평 살짝 밀기 예제가 있는 TreePager 앱의![스크린샷](images/01-intro-sml.png)](images/01-intro.png#lightbox)

일반적으로 `ViewPager`는 [조각](~/android/platform/fragments/index.md)과 함께 사용되지만 `Fragment`의 복잡성을 추가 하지 않고 `ViewPager`를 사용 해야 하는 경우도 있습니다.

`ViewPager` 어댑터 패턴을 사용 하 여 표시할 뷰를 제공 합니다. 여기에서 사용 된 어댑터는 &ndash; [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) 에서 사용 하는 것과 개념적으로 유사 합니다. `PagerAdapter`의 구현을 제공 하 여 `ViewPager` 사용자에 게 표시 되는 페이지를 생성할 수 있습니다. `ViewPager` 표시 되는 페이지는 `View`또는 `Fragment`수 있습니다. `View`s가 표시 되 면 어댑터의 서브 클래스 Android `PagerAdapter` 기본 클래스입니다. `Fragment`s가 표시 되 면 어댑터의 서브 클래스 Android `FragmentPagerAdapter`. Android 지원 라이브러리에는 `Fragment`를 데이터에 연결 하는 방법에 대 한 세부 정보를 제공 하는 `FragmentPagerAdapter` (`PagerAdapter`의 서브 클래스)도 포함 되어 있습니다. 

이 가이드에서는 두 가지 방법을 모두 보여 줍니다. 

- 뷰를 사용 [하는](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-treepager) [viewpager](~/android/user-interface/controls/view-pager/viewpager-and-views.md)에서 `ViewPager`를 사용 하 여 트리 카탈로그 (낙 엽 및 TreePager의 이미지 갤러리)의 뷰를 표시 하는 방법을 보여 주기 위해 개발 되었습니다. 
    `PagerTabStrip` 및 `PagerTitleStrip`는 페이지 탐색에 도움이 되는 제목을 표시 하는 데 사용 됩니다.

- [Viewpager with Fragments](~/android/user-interface/controls/view-pager/viewpager-and-fragments.md)에서 조각을 사용하는 viewpager에서와 함께 를 사용하여 플래시 카드로 수학 문제를 제공하고 사용자 입력에 응답하는 앱을 빌드하는 방법을 `ViewPager` 보여주기 위해`Fragment` 약간 더 복잡한 [FlashCardPager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager) 앱이 개발되었습니다. 

## <a name="requirements"></a>요구 사항

앱 프로젝트에서 `ViewPager`를 사용 하려면 [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 패키지를 설치 해야 합니다. NuGet 패키지를 설치 하는 방법에 대 한 자세한 내용은 [연습: 프로젝트에 Nuget 포함](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)을 참조 하세요. 

## <a name="architecture"></a>아키텍처

`ViewPager`를 사용 하 여 gestural 탐색을 구현 하는 데 세 가지 구성 요소가 사용 됩니다.

- ViewPager
- 어댑터
- 호출기 표시기

이러한 각 구성 요소는 아래에 요약 되어 있습니다.

### <a name="viewpager"></a>ViewPager

`ViewPager`은 한 번에 하나씩 `View`의 컬렉션을 표시 하는 레이아웃 관리자입니다. 해당 작업은 사용자의 살짝 밀기 제스처를 검색 하 고 적절 한 다음 또는 이전 뷰로 이동 하는 것입니다. 예를 들어 아래 스크린샷은 사용자 제스처에 대 한 응답으로 한 이미지에서 다음 이미지로의 전환을 수행 하는 `ViewPager`을 보여 줍니다. 

[보기 간 전환을 표시 하는 TreePager 앱의![확대](images/02-transition-sml.png)](images/02-transition.png#lightbox)

### <a name="adapter"></a>어댑터

`ViewPager`는 *어댑터*에서 데이터를 가져옵니다. 어댑터의 작업은 필요에 따라 `ViewPager`표시 되는 `View`를 만들어 제공 합니다. 아래 다이어그램에서는 어댑터가 `View`를 만들고 채우고 `ViewPager`에 제공 하 &ndash;이 개념을 보여 줍니다. `ViewPager`에서 사용자의 살짝 밀기 제스처를 검색 하면 어댑터에서 표시할 적절 한 `View`를 제공 하도록 요청 합니다. 

[어댑터가 ViewPager에 이미지 및 이름을 연결 하는 방법을 보여 주는![다이어그램](images/03-adapter-sml.png)](images/03-adapter.png#lightbox)

이 특정 예제에서는 `ViewPager`에 전달 되기 전에 트리 이미지와 트리 이름에서 각 `View` 생성 됩니다. 

### <a name="pager-indicator"></a>호출기 표시기

`ViewPager`는 대량 데이터 집합을 표시 하는 데 사용할 수 있습니다. 예를 들어 이미지 갤러리에는 수백 개의 이미지가 포함 될 수 있습니다. 사용자가 매우 많은 데이터 집합을 탐색 하는 데 도움을 주기 위해 문자열을 표시 하는 *호출기 표시기* 를 `ViewPager` 합니다. 문자열은 이미지 제목, 캡션 또는 단지 데이터 집합 내의 현재 뷰의 위치 일 수 있습니다. 

이 탐색 정보를 생성할 수 있는 두 가지 뷰가 있습니다. `PagerTabStrip` 및 `PagerTitleStrip.`는 각각 `ViewPager`맨 위에 문자열을 표시 하 고, 각각은 항상 현재 @no__t_ 표시 된와 동기화 상태로 유지 되도록 `ViewPager`의 어댑터에서 데이터를 가져옵니다. 4_. 두 항목 간의 차이점은 `PagerTabStrip`에는 "current" 문자열에 대 한 시각적 표시기가 포함 되어 있지만 `PagerTitleStrip`는 그렇지 않습니다 (이러한 스크린샷에 표시 된 것 처럼). 

[PagerTitleStrip 및 PagerTabStrip를 사용 하 여 TreePager 앱의![스크린샷](images/04-comparison-sml.png)](images/04-comparison.png#lightbox)

이 가이드에서는 `ViewPager`, 어댑터 및 표시기 앱 구성 요소를 immplement 하 고이를 통합 하 여 gestural 탐색을 지 원하는 방법을 보여 줍니다. 

## <a name="related-links"></a>관련 링크

- [TreePager (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-treepager)
- [FlashCardPager (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager)
