---
title: 조각 구현-연습
description: 이 문서에서는 Xamarin Android 응용 프로그램을 개발 하는 데 조각을 사용 하는 방법을 안내 합니다.
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
ms.openlocfilehash: e5a09c216f0def71efb1c3ddc0ed18672663bdfe
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68643612"
---
# <a name="implementing-fragments---walkthrough"></a>조각 구현-연습

_조각은 다양 한 화면 크기를 사용 하는 장치를 대상으로 하는 Android 앱의 복잡성을 해결 하는 데 도움이 될 수 있는 자체 포함 모듈식 구성 요소입니다. 이 문서에서는 Xamarin Android 응용 프로그램을 개발할 때 조각을 만들고 사용 하는 방법을 안내 합니다._

## <a name="overview"></a>개요

이 섹션에서는 Xamarin Android 응용 프로그램에서 조각을 만들고 사용 하는 방법을 안내 합니다. 이 응용 프로그램은 William 셰익스피어에 의해 목록에 여러 개의 재생 된 제목을 표시 합니다. 사용자가 play의 제목을 누르면 앱이 별도의 작업에서 해당 재생의 따옴표를 표시 합니다.

[![세로 모드로 Android 휴대폰에서 실행 되는 앱](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

전화가 가로 모드로 회전 되 면 앱의 모양이 변경 됩니다. 즉, 재생 목록과 견적이 모두 동일한 활동에 표시 됩니다. 재생을 선택 하면 해당 견적이 동일한 활동에 표시 됩니다.

[![가로 모드로 Android 휴대폰에서 실행 되는 앱](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

마지막으로 태블릿에서 앱을 실행 하는 경우:

[![Android 태블릿에서 실행 되는 앱](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

이 샘플 응용 프로그램은 조각 및 [대체 레이아웃](/xamarin/android/app-fundamentals/resources-in-android/alternate-resources)을 사용 하 여 코드를 최소한으로 변경 하 여 다양 한 폼 팩터 및 방향에 쉽게 적용할 수 있습니다.

응용 프로그램의 데이터는 응용 프로그램에서 C# 문자열 배열로 하드 코딩 된 두 개의 문자열 배열에 존재 합니다. 각 배열은 하나의 조각에 대 한 데이터 소스 역할을 합니다.  한 배열에서 셰익스피어의 일부 재생 이름을 보유 하 고 다른 배열에는 해당 play의 견적이 포함 됩니다. 앱이 시작 되 면에 `ListFragment`재생 이름이 표시 됩니다. 사용자가에서 `ListFragment`재생을 클릭 하면 앱이 견적을 표시 하는 다른 작업을 시작 합니다.

앱에 대 한 사용자 인터페이스는 세로 및 가로 모드의 두 가지 레이아웃으로 구성 됩니다. 런타임에 Android는 장치의 방향에 따라 로드할 레이아웃을 결정 하 고 렌더링할 작업에 해당 레이아웃을 제공 합니다. 사용자가 클릭 하 고 데이터를 표시 하는 데 대응 하는 모든 논리는 조각에 포함 됩니다. 앱의 작업은 조각을 호스트 하는 컨테이너에만 존재 합니다.

이 연습은 두 개의 가이드로 구분 됩니다. [첫 번째 부분은](./walkthrough.md) 응용 프로그램의 핵심 부분에 중점을 둡니다. 단일 레이아웃 집합 (세로 모드에 맞게 최적화 됨)이 생성 되며, 두 개의 조각과 두 가지 작업이 함께 생성 됩니다.

1. `MainActivity`&nbsp; 앱에 대 한 시작 작업입니다.
1. `TitlesFragment`&nbsp; 이 조각은 William 셰익스피어에서 작성 한 재생 제목의 목록을 표시 합니다. 에서 `MainActivity`호스트 됩니다.
1. `PlayQuoteActivity`는 사용자가 `PlayQuoteActivity` 재생을 선택 하는 것에 대 한 `TitlesFragment`응답으로를 시작 합니다. &nbsp; `TitlesFragment`
1. `PlayQuoteFragment`&nbsp; 이 조각에는 play by William 셰익스피어의 견적이 표시 됩니다. 에서 `PlayQuoteActivity`호스트 됩니다.

[이 연습의 두 번째 부분](./walkthrough-landscape.md) 에서는 화면에 두 조각을 모두 표시 하는 대체 레이아웃 (가로 모드에 대해 최적화 됨)을 추가 하는 방법에 대해 설명 합니다. 또한 앱이 화면에 동시에 표시 되는 조각 수에 대 한 동작을 조정할 수 있도록 코드에 약간의 사소한 코드 변경이 적용 됩니다.

## <a name="related-links"></a>관련 링크

- [FragmentsWalkthrough (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fragmentswalkthrough)
- [디자이너 개요](~/android/user-interface/android-designer/index.md)
- [조각 구현](https://developer.android.com/guide/topics/fundamentals/fragments.html)
- [지원 패키지](https://developer.android.com/sdk/compatibility-library.html)
