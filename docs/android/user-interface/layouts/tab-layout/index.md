---
title: 탭된 레이아웃
description: Android에서 탭된 레이아웃의 개요
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/08/2017
ms.openlocfilehash: 5d88ffb44d12ee142314c74ca8e749164cbfe3b9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105963"
---
# <a name="tabbed-layouts"></a>탭된 레이아웃


## <a name="overview"></a>개요

탭에는 해당 단순 성과 유용성 때문에 모바일 응용 프로그램에서 널리 사용 되는 사용자 인터페이스 패턴은입니다. 응용 프로그램에서 다양 한 화면 간의 탐색에 일관 되 고 간편한 방법을 제공 합니다. Android에는 몇 가지 API의 탭된 인터페이스에 있습니다. 

-   **작업 모음이** &ndash; 이러한 부분은 API의 일관성을 제공 하는 목표를 사용 하 여 Android 3.0 (API 레벨 11)에 도입 된 새 집합을 탐색 및 보기 전환 인터페이스입니다. 다시 사용 하 여 (API 수준 8) 2.2를 Android로 이식 되었으며 합니다 [Android 지원 라이브러리 v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)합니다. 

-   **PagerTabStrip** &ndash; 의 현재, 다음 및 이전 페이지 수를 나타냅니다는 `ViewPager`합니다. `ViewPager` 통해서만 사용할 수 [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)합니다.
     에 대 한 자세한 내용은 `PagerTabStrip`를 참조 하세요 [ViewPager](~/android/user-interface/controls/view-pager/index.md)합니다.

-   **도구 모음** &ndash; `Toolbar` 대체 하는 새롭고 보다 유연한 작업 표시줄 구성 요소 `ActionBar`합니다. `Toolbar` Android 5.0 Lollipop에서 사용할 수 있는 이상 하며 이전 버전의 Android 통해 사용할 수 이기도 합니다 [Android 지원 라이브러리 v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet 패키지. 
    `Toolbar` 현재 Android 앱에서 사용 하도록 권장 되는 작업 표시줄 구성 요소가입니다.
    자세한 내용은 [도구 모음](~/android/user-interface/controls/tool-bar/index.md)합니다. 



## <a name="related-links"></a>관련 링크

- [재질 디자인-탭](https://material.io/guidelines/components/tabs.html)- [작업 모음](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Android 지원 라이브러리 v7 AppCompat NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [v7 appcompat 라이브러리](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
