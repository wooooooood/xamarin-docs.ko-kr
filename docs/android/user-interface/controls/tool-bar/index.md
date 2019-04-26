---
title: ToolBar
description: '도구 모음에는 기본 작업 모음 보다 더 많은 유연성을 제공 하는 작업 표시줄 구성 요소: 앱에서 아무 곳 이나 배치 될 수, 크기를 변경할 수 있으며 다른 앱의 테마 색 구성표를 사용할 수 있습니다. 또한 각 앱 화면에는 여러 도구 모음이 있을 수 있습니다.'
ms.prod: xamarin
ms.assetid: 22EE5FBD-3240-4308-AF76-EF45D72936DE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 2c9de4058fdaee53671e65f49ad95c3af5e127d6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61082857"
---
# <a name="toolbar"></a>ToolBar

_도구 모음에는 기본 작업 모음 보다 더 많은 유연성을 제공 하는 작업 표시줄 구성 요소: 앱에서 아무 곳 이나 배치 될 수, 크기를 변경할 수 있으며 다른 앱의 테마 색 구성표를 사용할 수 있습니다. 또한 각 앱 화면에는 여러 도구 모음이 있을 수 있습니다._

 
## <a name="overview"></a>개요

모든 Android 활동의 주요 디자인 요소를은 *작업 모음*합니다. 작업 모음에는 Android 앱에서 브랜딩 메뉴, 탐색 및 검색에 사용 되는 UI 구성 요소입니다. 작업 모음 Android 5.0 Lollipop 이전 Android 버전에서 (라고도 합니다 *앱 바*)는이 기능을 제공 하는 데 권장 되는 구성 요소입니다. 

합니다 `Toolbar` (Android 5.0 Lollipop에 도입 됨) 하는 위젯을 작업 모음 인터페이스의 일반화로 생각할 수 있습니다 &ndash; 작업 모음을 대체 하기 위한 것입니다. `Toolbar` 인 앱 레이아웃에서는 어디서 나 사용할 수 있으며 작업 모음 보다 훨씬 더 사용자 지정 가능한 것입니다. 다음 스크린샷은 사용자 지정 방법을 보여 줍니다 `Toolbar` 이 가이드에서 만든 예제: 

[![예제 스크린샷 편집을 사용 하 여 도구 모음을 저장 하 고 오버플로 메뉴 항목](images/01-toolbar-sml.png)](images/01-toolbar.png#lightbox)

몇 가지 중요 한 차이점을 가지는 `Toolbar` 및 작업 표시줄: 

-   `Toolbar` 어디서 나 사용자 인터페이스에 배치할 수 있습니다.

-   동일한 화면의 여러 도구 모음을 표시할 수 있습니다.

-   조각을 사용 하는 경우 각 조각은 고유한 `Toolbar`합니다. 

-   `Toolbar` 화면 너비를 부분에 걸쳐 구성할 수 있습니다. 

-   때문에 `Toolbar` 바인딩되지 않은 활동의 창 장식의 색 구성표를 시각적으로 고유한 색 구성표를 가질 수 있습니다. 

-   작업 모음 달리는 `Toolbar` 왼쪽에서 아이콘을 다루지 않습니다. 오른쪽 메뉴 더 적은 공간을 사용합니다. 

-   `Toolbar` 높이 조정할 수 있습니다. 

-   내부 다른 보기에 포함할 수는 `Toolbar`합니다. 

`Toolbar` 다음 요소 중 하나 이상을 포함할 수 있습니다. 

-   탐색 단추

-   브랜드 로고 이미지

-   제목 및 부제목

-   사용자 지정 보기

-   작업 메뉴

-   오버플로 메뉴

Google [재료 디자인 지침](https://material.google.com/) 고유 확인 (에 의존 하지 않고 전적으로 응용 프로그램 아이콘 및 제목) 앱을 제공 하려면 이러한 요소를 활용 하는 것이 좋습니다. 

이 가이드는 가장 일반적으로 사용 되는 설명 `Toolbar` 시나리오:

-   사용 하 여 활동의 기본 작업 모음 바꾸기는 `Toolbar`합니다. 

-   두 번째 추가 `Toolbar` 활동입니다.

-   사용 하 여 합니다 **Android 지원 라이브러리 v7 AppCompat** 라이브러리 (라고 *AppCompat* 이 가이드의 나머지 부분에서) 배포 `Toolbar` 이전 버전의 Android에서. 

 
 
## <a name="requirements"></a>요구 사항

`Toolbar` 이 제품은 (API 21) Android 5.0 Lollipop 이상입니다. Android 5.0 이전 릴리스 Android를 대상으로 하는 경우 사용 합니다 [Android 지원 라이브러리 v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/), 이전 버전과 호환성을 제공 하는 `Toolbar` NuGet 패키지를 지원 합니다. 
[도구 모음 호환성](~/android/user-interface/controls/tool-bar/toolbar-compatibility.md) 이 라이브러리를 사용 하는 방법에 설명 합니다. 




## <a name="related-links"></a>관련 링크

- [롤리팝 도구 모음 (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat 도구 모음 (샘플)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
