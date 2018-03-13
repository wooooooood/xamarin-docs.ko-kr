---
title: "TabHost 레이아웃 탭"
description: "이 문서에서는의 높은 수준의 개요를 제공 하는 여는 TabHost, 이전 API Xamarin.Android 응용 프로그램에서 탭된 레이아웃을 만드는 데 사용 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 77B890A4-27A6-41DF-81BA-22C6116A8FB2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/25/2017
ms.openlocfilehash: e27557c65d2b3049457640a3492d090c5fa26a43
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="tab-layout-with-tabhost"></a>TabHost 레이아웃 탭

_이 문서에서는의 높은 수준의 개요를 제공 하는 여는 TabHost, 이전 API Xamarin.Android 응용 프로그램에서 탭된 레이아웃을 만드는 데 사용 합니다._


## <a name="overview"></a>개요

> [!NOTE]
> `TabHost` Google에서 사용 되지 않은 오래 된 API입니다. 개발자는를 사용 하 여 탭된 응용 프로그램을 빌드하는 [작업 모음](~/android/user-interface/controls/action-bar.md)합니다. `ActionBar` Android의 모든 버전에서 사용할 수 있습니다. Android (API 수준 11) 3.0에서 처음 도입 하 고 Android 2.2 (API 수준 8) 및 Android 2.3 (API 수준 10)에서 다시 포팅된는 [V7 AppCompat 라이브러리](http://developer.android.com/tools/support-library/features.html#v7-appcompat), Xamarin.Android를 통해 제공 되는 [Xamarin Android 지원 라이브러리-V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) 패키지 합니다.

`TabHost` 가 오래 되어 원래 API는 Android 2.2 및 Android 2.3을 지원 해야 하 고 사용할 수 없는 Xamarin.Android 응용 프로그램에 가장 적합 한는 탭된 사용자 interfacesIt 만드는 **ActionBarSherlock**합니다.
다음 5 개의 구성 요소가와 관련 된 모든는 `TabHost` API:

-  **TabActivity** &ndash; 에 대 한 컨테이너 역할을 하는 특수 한 보기 된 `TabHost` (아래 설명 참조).

-  **TabHost** &ndash; 이것이 탭된 UI에 대 한 컨테이너입니다. 두 명의 자식 호스트: 탭 레이블 목록을 `FrameLayout` 탭 콘텐츠를 표시 하는입니다.

-  **TabWidget** &ndash; 이 컨트롤에서 각 탭에 대 한 탭 레이블 목록이 표시 됩니다는 `TabHost` 합니다. 각 탭에는 `TabHost` 으로 표시 됩니다는 `TabHost.TabSpec` 개체입니다. 이 개체는 각 탭에 대 한 메타 데이터를 포함합니다. 사용자가 탭을 선택 된 `TabHost` 적절 한 탭이 표시 하 여 선택 영역에 응답 합니다.

-  **FrameLayout** &ndash; 탭된 UI 있어야는 `FrameLayout` 내에 포함 된 한 `TabHost`합니다. 탭에 대 한 내용을 표시 하려면 사용 됩니다.

-  **활동이 나 보기** &ndash; 작업 또는 보기에 표시 탭을 선택 하는 경우는 `FrameLayout`합니다.

다음 다이어그램에서는 이러한 모든 구성이 요소 간의 관계 함께 보여 줍니다.

![TabHost 내의 TabWidget 내 프레임 레이아웃을 보여 주는 다이어그램](tab-host-images/image03.png)

탭 내용 활동이 나 보기 중 하나를 수 있습니다. 뷰는 상대적으로 간단 하 고 간단 있지만 활동에 관련 되지 않은 코드 co habitating 많이 발생할 수 있습니다. 고려 사항 및를 유지 하기는 너무 커지면된 클래스 저하 분리 됩니다. 반면, 활동 시스템 리소스가 필요 하지만 모듈식 방법을 자신의 고유 클래스에 캡슐화 된 각 탭에 대 한 논리입니다.


## <a name="summary"></a>요약

이 문서에서는 이전의 높은 수준의 구성 요소 `TabHost` 서로 함께 및 Android에 대 한 API입니다.



## <a name="related-links"></a>관련 링크

- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [TabHost](https://developer.xamarin.com/api/type/Android.Widget.TabHost/)
- [TabHost.TabSpec](https://developer.xamarin.com/api/type/Android.Widget.TabHost+TabSpec/)
- [TabWidget](https://developer.xamarin.com/api/type/Android.Widget.TabWidget/)
- [TabActivity](https://developer.xamarin.com/api/type/Android.App.TabActivity/)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Android 지원 라이브러리 v7 AppCompat NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [v7 appcompat 라이브러리](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
