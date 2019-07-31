---
title: 도구 모음
description: 도구 모음은 기본 작업 모음 보다 더 많은 유연성을 제공 하는 작업 모음 구성 요소입니다 .이 구성 요소는 앱의 어느 위치에 나 배치 될 수 있으며, 크기를 변경할 수 있으며, 앱의 테마와 다른 색 구성표를 사용할 수 있습니다. 또한 각 앱 화면에는 여러 도구 모음이 있을 수 있습니다.
ms.prod: xamarin
ms.assetid: 22EE5FBD-3240-4308-AF76-EF45D72936DE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 25287e5aa52eeac712f93c3973e02c7e14c89a78
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645094"
---
# <a name="toolbar"></a>도구 모음

_도구 모음은 기본 작업 모음 보다 더 많은 유연성을 제공 하는 작업 모음 구성 요소입니다 .이 구성 요소는 앱의 어느 위치에 나 배치 될 수 있으며, 크기를 변경할 수 있으며, 앱의 테마와 다른 색 구성표를 사용할 수 있습니다. 또한 각 앱 화면에는 여러 도구 모음이 있을 수 있습니다._

 
## <a name="overview"></a>개요

모든 Android 작업의 핵심 디자인 요소는 *작업 모음*입니다. 작업 모음은 Android 앱의 탐색, 검색, 메뉴 및 브랜딩에 사용 되는 UI 구성 요소입니다. Android 5.0 롤리팝 이전 Android 버전에서는이 기능을 제공 하기 위한 작업 모음 ( *앱 바*라고도 함)이 권장 구성 요소 였습니다. 

위젯 `Toolbar` (Android 5.0 롤리팝에 도입 됨)은 작업 모음 인터페이스 &ndash; 의 일반화로 간주할 수 있습니다 .이는 작업 모음을 대체 하기 위한 것입니다. 는 `Toolbar` 앱 레이아웃의 어디에서 나 사용할 수 있으며, 작업 모음 보다 훨씬 더 쉽게 사용자 지정할 수 있습니다. 다음 스크린샷에는이 가이드에서 `Toolbar` 만든 사용자 지정 된 예제가 나와 있습니다. 

[![편집, 저장 및 오버플로 메뉴 항목이 있는 도구 모음의 예 스크린샷](images/01-toolbar-sml.png)](images/01-toolbar.png#lightbox)

와 작업 모음 사이에는 `Toolbar` 몇 가지 중요 한 차이점이 있습니다. 

-   는 `Toolbar` 사용자 인터페이스의 어느 위치에 나 배치할 수 있습니다.

-   여러 도구 모음이 동일한 화면에 표시 될 수 있습니다.

-   조각을 사용 하는 경우 각 조각에는 고유한 `Toolbar`가 있을 수 있습니다. 

-   는 `Toolbar` 화면의 일부 너비로만 구성 될 수 있습니다. 

-   는 `Toolbar` 활동의 창 décor 색 구성표에 바인딩되지 않으므로 시각적으로 고유한 색 구성표가 있을 수 있습니다. 

-   작업 표시줄과 달리의 왼쪽에 `Toolbar` 는 아이콘이 포함 되지 않습니다. 오른쪽의 메뉴에는 더 작은 공간이 사용 됩니다. 

-   `Toolbar` 높이를 조정할 수 있습니다. 

-   다른 뷰는 내에 `Toolbar`포함 될 수 있습니다. 

에 `Toolbar` 는 다음 요소 중 하나 이상이 포함 될 수 있습니다. 

-   탐색 단추

-   브랜드 로고 이미지

-   제목 및 부제목

-   사용자 지정 보기

-   작업 메뉴

-   오버플로 메뉴

Google의 [재질 디자인 지침은](https://material.google.com/) 이러한 요소를 활용 하 여 응용 프로그램 아이콘 및 제목만을 사용할 수 있는 것이 아니라 앱을 고유 하 게 만드는 것이 좋습니다. 

이 가이드에서는 가장 일반적으로 사용 되 `Toolbar` 는 시나리오에 대해 설명 합니다.

-   활동의 기본 작업 모음을 `Toolbar`로 바꾸기 

-   활동에 두 `Toolbar` 번째를 추가 합니다.

-   **Android Support library v7 appcompat** 라이브러리 (이 가이드의 나머지 부분에서는 *appcompat* 이라고 함)를 사용 하 여 이전 `Toolbar` 버전의 android에서 배포 합니다. 

 
 
## <a name="requirements"></a>요구 사항

`Toolbar`Android 5.0 롤리팝 (API 21) 이상에서 사용할 수 있습니다. Android 5.0 이전 버전의 android 릴리스를 대상으로 하는 경우 NuGet 패키지에서 이전 버전과 호환 되 `Toolbar` 는 지원을 제공 하는 [android 지원 라이브러리 v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)을 사용 합니다. 
[도구 모음 호환성](~/android/user-interface/controls/tool-bar/toolbar-compatibility.md) 에서는이 라이브러리를 사용 하는 방법을 설명 합니다. 




## <a name="related-links"></a>관련 링크

- [롤리팝 도구 모음 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat 도구 모음 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
