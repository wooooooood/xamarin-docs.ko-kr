---
title: 레이아웃 탭된
description: Android에서 탭된 레이아웃의 개요
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/08/2017
ms.openlocfilehash: 53ed5f91583d43839e96388194aea8c0d6ac5315
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2018
---
# <a name="tabbed-layouts"></a>레이아웃 탭된


## <a name="overview"></a>개요

탭의 단순성 및 유용성 때문에 모바일 응용 프로그램에서 널리 사용 되는 사용자 인터페이스 패턴은입니다. 응용 프로그램에서 다양 한 화면 간의 탐색 하는 일관 되 고 쉬운 방법을 제공 합니다. Android에는 몇 가지 API의 탭된 인터페이스에 대 한에 있습니다. 

-   **작업 모음** &ndash; 이것은 API의 일관 된 제공 하는 목표와 Android 3.0 (API 수준 11)에 도입 된 새로운 집합의 일부 탐색 및 인터페이스 보기 전환 합니다. 다시와 Android 2.2 (API 수준 8)에 이식 되었습니다는 [지원 라이브러리 Android v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)합니다. 

-   **PagerTabStrip** &ndash; current, 다음 및 이전 페이지의 수를 나타냅니다는 `ViewPager`합니다. `ViewPager` 통해서만 사용할 수 [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)합니다.
     에 대 한 자세한 내용은 `PagerTabStrip`, 참조 [ViewPager](~/android/user-interface/controls/view-pager/index.md)합니다.

-   **도구 모음** &ndash; `Toolbar` 대체 하는 새롭고 보다 유연한 작업 모음 구성 요소 `ActionBar`합니다. `Toolbar` Android 5.0 롤리팝에서 사용할 수 있는 이상 이전 버전의를 통해 Android에 사용할 수 있는 것도 하 고는 [v7 지원 라이브러리 Android](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet 패키지 합니다. 
    `Toolbar` 현재 Android 앱에서 사용 하도록 권장된 하는 작업 모음 구성 요소가입니다.
    자세한 내용은 참조 [도구 모음](~/android/user-interface/controls/tool-bar/index.md)합니다. 



## <a name="related-links"></a>관련 링크

- [Tab 키 재료 디자인-](https://material.io/guidelines/components/tabs.html)- [작업 모음](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Android 지원 라이브러리 v7 AppCompat NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [v7 appcompat 라이브러리](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
