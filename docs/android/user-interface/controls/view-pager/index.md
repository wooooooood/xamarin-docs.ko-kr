---
title: ViewPager
description: "ViewPager gestural 탐색을 구현할 수 있게 하는 레이아웃 관리자입니다. Gestural 탐색 왼쪽과 오른쪽의 데이터 페이지를 통해 단계로 살짝에 사용자를 수 있습니다. 이 가이드와 조각이 없는 ViewPager를 사용 하 여 gestural 탐색을 구현 하는 방법을 설명 합니다. 또한 PagerTitleStrip 및 PagerTabStrip 사용 하 여 페이지 표시기를 추가 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: D42896C0-DE7C-4818-B171-CB2D5E5DD46A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 5e2f93eea970a15df03b00cc962ca7482624973d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="viewpager"></a>ViewPager

_ViewPager gestural 탐색을 구현할 수 있게 하는 레이아웃 관리자입니다. Gestural 탐색 왼쪽과 오른쪽의 데이터 페이지를 통해 단계로 살짝에 사용자를 수 있습니다. 이 가이드와 조각이 없는 ViewPager를 사용 하 여 gestural 탐색을 구현 하는 방법을 설명 합니다. 또한 PagerTitleStrip 및 PagerTabStrip 사용 하 여 페이지 표시기를 추가 하는 방법을 설명 합니다._

 
## <a name="overview"></a>개요

응용 프로그램 개발의 일반적인 시나리오에는 형제 뷰 간에 gestural 탐색이 있는 사용자가 제공 하는 것입니다. 이 방법에서는 사용자 천공 기와 왼쪽 이나 오른쪽으로 액세스의 내용 페이지 (예를 들어 설치 마법사 또는 슬라이드 쇼). 사용 하 여 이러한 살짝 밀어 뷰를 만들 수 있습니다는 `ViewPager` 에서 사용할 수 있는 위젯 [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)합니다. `ViewPager` 은 여러 자식 뷰가 이루어진 레이아웃 위젯 각 자식 뷰 레이아웃에서 페이지를 구성 합니다. 

