---
title: Xamarin Android 및 마모 된 OS에서 화면 크기 작업
ms.prod: xamarin
ms.assetid: 77831169-C663-4D42-B742-B8B556B1DA4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 4673bc6898da06f07a624b4aa585e62009a575e1
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758310"
---
# <a name="working-with-screen-sizes"></a>화면 크기 작업

Android 장치는 사각형 또는 둥근 표시를 포함할 수 있으며 크기는 달라질 수도 있습니다.

![사각형 및 라운드 마모 표시의 스크린샷](screen-sizes-images/moyeu-wear.png)

## <a name="identifying-screen-type"></a>화면 유형 식별

마모 된 지원 라이브러리는 `WatchViewStub` 및 `BoxInsetLayout`와 같은 다양 한 화면 모양을 검색 하 고이에 맞게 조정 하는 데 도움이 되는 몇 가지 컨트롤을 제공 합니다.

다른 지원 라이브러리 컨트롤 (예: `GridViewPager`) 중 일부는 화면 셰이프 자체를 *자동으로* 검색 하 고 아래에서 설명 하는 컨트롤의 자식으로 추가 되어서는 안 됩니다.

### <a name="watchviewstub"></a>WatchViewStub

[WatchViewStub](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-watchviewstub) 샘플을 참조 하 여 화면 유형을 검색 하 고 각 유형별로 다른 레이아웃을 표시 하는 방법을 확인 합니다.

주 레이아웃 파일에는 `android.support.wearable.view.WatchViewStub` `app:rectLayout` 및 `app:roundLayout` 특성을 사용 하는 사각형 및 원형 화면에 대해 서로 다른 레이아웃을 참조 하는가 포함 되어 있습니다.

```xml
<android.support.wearable.view.WatchViewStub
    xmlns:app="http://schemas.android.com/apk/res-auto"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:id="@+id/stub"
  app:rectLayout="@layout/rect_layout"
  app:roundLayout="@layout/round_layout" />
```

솔루션에는 런타임에 선택 되는 각 스타일에 대 한 다양 한 레이아웃이 포함 되어 있습니다.

![리소스/레이아웃 아래에 표시 되는 파일](screen-sizes-images/solution.png)

### <a name="boxinsetlayout"></a>BoxInsetLayout

각 화면 유형에 대해 서로 다른 레이아웃을 작성 하는 대신 사각형 또는 둥근 화면에 맞게 조정 되는 단일 보기를 만들 수도 있습니다.

이 [Google 예제](https://developer.android.com/training/wearables/ui/layouts.html#same-layout) 에서는를 `BoxInsetLayout` 사용 하 여 사각형 및 원형 화면에서 동일한 레이아웃을 사용 하는 방법을 보여 줍니다.

## <a name="wear-ui-designer"></a>마모 UI 디자이너

Xamarin Android Designer는 사각형 및 원형 화면을 모두 지원 합니다.

![Xamarin Android Designer에서 Android 착용 사각형 화면 선택](screen-sizes-images/design-screen-type.png)

디자인 화면이 사각형 스타일로 표시 됩니다.

![사각형 스타일의 디자인 화면](screen-sizes-images/design-rect.png) 

다음과 같이 둥근 스타일의 디자인 화면이 표시 됩니다.

![둥근 스타일의 디자인 화면](screen-sizes-images/design-round.png)

## <a name="wear-simulator"></a>마모 시뮬레이터

**Google Emulator Manager** 에는 두 화면 유형 모두에 대 한 장치 정의가 포함 되어 있습니다. 사각형 및 라운드 에뮬레이터를 만들어 앱을 테스트할 수 있습니다.

![Google 에뮬레이터 관리자에 표시 된 장치 정의를 착용](screen-sizes-images/emulator-devices.png)

에뮬레이터는 사각형 화면에 대해 다음과 같이 렌더링 합니다.

![사각형 화면의 에뮬레이터 렌더링](screen-sizes-images/recipe-2.png) 

라운드 화면에 대해 다음과 같이 렌더링 됩니다.

![원형 화면의 에뮬레이터 렌더링](screen-sizes-images/recipe-2-round.png)

## <a name="video"></a>비디오

[Developers.google.com](https://www.youtube.com/channel/UC_x5XG1OV2P6uZZ5FSM9Ttw)에서 [Android를 위한 전체 화면 앱](https://www.youtube.com/watch?v=naf_WbtFAlY) 입니다.
