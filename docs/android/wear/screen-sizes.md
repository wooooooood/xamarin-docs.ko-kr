---
title: Xamarin.Android 및 마모 운영 체제에 대 한 화면 크기와 작업
ms.prod: xamarin
ms.assetid: 77831169-C663-4D42-B742-B8B556B1DA4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: 40e7850ffe239b0ede43e4d0cd3c6da08bce3a40
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436078"
---
# <a name="working-with-screen-sizes"></a>화면 크기와 작업

Android 마모 장치는 사각형 또는 라운드 표시 되는 다양 한 크기 수 있는 가질 수 있습니다.

![사각형 및 라운드 마모의 스크린 샷을 표시 됩니다.](screen-sizes-images/moyeu-wear.png)

## <a name="identifying-screen-type"></a>화면 형식 식별

마모 지원 라이브러리에서 제공 하는 데 도움이 되는 일부 컨트롤 검색 및와 같은 다양 한 화면 모양에 맞게 조정 `WatchViewStub` 및 `BoxInsetLayout`합니다.

일부 다른 라이브러리 컨트롤이 지원 (같은 `GridViewPager`) *자동으로* 자체 화면 셰이프를 검색 하 고 컨트롤의 자식 아래 설명 된 대로 추가할 수 없습니다.

### <a name="watchviewstub"></a>WatchViewStub

참조는 [WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/) 화면 종류를 검색 하 여 각 형식에 대 한 다른 레이아웃을 표시 하는 방법을 보려면 샘플입니다.

기본 레이아웃 파일에 포함 되어는 `android.support.wearable.view.WatchViewStub` 를 사용 하 여 사각형 및 라운드 화면에 대 한 다양 한 레이아웃을 참조 하는 `app:rectLayout` 및 `app:roundLayout` 특성:

```xml
<android.support.wearable.view.WatchViewStub
    xmlns:app="http://schemas.android.com/apk/res-auto"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:id="@+id/stub"
  app:rectLayout="@layout/rect_layout"
  app:roundLayout="@layout/round_layout" />
```

솔루션에서 런타임에 선택할 수 있는 각 스타일에 대 한 다양 한 레이아웃을 포함 되어 있습니다.

![리소스/레이아웃 아래에 표시 된 파일](screen-sizes-images/solution.png)


### <a name="boxinsetlayout"></a>BoxInsetLayout

각 화면 형식에 대 한 다른 레이아웃을 작성 하지 않고 사각형 또는 라운드 화면에 맞게 조정 하는 단일 뷰를 만들 수도 있습니다.

이 [Google 예제](https://developer.android.com/training/wearables/ui/layouts.html#same-layout) 사용 하는 방법을 보여 줍니다.는 `BoxInsetLayout` 사각형 및 라운드 화면에서 동일한 레이아웃을 사용 하도록 합니다.


## <a name="wear-ui-designer"></a>UI 디자이너 쓰는 유형

Xamarin Android 디자이너 사각형 및 라운드 화면을 지원합니다.

![Xamarin Android 디자이너에서 Android 마모 정사각형 화면을 선택 하면](screen-sizes-images/design-screen-type.png)

사각형 스타일의 디자인 화면에는 다음과 같습니다.

![사각형 스타일의 디자인 화면](screen-sizes-images/design-rect.png) 

Round 스타일의 디자인 화면에는 다음과 같습니다.

![Round 스타일의 디자인 화면](screen-sizes-images/design-round.png)


## <a name="wear-simulator"></a>시뮬레이터 쓰는 유형

**Google 에뮬레이터 관리자** 형식 화면 둘 다에 대 한 장치 정의 포함 합니다. 응용 프로그램을 테스트 하려면 사각형 및 라운드 에뮬레이터를 만들 수 있습니다.

![Google 에뮬레이터 관리자에 표시 되는 장치 정의 되지 않습니다.](screen-sizes-images/emulator-devices.png)

에뮬레이터는 직사각형 화면에 대 한 다음과 같은 렌더링 됩니다.

![에뮬레이터의 직사각형 화면 렌더링](screen-sizes-images/recipe-2.png) 

Round 화면에 대 한 다음과 같은 렌더링 합니다.

![에뮬레이터의 라운드 화면의 렌더링](screen-sizes-images/recipe-2-round.png)

## <a name="video"></a>비디오

[Android 마모에 대 한 전체 화면 앱](https://www.youtube.com/watch?v=naf_WbtFAlY) 에서 [developers.google.com](https://www.youtube.com/channel/UC_x5XG1OV2P6uZZ5FSM9Ttw)합니다.