[![가로 살짝 예제로 앱 TreePager 스크린 샷](images/01-intro-sml.png)](images/01-intro.png#lightbox)

일반적으로 `ViewPager` 와 함께에서 사용 되는 [조각](https://developer.xamarin.com/guides/android/platform_features/fragments/)소비량이 적어지지만 사용 하려는 경우가 있습니다 `ViewPager` 의 복잡 한 추가 하지 않고 `Fragment`s입니다.

`ViewPager` 표시할 보기를 제공 하는 어댑터 패턴을 사용 합니다. 여기에 사용 되는 어댑터에서 사용 되는 개념적으로 비슷합니다 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) &ndash; 의 구현을 제공 `PagerAdapter` 는 페이지를 생성 하는 `ViewPager` 사용자에 게 표시 합니다. 으로 표시 되는 페이지 `ViewPager` 수 `View`s 또는 `Fragment`s입니다. 때 `View`s 표시 되는 어댑터 하위 클래스 Android의 `PagerAdapter` 기본 클래스입니다. 경우 `Fragment`s 표시 되는 어댑터 하위 클래스 Android의 `FragmentPagerAdapter`합니다. Android 지원 라이브러리도 포함 되어 `FragmentPagerAdapter` (의 서브 클래스 `PagerAdapter`)의 연결 세부 정보를 사용 하려면 `Fragment`데이터에 s입니다. 

이 가이드에는 두 가지 방법을 모두 보여 줍니다. 

-   [보기와 Viewpager](~/android/user-interface/controls/view-pager/viewpager-and-views.md), [TreePager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) 앱으로 사용 하는 방법을 보여 주기 위해 개발 된 `ViewPager` 트리 카탈로그 (이미지 갤러리 낙 및 evergreen 트리)의 보기를 표시 하 합니다. 
    `PagerTabStrip`  및 `PagerTitleStrip` 페이지 탐색에 사용 되는 제목을 표시 하는 데 사용 됩니다.

-   [조각 사용 하 여 Viewpager](~/android/user-interface/controls/view-pager/viewpager-and-fragments.md), 약간 더 복잡 [FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) 앱으로 사용 하는 방법을 보여 주기 위해 개발 된 `ViewPager` 와 `Fragment`의수학 문제를 표시 하는 앱을 빌드하려면 플래시 카드 및 사용자 입력에 응답 합니다. 


## <a name="requirements"></a>요구 사항

사용 하도록 `ViewPager` 응용 프로그램 프로젝트에서 설치 해야는 [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 패키지 합니다. NuGet 패키지를 설치 하는 방법에 대 한 자세한 내용은 참조 [연습: 프로젝트에 포함 하는 NuGet](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)합니다. 

 
## <a name="architecture"></a>아키텍처

Gestural 탐색을 구현 하기 위한 세 가지 구성 요소는 사용 `ViewPager`:

-   ViewPager
-   어댑터
-   호출기 표시기

이러한 각 구성이 요소 아래에 요약 되어 있습니다.



### <a name="viewpager"></a>ViewPager

`ViewPager` 컬렉션을 표시 하는 레이아웃 관리자 `View`한 번에 하나의 s입니다. 일은 사용자의 살짝 밀기를 검색 하 고 적절 하 게 다음 또는 이전 보기로 이동 하는 것입니다. 예를 들어, 아래 스크린샷에서 보여 줍니다는 `ViewPager` 사용자 제스처에 대 한 응답에서 다음 이미지에서 전환 하기: 

[![TreePager 클로즈업 앱 보기 간 전환을 표시](images/02-transition-sml.png)](images/02-transition.png#lightbox)


### <a name="adapter"></a>어댑터

`ViewPager` 해당 데이터를 가져오고는 *어댑터*합니다. 어댑터의 작업을 만드는 것은 `View`하 여 표시 되는 `ViewPager`, 필요에 따라 제공 합니다. 다음 다이어그램에서는이 개념 &ndash; 어댑터를 만들고 채웁니다 `View`s를 제공 하 고는 `ViewPager`합니다. 로 `ViewPager` 사용자의 살짝 제스처를 감지는 제공 하기 위해 어댑터에서 적절 한 요청 `View` 표시 하려면: 

[![어댑터는 ViewPager에 이미지 및 이름을 연결 방법을 보여 주는 다이어그램](images/03-adapter-sml.png)](images/03-adapter.png#lightbox)

이 특정 예제의 각 `View` 에 전달 되기 전에 트리 이름과 트리 이미지에서 생성 되는 `ViewPager`합니다. 



### <a name="pager-indicator"></a>호출기 표시기

`ViewPager` 큰 데이터 집합을 표시 하는 데 사용 될 수 있습니다 (예를 들어 이미지 갤러리 포함 될 수 있습니다 이미지). 큰 데이터 집합을 탐색 하 여 사용자를 `ViewPager` 는 자주 함께 발생 한 *호출기 표시기* 문자열을 표시 하 합니다. 이미지 제목, 캡션 또는 데이터 집합 내에서 현재 보기의 위치를 단순히 문자열 수 있습니다. 

이 탐색 정보를 생성할 수 있는 두 개의 보기가: `PagerTabStrip` 및 `PagerTitleStrip.` 각 문자열의 맨 위쪽에 표시 됩니다는 `ViewPager`, 해당 데이터를 가져오는 각 및는 `ViewPager`의 한다는 항상 동기화를 유지 하므로 어댑터는 현재 표시 된 `View`합니다. 둘 간의 차이점은 `PagerTabStrip` 포함 하는 동안 "현재" 문자열에 대 한 시각적 표시기 `PagerTitleStrip` 없습니다 (표시 된 것 처럼이 스크린 샷에) 않습니다: 

[![PagerTitleStrip 및 PagerTabStrip TreePager 응용 프로그램의 스크린 샷](images/04-comparison-sml.png)](images/04-comparison.png#lightbox)

이 가이드에서는 immplement 하는 방법을 `ViewPager`, 어댑터 및 표시기 응용 프로그램 구성 요소 gestural 탐색을 지원 하기를 통합 합니다. 



## <a name="related-links"></a>관련 링크

- [TreePager (샘플)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
- [FlashCardPager (샘플)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
