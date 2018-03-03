---
title: Toolbar
description: "도구 모음을 기본 작업 모음 보다 더 많은 유연성을 제공 하는 작업 모음 구성 요소: 앱에서 아무 곳 이나 배치 될 수, 크기는 변경할 수 있으며 다른 앱의 테마 색 구성표를 사용할 수 있습니다. 또한 각 응용 프로그램 화면에는 여러 도구 모음이 있을 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 22EE5FBD-3240-4308-AF76-EF45D72936DE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: cf2211a572d45b7c29018d00f36cb8408484483f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="toolbar"></a>Toolbar

_도구 모음을 기본 작업 모음 보다 더 많은 유연성을 제공 하는 작업 모음 구성 요소: 앱에서 아무 곳 이나 배치 될 수, 크기는 변경할 수 있으며 다른 앱의 테마 색 구성표를 사용할 수 있습니다. 또한 각 응용 프로그램 화면에는 여러 도구 모음이 있을 수 있습니다._


<a name="overview" />
 
## <a name="overview"></a>개요

모든 Android 활동의 주요 디자인 요소는 한 *작업 모음*합니다. 작업 모음에는 Android 응용 프로그램의 브랜딩 탐색, 검색, 메뉴, 사용 되는 UI 구성 요소입니다. 작업 모음 Android 5.0 롤리팝 이전 Android 버전에서 (라고도 *앱 바*)는이 기능을 제공 하기 위한 권장된 구성 요소입니다. 

`Toolbar` (Android 5.0 롤리팝에 도입 된) 위젯 작업 모음 인터페이스의 개념으로 생각할 수 있습니다 &ndash; 작업 모음을 대체 하기 위한 것입니다. `Toolbar` 앱 레이아웃에서 어디서 나 사용할 수 있고 그것이 작업 모음 보다 훨씬 더 사용자 지정할 수 있습니다. 다음 스크린 샷에서 사용자 지정에서는 `Toolbar` 이 가이드에서 만든 예제: 

[![예제 스크린 샷 편집, 도구 모음에 저장 하 고 오버플로 메뉴 항목](images/01-toolbar-sml.png)](images/01-toolbar.png)

몇 가지 중요 한 차이점이 간에 `Toolbar` 및 작업 모음: 

-   A `Toolbar` 사용자 인터페이스에서 임의의 위치에 올 수 있습니다.

-   여러 도구 모음 같은 화면에 표시할 수 있습니다.

-   조각을 사용 되는 경우 각 조각은 고유한 `Toolbar`합니다. 

-   A `Toolbar` 화면 너비를 부분에 걸쳐 하도록 구성할 수 있습니다. 

-   때문에 `Toolbar` 바인딩되지 않은 활동의 창 장식의 색 구성표를 시각적으로 고유한 색 구성표를 가질 수 있습니다. 

-   작업 모음 달리는 `Toolbar` 왼쪽에 있는 아이콘에 포함 되지 않습니다. 오른쪽의 메뉴에는 공간을 적게 사용 됩니다. 

-   `Toolbar` 높이 조정할 수 있습니다. 

-   내 다른 보기에 포함할 수는 `Toolbar`합니다. 

A `Toolbar` 다음 요소 중 하나 이상 포함 될 수 있습니다. 

-   탐색 단추

-   브랜드 로고 이미지

-   제목 및 부제목

-   사용자 지정 보기

-   동작 메뉴

-   오버플로 메뉴

Google의 [자료 디자인 지침](https://material.google.com/) 다른 모양을 (에 의존 하지 않고 전적으로 응용 프로그램 아이콘 및 제목을) 앱을 제공 하려면 이러한 요소를 활용 하는 것이 좋습니다. 

이 가이드는 가장 일반적으로 사용 되는 소개 `Toolbar` 시나리오:

-   활동의 기본 작업 모음으로 대체 한 `Toolbar`합니다. 

-   두 번째 추가 `Toolbar` 활동입니다.

-   사용 하는 **지원 라이브러리 Android v7 AppCompat** 라이브러리 (라고 *AppCompat* 이 가이드의 나머지 부분에서)를 배포 하 `Toolbar` 이전 버전의 Android 합니다. 

 
<a name="requirements" />
 
## <a name="requirements"></a>요구 사항

`Toolbar` 이상에서 Android 5.0 롤리팝 (API 21)를 사용할 수 있습니다. Android 5.0 이전 해제 Android 대상으로 하는 경우 사용 된 [Android 지원 라이브러리 v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/), 이전 버전과 호환성을 제공 하는 `Toolbar` NuGet 패키지를 지원 합니다. 
[도구 모음 호환성](~/android/user-interface/controls/tool-bar/toolbar-compatibility.md) 이 라이브러리를 사용 하는 방법을 설명 합니다. 




## <a name="related-links"></a>관련 링크

- [롤리팝 도구 모음 (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat 도구 모음 (샘플)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
