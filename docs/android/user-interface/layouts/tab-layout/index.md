---
title: 레이아웃 탭된
description: Android에서 탭된 레이아웃의 개요
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/23/2017
ms.openlocfilehash: 4095944bb630637a2e761af18796dacdef17785c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="tabbed-layouts"></a>레이아웃 탭된


## <a name="overview"></a>개요

탭의 단순성 및 유용성 때문에 모바일 응용 프로그램에서 널리 사용 되는 사용자 인터페이스 패턴은입니다. 응용 프로그램에서 다양 한 화면 간의 탐색 하는 일관 되 고 쉬운 방법을 제공 합니다. Android에는 몇 가지 API의 탭된 인터페이스에 대 한에 있습니다. 

-   **TabHost** &ndash; 탭된 사용자 인터페이스를 만들기 위한 원래 API입니다. A `TabHost` 위젯 레이아웃에 추가 되 고 탭된 뷰에 대 한 컨테이너 역할을 합니다. 이 API는 이후 더 이상의 사용은 권장 되지 않습니다. 

-   **작업 모음** &ndash; 이것은 API의 일관 된 제공 하는 목표와 Android 3.0 (API 수준 11)에 도입 된 새로운 집합의 일부 탐색 및 인터페이스 보기 전환 합니다. 다시와 Android 2.2 (API 수준 8)에 이식 되었습니다는 [지원 라이브러리 Android v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)합니다. 

-   **PagerTabStrip** &ndash; current, 다음 및 이전 페이지의 수를 나타냅니다는 `ViewPager`합니다. `ViewPager` 통해서만 사용할 수 [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)합니다.
     에 대 한 자세한 내용은 `PagerTabStrip`, 참조 [ViewPager](~/android/user-interface/controls/view-pager/index.md)합니다.

-   **도구 모음** &ndash; `Toolbar` 대체 하는 새롭고 보다 유연한 작업 모음 구성 요소 `ActionBar`합니다. `Toolbar` Android 5.0 롤리팝에서 사용할 수 있는 이상 이전 버전의를 통해 Android에 사용할 수 있는 것도 하 고는 [v7 지원 라이브러리 Android](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet 패키지 합니다. 
    `Toolbar` 현재 Android 앱에서 사용 하도록 권장된 하는 작업 모음 구성 요소가입니다.
    자세한 내용은 참조 [도구 모음](~/android/user-interface/controls/tool-bar/index.md)합니다. 


이러한 API의 시각적으로 매우 다르고 서로 호환 되지 않습니다. 다음 이미지에서는 화면 `TabHost` 및 `ActionBar` -나란히: 

![스크린 샷을의 TabHost 왼쪽 및 오른쪽에 작업 모음](images/image01.png)

Android 3.0 (API 수준 11) 이후이 호환 되지 않는 API의 중요 한 UI 변경 인해 발생합니다. 이러한 UI 변경 중 하나는 [디자인 패턴 막대 작업](http://www.androidpatterns.com/uap_pattern/action-bar), 응용 프로그램에서 키 및 탐색 기능에 쉽게 액세스할 수를 제공 하기 위한 패턴입니다. `ActionBar` API이이 패턴을 지원 하기 위해 구현 되었습니다. 

`ActionBar` API는 더 간단 하 고 풀링할 때 더 나은 사용자 환경을 제공 합니다. Android 2.2에 다시 이식 되었습니다 하 고 Xamarin.Android 응용 프로그램에 대 한 기본 선택입니다. 

`TabHost` API Android의 모든 버전에서 호환 되는 데 더 많은 노력이 필요 있고 현재와 일치 하지 않습니다 [Android UI 지침](http://developer.android.com/design/index.html)합니다. 개발자가이 API를 사용 하 여 해서는 안 되며 Xamarin.Android 응용 프로그램에는 최신 작업 모음에 맞도록 해야 합니다. 



## <a name="actionbarsherlock"></a>ActionBarSherlock

작업 모음 API를 Android 2.2, 개발자에 게 타사 라이브러리를 사용할 수 있지만 작업 모음 API의 최신 모양과 느낌을 원하는 backported 되기 전에 [ActionBarSherlock](http://actionbarsherlock.com)합니다. ActionBarSherlock Android 지원 라이브러리의 확장인 설계 backport Android 2.x에 작업 모음 디자인 패턴입니다. ActionBarSherlock 네이티브 Android 3.0 이상를 실행할 때는 사용 자동으로 됩니다 `ActionBar` Android에서 제공 하는 API입니다. 이전 버전의 Android와 같은 최신의 모양과 느낌은 사용자 지정 구현을 ´ ֲ `ActionBar` API입니다. [ActionBarSherlock 구성 요소](https://www.nuget.org/packages/xamstore-XamarinActionBarSherlock/) 쉽게 ActionBarSherlock Xamarin.Android 응용 프로그램에 추가 합니다. 



## <a name="related-links"></a>관련 링크

- [TabHost 개요](tab-host.md)
- [TabHost 연습](~/android/user-interface/layouts/tab-layout/creating-a-tabbed-ui.md)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Android 지원 라이브러리 v7 AppCompat NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [v7 appcompat 라이브러리](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
