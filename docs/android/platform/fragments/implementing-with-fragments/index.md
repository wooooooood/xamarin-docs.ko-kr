---
title: 조각 구현 - 연습
description: 이 문서에서는 Xamarin.Android 애플리케이션을 개발할 때 조각을 사용하는 방법을 안내합니다.
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/26/2018
ms.openlocfilehash: b601fc37cc75dcd43c3688de8d302f0a47a06b35
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027413"
---
# <a name="implementing-fragments---walkthrough"></a>조각 구현 - 연습

_조각은 화면 크기가 다양한 디바이스를 대상으로 하는 Android 앱의 복잡성을 해결하는 데 도움이 될 수 있는 독립적인 모듈식 구성 요소입니다. 이 문서에서는 Xamarin.Android 애플리케이션을 개발할 때 조각을 만들고 사용하는 방법을 안내합니다._

## <a name="overview"></a>개요

이 섹션에서는 Xamarin.Android 애플리케이션에서 조각을 만들고 사용하는 방법을 안내합니다. 이 애플리케이션은 윌리엄 셰익스피어가 쓴 희곡의 제목을 한 목록에 표시합니다. 사용자가 희곡 제목을 누르면 앱이 별도의 작업에서 해당 희곡의 인용구를 표시합니다.

[![Android 휴대폰에서 세로 모드로 실행되는 앱](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

휴대폰이 가로 모드로 회전되면 앱 모양이 변경됩니다. 즉, 희곡 목록과 인용구가 모두 동일한 작업에 표시됩니다. 희곡을 선택하면 인용구가 동일한 작업에서 표시됩니다.

[![Android 휴대폰에서 가로 모드로 실행되는 앱](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

마지막으로 태블릿에서 앱을 실행하는 경우:

[![Android 태블릿에서 실행되는 앱](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

이 샘플 애플리케이션은 조각과 [대체 레이아웃](/xamarin/android/app-fundamentals/resources-in-android/alternate-resources)을 사용하여 최소한의 코드 변경으로 다양한 폼 팩터 및 방향에 따라 쉽게 조정될 수 있습니다.

애플리케이션용 데이터는 애플리케이션에서 C# 문자열 배열로 하드 코딩된 두 개의 문자열 배열에 존재합니다. 각 배열은 하나의 조각에 대한 데이터 원본 역할을 합니다.  한 배열에는 셰익스피어가 쓴 희곡의 이름이 보관되고 다른 배열에는 해당 희곡의 인용구가 보관됩니다. 앱이 시작되면 `ListFragment`에 희곡 이름이 표시됩니다. 사용자가 `ListFragment`에서 희곡을 클릭하면 앱이 인용구를 표시하는 다른 작업을 시작합니다.

앱의 사용자 인터페이스는 세로 모드와 가로 모드 두 가지 레이아웃으로 구성됩니다. 런타임에 Android는 디바이스의 방향에 따라 로드할 레이아웃을 결정하고 렌더링할 작업에 해당 레이아웃을 제공합니다. 사용자가 클릭하고 데이터를 표시하는 데 대응하는 모든 논리는 조각에 포함됩니다. 앱의 작업은 조각을 호스트하는 컨테이너에만 존재합니다.

이 연습은 두 개의 가이드로 구분됩니다. [첫 번째 부분은 애플리케이션의 코어 부분에 중점을 둡니다](./walkthrough.md). 단일 레이아웃 집합(세로 모드에 최적화)이 생성되며, 두 개의 조각과 두 가지 작업이 함께 생성됩니다.

1. `MainActivity` &nbsp; 앱의 시작 작업입니다.
1. `TitlesFragment` &nbsp; 이 조각은 윌리엄 셰익스피어가 쓴 희곡의 제목 목록을 표시합니다. 이 조각은 `MainActivity`에 의해 호스트됩니다.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` `TitlesFragment`에서 사용자의 희곡 선택에 대한 응답으로 `PlayQuoteActivity`를 시작합니다.
1. `PlayQuoteFragment` &nbsp; 이 조각은 윌리엄 셰익스피어의 희곡 중 하나의 인용구를 표시합니다. 이 조각은 `PlayQuoteActivity`에 의해 호스트됩니다.

[이 연습의 두 번째 부분](./walkthrough-landscape.md)에서는 화면에 두 조각을 모두 표시하는 대체 레이아웃(가로 모드에 최적화)을 추가하는 방법을 설명합니다. 또한 앱이 화면에 동시에 표시되는 조각 수에 대한 동작을 조정할 수 있도록 코드를 약간 변경합니다.

## <a name="related-links"></a>관련 링크

- [FragmentsWalkthrough(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fragmentswalkthrough)
- [디자이너 개요](~/android/user-interface/android-designer/index.md)
- [조각 구현](https://developer.android.com/guide/topics/fundamentals/fragments.html)
- [지원 패키지](https://developer.android.com/sdk/compatibility-library.html)
