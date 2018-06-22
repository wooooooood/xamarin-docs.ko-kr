---
title: 조각-연습 구현
description: 이 문서 조각 Xamarin.Android 응용 프로그램을 개발을 사용 하는 방법을 안내 합니다.
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: 92c68298d7abd2570efd89e12d7cfb6364e90972
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33798358"
---
# <a name="implementing-fragments---walkthrough"></a>조각-연습 구현

_조각은 다양 한 화면 크기와 장치를 대상으로 하는 Android 앱의 복잡성을 해결 하는 데 도움이 되는 자체 포함 된, 모듈형 구성 요소입니다. 이 문서를 만들고 Xamarin.Android 응용 프로그램을 개발 하는 경우 조각을 사용 하는 방법을 안내 합니다._

## <a name="overview"></a>개요

이 섹션에서는 만들고 조각 Xamarin.Android 응용 프로그램에서 사용 하는 방법을 단계별로 설명 합니다. 이 응용 프로그램 목록에 William 셰익스피어 하 여 여러 재생 타이틀을 표시 됩니다. 사용자는 재생의 제목에 누르면 다음 응용 프로그램에에서 표시 됩니다 해당 play에서 따옴표를 별도 동작:

[![Android 휴대폰 세로 모드에서 실행 중인 앱](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

전화를 가로 모드로 회전할 때 앱의 모양을 변경 됩니다: 재생 목록을 따옴표 모두 동일한 동작에 나타납니다. 재생 옵션을 선택 하면 견적 동일한 동작에서 표시 됩니다.

[![Android 휴대폰에서 가로 모드에서 실행 중인 앱](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

마지막으로, 태블릿 컴퓨터에서 앱을 실행 하는 경우:

[![Android 태블릿에서 실행 중인 앱](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

이 샘플 응용 프로그램 조각을 사용 하 여 다양 한 폼 팩터 및 최소한의 코드 변경 내용으로 방향에 적용할 수 쉽게 및 [대체 레이아웃](/xamarin/android/app-fundamentals/resources-in-android/alternate-resources)합니다.

응용 프로그램에 대 한 데이터는 C# 문자열 배열로 사용 되는 응용 프로그램에서 하드 코딩 된 두 문자열 배열에 존재 합니다. 배열의 각 항목은 하나의 조각에 대 한 데이터 소스로 역할 합니다.  한 배열을 셰익스피어에서 일부 역할의 이름을 누른 다른 배열의 해당 play에서 견적을 보유 합니다. 응용 프로그램 시작 되 면에 있는 재생 이름이 표시 됩니다는 `ListFragment`합니다. 사용자의 재생을 클릭할 때는 `ListFragment`, 하면 앱이 인용 표시 하는 다른 활동을 시작 합니다.

응용 프로그램에 대 한 사용자 인터페이스는 두 개의 레이아웃, 세로 및 가로 모드에 대 한 단계로 구성 됩니다. 런타임 시 Android 장치의 방향에 따라 로드 하는 레이아웃을 결정 합니다 및 렌더링 작업에 해당 레이아웃을 제공 합니다. 모든 사용자가 클릭에 응답 하 고 데이터를 표시 하기 위한 논리는 조각에 포함 됩니다. 응용 프로그램의 활동은이 조각 들을 호스팅할 컨테이너로 존재 합니다.

이 연습에서는 두 명의 가이드로 세분화 됩니다. [첫 번째 부분](./walkthrough.md) 응용 프로그램의 핵심 부분에 집중 합니다. 두 조각 및 두 개의 활동 함께 (세로 모드에 대 한 최적화) 레이아웃의 단일 집합 생성 됩니다.

1. `MainActivity` &nbsp; 응용 프로그램에 대 한 시작 작업입니다.
1. `TitlesFragment` &nbsp; 이 조각의 William 셰익스피어로 작성 된 재생의 타이틀의 목록을 표시 됩니다. 의해 호스트 될 `MainActivity`합니다.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` 시작 됩니다는 `PlayQuoteActivity` 에서 재생을 선택 하면 사용자에 대 한 응답에서 `TitlesFragment`합니다.
1. `PlayQuoteFragment` &nbsp; 이 조각의 William 셰익스피어 하 여 재생에서 견적을 표시 됩니다. 의해 호스트 될 `PlayQuoteActivity`합니다.

[이 연습에서는의 두 번째 부분](./walkthrough-landscape.md) 추가 두 조각 화면에 표시 하는 대체 레이아웃 (가로 모드에 대 한 최적화)에 대해 설명 합니다. 또한 몇 가지 사소한 코드 변경이 걸 수 코드를 응용 프로그램을 동시에 화면에 표시 되는 조각 수를 해당 동작을 적용할 수 있도록.

## <a name="related-links"></a>관련 링크

- [FragmentsWalkthrough (샘플)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [디자이너 개요](~/android/user-interface/android-designer/index.md)
- [조각 구현](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [지원 패키지](http://developer.android.com/sdk/compatibility-library.html)
