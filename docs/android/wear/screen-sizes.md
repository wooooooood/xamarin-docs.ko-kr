---
title: Xamarin.Android 및 Wear OS의 화면 크기를 사용 하 여 작업
ms.prod: xamarin
ms.assetid: 77831169-C663-4D42-B742-B8B556B1DA4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 9fc22a3c08b60a8474b006f1c9225155b9705507
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113120"
---
# <a name="working-with-screen-sizes"></a>화면 크기를 사용 하 여 작업

Android Wear 장치는 사각형 또는 round 표시 되는 다양 한 일 수 있음에 있을 수 있습니다.

![사각형 및 round Wear의 스크린 샷에 표시](screen-sizes-images/moyeu-wear.png)

## <a name="identifying-screen-type"></a>화면 형식 식별

Wear 지원 라이브러리는 도움이 되는 일부 컨트롤 검색 및와 같은 다양 한 화면 모양에 맞게 제공 `WatchViewStub` 고 `BoxInsetLayout`입니다.

라이브러리 컨트롤 지원 몇 가지 다른 (같은 `GridViewPager`) *자동으로* 자체 화면 셰이프를 검색 하 고 아래 설명 된 컨트롤의 자식으로 추가 해서는 안 됩니다.

### <a name="watchviewstub"></a>WatchViewStub

참조를 [WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/) 샘플을 화면 종류를 검색 하 여 각 형식에 대 한 다른 레이아웃을 표시 하는 방법을 참조 하십시오.

기본 레이아웃 파일에 포함 되어는 `android.support.wearable.view.WatchViewStub` 를 사용 하 여 사각형 및 둥근 화면에 대 한 다른 레이아웃을 참조 하는 합니다 `app:rectLayout` 및 `app:roundLayout` 특성:

```xml
<android.support.wearable.view.WatchViewStub
    xmlns:app="http://schemas.android.com/apk/res-auto"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:id="@+id/stub"
  app:rectLayout="@layout/rect_layout"
  app:roundLayout="@layout/round_layout" />
```

솔루션에는 런타임 시 선택 하는 각 스타일에 대 한 다른 레이아웃 포함 됩니다.

![리소스/레이아웃 아래에 표시 된 파일](screen-sizes-images/solution.png)


### <a name="boxinsetlayout"></a>BoxInsetLayout

각 화면 형식에 대 한 다른 레이아웃을 빌드, 대신 사각형 또는 둥근 화면에 맞게 조정 되는 단일 뷰를 만들 수도 있습니다.

이 [Google 예제](https://developer.android.com/training/wearables/ui/layouts.html#same-layout) 사용 하는 방법을 보여 줍니다는 `BoxInsetLayout` 사각형 및 둥근 화면에서 동일한 레이아웃을 사용 하도록 합니다.


## <a name="wear-ui-designer"></a>Wear UI 디자이너

Xamarin Android Designer는 사각형 및 둥근 화면을 지원합니다.

![Xamarin Android Designer에서 Android Wear 정사각형 화면 선택](screen-sizes-images/design-screen-type.png)

디자인 화면에서 사각형 스타일은 다음과 같습니다.

![사각형 스타일에서 디자인 화면](screen-sizes-images/design-rect.png) 

반올림 스타일에서 디자인 화면을은 다음과 같습니다.

![반올림 스타일의 디자인 화면](screen-sizes-images/design-round.png)


## <a name="wear-simulator"></a>Wear 시뮬레이터

합니다 **Google 에뮬레이터 관리자** 화면 형식은 둘 다에 대 한 장치 정의 포함 합니다. 앱을 테스트 하려면 사각형 및 round 에뮬레이터를 만들 수 있습니다.

![Wear 장치 정의 Google 에뮬레이터 관리자에 표시](screen-sizes-images/emulator-devices.png)

에뮬레이터는 사각형을 화면에 대 한이 다음과 같이 렌더링 됩니다.

![에뮬레이터 직사각형 화면 렌더링](screen-sizes-images/recipe-2.png) 

둥근 화면에 대 한 다음과 같은 렌더링 됩니다.

![에뮬레이터는 둥근 화면 렌더링](screen-sizes-images/recipe-2-round.png)

## <a name="video"></a>비디오

[전체 화면 앱을 Android Wear](https://www.youtube.com/watch?v=naf_WbtFAlY) 에서 [developers.google.com](https://www.youtube.com/channel/UC_x5XG1OV2P6uZZ5FSM9Ttw)합니다.

