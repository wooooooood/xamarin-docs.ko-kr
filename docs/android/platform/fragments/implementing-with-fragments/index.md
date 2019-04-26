---
title: 연습-조각 구현
description: 이 문서 조각을 사용 하 여 Xamarin.Android 응용 프로그램을 개발 하는 방법을 안내 합니다.
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
ms.openlocfilehash: 2ff4729e68497391d41521da26917571c146b541
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60953298"
---
# <a name="implementing-fragments---walkthrough"></a>연습-조각 구현

_조각은 다양 한 화면 크기를 사용 하 여 장치를 대상으로 하는 Android 앱의 복잡성을 해결 하는 데 도움이 되는 자체 포함 된, 모듈식 구성 요소입니다. 이 문서는 만들고 Xamarin.Android 응용 프로그램을 개발 하는 경우 조각을 사용 하는 방법을 안내 합니다._

## <a name="overview"></a>개요

이 섹션에서는 있습니다 과정을 통해 만들고 조각 Xamarin.Android 응용 프로그램에서 사용 하는 방법입니다. 이 응용 프로그램은 여러 재생의 제목을 목록의 William 셰익스피어도 표시 됩니다. 사용자는 재생의 제목에 누르면 다음 앱에에서 표시 됩니다는 play에서 견적을 별도 동작:

[![세로 모드의 Android 휴대폰에서 실행 중인 앱](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

전화를 가로 모드로 회전, 앱의 모양을 변경 됩니다: 동일한 동작에서 재생 및 따옴표의 목록을 모두 표시 됩니다. 재생을 선택 하면 견적 동일한 동작에서 표시 됩니다.

[![Android 휴대폰에서 가로 모드에서 실행 중인 앱](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

마지막으로, 태블릿에서 앱을 실행 하는 경우:

[![Android 태블릿에서 실행 되는 앱](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

이 샘플 응용 프로그램 맞게 쉽게 적용할 수 다양 한 폼 팩터 및 최소한의 코드 변경 방향에 조각을 사용 하 여 및 [대체 레이아웃](/xamarin/android/app-fundamentals/resources-in-android/alternate-resources)합니다.

해당 응용 프로그램에 하드 코딩 되는 두 문자열 배열에서 응용 프로그램에 대 한 데이터는 존재 하는 C# 문자열 배열입니다. 배열의 각 하나의 조각에 대 한 데이터 원본으로 제공 됩니다.  하나의 배열만 셰익스피어에서 일부 재생의 이름을 누른 다른 배열의 해당 play에서 견적 보유할 합니다. 앱 시작에서 play 이름이 표시 됩니다는 `ListFragment`합니다. 사용자의 play를 클릭할 때는 `ListFragment`, 앱 견적을 표시 하는 다른 활동으로 시작 됩니다.

두 가지 레이아웃, 세로 및 가로 모드에 대 한 앱의 사용자 인터페이스 구성 됩니다. 런타임에 Android 장치의 방향을 기준으로 로드 하는 레이아웃을 결정 합니다 및 해당 레이아웃 렌더링 작업을 제공 합니다. 모든 사용자 클릭에 응답 하 고 데이터를 표시 하기 위한 논리 조각에 포함 됩니다. 앱의 활동이 조각을 호스트 하는 컨테이너로 존재 합니다.

이 연습에서는 두 가지 가이드로 세분화 됩니다. 합니다 [첫 번째 파트](./walkthrough.md) 응용 프로그램의 핵심 부분에 집중 합니다. 2 개의 조각을와 두 개의 활동 (세로 모드에 대해 최적화 됨) 레이아웃의 단일 집합, 만들어집니다.

1. `MainActivity` &nbsp; 이 앱에 대 한 시작 작업입니다.
1. `TitlesFragment` &nbsp; 이 조각 William 셰익스피어 하 여 작성 된 재생의 타이틀의 목록을 표시 됩니다. 호스트할 `MainActivity`합니다.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` 시작 합니다 `PlayQuoteActivity` 에 대 한 응답에서 재생을 선택 하면 사용자에 게 `TitlesFragment`합니다.
1. `PlayQuoteFragment` &nbsp; 이 조각 William 셰익스피어에서 play에서 견적을 표시 됩니다. 호스트할 `PlayQuoteActivity`합니다.

합니다 [의이 연습에서는 두 번째 부분](./walkthrough-landscape.md) 조각을 모두 화면에 표시 하는 대체 레이아웃 (가로 모드에 대 한 최적화)를 추가 하겠습니다. 또한 몇 가지 사소한 코드 변경이 됩니다 수에 대 한 코드 앱 화면에서 동시에 표시 되는 조각 수에 해당 동작에 맞게 조정 되도록 합니다.

## <a name="related-links"></a>관련 링크

- [FragmentsWalkthrough (샘플)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [디자이너 개요](~/android/user-interface/android-designer/index.md)
- [조각 구현](https://developer.android.com/guide/topics/fundamentals/fragments.html)
- [지원 패키지](https://developer.android.com/sdk/compatibility-library.html)
