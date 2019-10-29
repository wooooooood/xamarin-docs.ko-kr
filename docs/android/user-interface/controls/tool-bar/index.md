---
title: ToolBar
description: 도구 모음은 기본 작업 모음 보다 더 많은 유연성을 제공 하는 작업 모음 구성 요소입니다 .이 구성 요소는 앱의 어느 위치에 나 배치 될 수 있으며, 크기를 변경할 수 있으며, 앱의 테마와 다른 색 구성표를 사용할 수 있습니다. 또한 각 앱 화면에는 여러 도구 모음이 있을 수 있습니다.
ms.prod: xamarin
ms.assetid: 22EE5FBD-3240-4308-AF76-EF45D72936DE
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 3f4a9393d8ef6ef54ba3dba29f1109080f1e321a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029097"
---
# <a name="toolbar"></a>ToolBar

_도구 모음은 기본 작업 모음 보다 더 많은 유연성을 제공 하는 작업 모음 구성 요소입니다 .이 구성 요소는 앱의 어느 위치에 나 배치 될 수 있으며, 크기를 변경할 수 있으며, 앱의 테마와 다른 색 구성표를 사용할 수 있습니다. 또한 각 앱 화면에는 여러 도구 모음이 있을 수 있습니다._

## <a name="overview"></a>개요

모든 Android 작업의 핵심 디자인 요소는 *작업 모음*입니다. 작업 모음은 Android 앱의 탐색, 검색, 메뉴 및 브랜딩에 사용 되는 UI 구성 요소입니다. Android 5.0 롤리팝 이전 Android 버전에서는이 기능을 제공 하기 위한 작업 모음 ( *앱 바*라고도 함)이 권장 구성 요소 였습니다. 

`Toolbar` 위젯 (Android 5.0 롤리팝에서 도입 됨)은 작업 모음을 대체 하기 위한 것 &ndash; 작업 모음 인터페이스의 일반화로 간주할 수 있습니다. `Toolbar`는 앱 레이아웃의 어디에서 나 사용할 수 있으며, 작업 모음 보다 훨씬 더 쉽게 사용자 지정할 수 있습니다. 다음 스크린샷에서는이 가이드에서 만든 사용자 지정 `Toolbar` 예제를 보여 줍니다. 

[![편집, 저장 및 오버플로 메뉴 항목이 있는 도구 모음의 예제 스크린샷](images/01-toolbar-sml.png)](images/01-toolbar.png#lightbox)

`Toolbar`와 작업 모음 사이에는 몇 가지 중요 한 차이점이 있습니다. 

- `Toolbar`은 사용자 인터페이스의 모든 위치에 배치할 수 있습니다.

- 여러 도구 모음이 동일한 화면에 표시 될 수 있습니다.

- 조각을 사용 하는 경우 각 조각에는 고유한 `Toolbar`있을 수 있습니다. 

- 화면 크기의 일부에만 적용 되도록 `Toolbar`를 구성할 수 있습니다. 

- `Toolbar`는 활동의 창 décor 색 구성표에 바인딩되지 않으므로 시각적으로 고유한 색 구성표가 있을 수 있습니다. 

- 작업 표시줄과 달리 `Toolbar` 왼쪽에는 아이콘이 포함 되지 않습니다. 오른쪽의 메뉴에는 더 작은 공간이 사용 됩니다. 

- `Toolbar` 높이를 조정할 수 있습니다. 

- 다른 보기는 `Toolbar`내에 포함 될 수 있습니다. 

`Toolbar`에는 다음 요소 중 하나 이상이 포함 될 수 있습니다. 

- 탐색 단추

- 브랜드 로고 이미지

- 제목 및 부제목

- 사용자 지정 보기

- 작업 메뉴

- 오버플로 메뉴

Google의 [재질 디자인 지침은](https://material.google.com/) 이러한 요소를 활용 하 여 응용 프로그램 아이콘 및 제목만을 사용할 수 있는 것이 아니라 앱을 고유 하 게 만드는 것이 좋습니다. 

이 가이드에서는 가장 일반적으로 사용 되는 `Toolbar` 시나리오를 다룹니다.

- 활동의 기본 작업 모음을 `Toolbar`바꿉니다. 

- 활동에 두 번째 `Toolbar` 추가

- **Android Support library V7 appcompat** Library (이 가이드의 나머지 부분에서는 *appcompat* 이라고 함)를 사용 하 여 이전 버전의 android에 `Toolbar`를 배포 합니다. 

## <a name="requirements"></a>요구 사항

`Toolbar`는 Android 5.0 롤리팝 (API 21) 이상에서 사용할 수 있습니다. Android 5.0 이전 버전의 Android 릴리스를 대상으로 하는 경우 NuGet 패키지에서 이전 버전과 호환 되는 `Toolbar` 지원을 제공 하는 [Android 지원 라이브러리 V7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)을 사용 합니다. 
[도구 모음 호환성](~/android/user-interface/controls/tool-bar/toolbar-compatibility.md) 에서는이 라이브러리를 사용 하는 방법을 설명 합니다. 

## <a name="related-links"></a>관련 링크

- [롤리팝 도구 모음 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat 도구 모음 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
