---
title: Xamarin.Forms TwoPaneView
description: 이 가이드에서는 Xamarin.Forms TwoPaneView를 사용하여 Surface Duo 및 Surface Neo와 같은 이중 화면 디바이스의 앱 환경을 최적화하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 17ee8afa-5e7c-4a4f-a9b6-2aca03f30fe3
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: 1992a714774dba79e42c82e71af0bfe6aed7feb7
ms.sourcegitcommit: 07941cf9704ff88cf4087de5ebdea623ff54edb1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145507"
---
# <a name="xamarinforms-twopaneview"></a>Xamarin.Forms TwoPaneView

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/UserInterface/DualScreenDemos)

사용 가능한 공간에서 콘텐츠를 나란히 또는 위아래로 크기와 위치를 지정하는 보기가 2개 포함된 컨테이너를 나타냅니다. TwoPaneView는 Grid에서 상속하므로 이러한 속성이 그리드에 적용되는 것처럼 생각하면 가장 간단합니다.

## <a name="set-up-twopaneview"></a>TwoPaneView 설정

`TwoPaneView` 속성 `Source`에는 URI나 로컬 파일 경로를 사용할 수 있습니다. 미디어를 열면 바로 재생이 시작됩니다.

```xaml
<ContentPage xmlns:dualScreen="clr-namespace:Xamarin.Forms.DualScreen;assembly=Xamarin.Forms.DualScreen">
    <dualScreen:TwoPaneView>
        <dualScreen:TwoPaneView.Pane1>
            <StackLayout>
                <Label Text="Pane1 Content"></Label>
            </StackLayout>
        </dualScreen:TwoPaneView.Pane1>
        <dualScreen:TwoPaneView.Pane2>
            <StackLayout>
                <Label Text="Pane2 Content"></Label>
            </StackLayout>
        </dualScreen:TwoPaneView.Pane2>
    </dualScreen:TwoPaneView>
</ContentPage>
```

## <a name="understand-twopaneview-modes"></a>TwoPaneView 모드 이해

다음 모드 중 하나만 활성화할 수 있습니다.

- `SinglePane` - 한 창만 현재 표시됩니다.
- `Wide` - 두 창이 가로로 배치됩니다. 한 창은 왼쪽에, 다른 창은 오른쪽에 있습니다. 두 화면에 표시되는 경우 디바이스가 세로일 때 이 모드가 사용됩니다.
- `Tall` - 두 창이 세로로 배치됩니다. 한 창은 위쪽에, 다른 창은 아래쪽에 있습니다. 두 화면에 표시되는 경우 디바이스가 가로일 때 이 모드가 사용됩니다.

## <a name="control-twopaneview-when-its-only-on-one-screen"></a>한 화면에만 표시될 때 TwoPaneView 제어

다음 속성은 TwoPaneView가 단일 화면을 차지하는 경우에만 사용됩니다. TwoPaneView가 두 개 화면에 걸쳐 있는 경우 이러한 속성은 적용되지 않습니다.

- `MinTallModeHeight` - 컨트롤이 세로 모드로 전환하기 위해 되어야 하는 최소 높이를 나타냅니다.
- `MinWideModeWidth` - 컨트롤이 가로 모드로 전환하기 위해 되어야 하는 최소 너비를 나타냅니다.
- `Pane1Length` - 가로 모드에서는 Pane1의 너비, 세로 모드에서는 Pane1의 높이를 설정하며 SinglePane 모드에서는 적용되지 않습니다.
- `Pane2Length` - 가로 모드에서는 Pane2의 너비, 세로 모드에서는 Pane2의 높이를 설정하며 SinglePane 모드에서는 적용되지 않습니다.

## <a name="properties-that-apply-when-on-one-screen-or-two"></a>한 화면 또는 두 화면에 표시될 때 적용되는 속성

- `TallModeConfiguration` - 세로 모드일 때 왼쪽/오른쪽 정렬을 나타내거나 TwoPaneViewPriority에서 정의한 대로 단일 창만 표시할지 여부를 나타냅니다.
- `WideModeConfiguration` - 세로 모드일 때 위쪽/아래쪽 정렬을 나타내거나 TwoPaneViewPriority에서 정의한 대로 단일 창만 표시할지 여부를 나타냅니다.
- `PanePriority` - SinglePane 모드일 경우 Pane1을 표시할지, Pane2를 표시할지를 나타냅니다.

## <a name="related-links"></a>관련 링크

- [DualScreen(sample)](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/UserInterface/DualScreenDemos)(DualScreen(샘플))
