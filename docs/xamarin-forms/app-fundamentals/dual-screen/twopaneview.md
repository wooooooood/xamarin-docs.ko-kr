---
title: Xamarin.Forms 이중 화면 레이아웃
description: 이 가이드에서는 Xamarin.Forms TwoPaneView를 사용하여 Surface Duo 및 Surface Neo와 같은 이중 화면 디바이스의 앱 환경을 최적화하는 방법을 설명합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 28d4b3da44cc1a022b70c0de0720be747e047f9f
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138894"
---
# <a name="xamarinforms-dual-screen-layout"></a>Xamarin.Forms 이중 화면 레이아웃

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)

`TwoPaneView` 클래스는 사용 가능한 공간에서 나란히 또는 위아래로 콘텐츠의 크기와 위치를 지정하는 2개의 보기가 있는 컨테이너를 나타냅니다. `TwoPaneView`는 `Grid`에서 상속되므로 속성이 그리드에 적용되는 것이라고 생각하면 쉽습니다.

## <a name="set-up-twopaneview"></a>TwoPaneView 설정

`TwoPaneView.Source` 속성은 URI 또는 로컬 파일 경로를 받습니다. 미디어를 열면 바로 재생이 시작됩니다.

```xaml
<ContentPage xmlns:dualScreen="clr-namespace:Xamarin.Forms.DualScreen;assembly=Xamarin.Forms.DualScreen">
    <dualScreen:TwoPaneView>
        <dualScreen:TwoPaneView.Pane1>
            <StackLayout>
                <Label Text="Pane1 Content" />
            </StackLayout>
        </dualScreen:TwoPaneView.Pane1>
        <dualScreen:TwoPaneView.Pane2>
            <StackLayout>
                <Label Text="Pane2 Content" />
            </StackLayout>
        </dualScreen:TwoPaneView.Pane2>
    </dualScreen:TwoPaneView>
</ContentPage>
```

## <a name="understand-twopaneview-modes"></a>TwoPaneView 모드 이해

다음 모드 중 하나만 활성화할 수 있습니다.

- `SinglePane` - 현재 창 하나만 볼 수 있습니다.
- `Wide` - 두 창이 가로로 배치됩니다. 한 창은 왼쪽에, 다른 창은 오른쪽에 있습니다. 두 개 화면에서는 이것이 디바이스가 세로 방향일 때의 모드입니다.
- `Tall` - 두 창이 세로로 배치됩니다. 한 창은 위쪽에, 다른 창은 아래쪽에 있습니다. 두 개 화면에서는 이것이 디바이스가 가로 방향일 때의 모드입니다.

## <a name="control-twopaneview-when-its-only-on-one-screen"></a>한 화면에만 표시될 때 TwoPaneView 제어

다음 속성은 `TwoPaneView`가 하나의 화면을 차지하고 있는 경우에 적용됩니다.

- `MinTallModeHeight`는 컨트롤이 세로 모드가 되려면 필요한 최소 높이를 나타냅니다.
- `MinWideModeWidth`는 컨트롤이 가로 모드가 되려면 필요한 최소 너비를 나타냅니다.
- `Pane1Length`는 가로 모드에서 Pane1의 너비를, 세로 모드에서 Pane1의 높이를 설정하며 SinglePane 모드에서는 적용되지 않습니다.
- `Pane2Length`는 가로 모드에서 Pane2의 너비를, 세로 모드에서 Pane2의 높이를 설정하며 SinglePane 모드에서는 적용되지 않습니다.

> [!IMPORTANT]
> `TwoPaneView`가 두 개 화면에 걸쳐 있는 경우 이들 속성은 적용되지 않습니다.

## <a name="properties-that-apply-when-on-one-screen-or-two"></a>한 화면 또는 두 화면에 표시될 때 적용되는 속성

다음 속성은 `TwoPaneView`가 하나의 화면 또는 두 개의 화면을 차지하고 있는 경우에 적용됩니다.

- `TallModeConfiguration`은 세로 모드일 때 왼쪽/오른쪽 정렬을 나타내거나 TwoPaneViewPriority에서 정의한 대로 단일 창만 표시할지 여부를 나타냅니다.
- `WideModeConfiguration`은 가로 모드일 때 위쪽/아래쪽 정렬을 나타내거나 TwoPaneViewPriority에서 정의한 대로 단일 창만 표시할지 여부를 나타냅니다.
- `PanePriority`는 SinglePane 모드일 경우 Pane1와 Pane2 중 어느 것을 표시할지를 정합니다.

## <a name="related-links"></a>관련 링크

- [DualScreen(sample)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)(DualScreen(샘플))
