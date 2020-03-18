---
title: 요약 - 14장 절대 레이아웃
description: 'Xamarin.Forms로 모바일 앱 만들기: 요약 - 14장 절대 레이아웃'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 88882A48-3226-42D1-96ED-241250B64A84
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: c489bf244396cf180ed8e1272308048a14b67300
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "70771125"
---
# <a name="summary-of-chapter-14-absolute-layout"></a>요약 - 14장 절대 레이아웃

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)

`StackLayout`의 경우처럼 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)은 `Layout<View>`에서 파생되고 `Children` 속성을 상속합니다. `AbsoluteLayout`은 프로그래머가 자식의 위치를 지정하고 필요에 따라 크기를 지정해야 하는 레이아웃 시스템을 구현합니다. 위치는 디바이스 독립적 단위를 사용하여 `AbsoluteLayout`의 왼쪽 위 구석에 따른 자식의 왼쪽 위 구석으로 지정합니다. 또한 `AbsoluteLayout`은 비례 위치 지정 및 크기 조정 기능을 구현합니다.

`AbsoluteLayout`은 프로그래머가 자식에 크기를 적용할 수 있는 경우(예: `BoxView` 요소) 또는 요소의 크기가 다른 자식의 위치 지정에 영향을 주지 않는 경우에만 사용되는 특수한 용도의 레이아웃 시스템으로 간주해야 합니다. `HorizontalOptions` 및 `VerticalOptions` 속성은 `AbsoluteLayout`의 자식에 영향을 주지 않습니다.

또한 이 장에서는 한 클래스(이 경우 `AbsoluteLayout`)에 정의된 속성을 다른 클래스(`AbsoluteLayout`의 자식)에 연결할 수 있는 *연결된 바인딩 가능 속성*의 중요한 기능을 소개합니다.

## <a name="absolutelayout-in-code"></a>코드의 AbsoluteLayout

표준 [`Add`](xref:System.Collections.Generic.ICollection`1.Add*) 메서드를 사용하여 `AbsoluteLayout`의 `Children` 컬렉션에 자식을 추가할 수 있지만 `AbsoluteLayout`은 사용자가 [`Rectangle`](xref:Xamarin.Forms.Rectangle)을 지정할 수 있도록 하는 확장된 [`Add`](xref:Xamarin.Forms.AbsoluteLayout.IAbsoluteList`1.Add*) 메서드도 제공합니다. 다른 [`Add`](xref:Xamarin.Forms.AbsoluteLayout.IAbsoluteList`1.Add*) 메서드에는 [`Point`](xref:Xamarin.Forms.Point)만 필요하며, 이 경우 자식은 제한되지 않고 자체적으로 크기를 조정할 수 있습니다.

4개의 값을 요구하는 [생성자](xref:Xamarin.Forms.Rectangle.%23ctor(System.Double,System.Double,System.Double,System.Double))를 사용하여 `Rectangle` 값을 만들 수 있습니다. 처음 2개 값은 부모를 기준으로 자식의 왼쪽 위 구석 위치를 나타내고 다음 2개 값은 자식의 크기를 나타냅니다. 또는 `Point` 및 [`Size`](xref:Xamarin.Forms.Size) 값이 필요한 [생성자](xref:Xamarin.Forms.Rectangle.%23ctor(Xamarin.Forms.Point,Xamarin.Forms.Size))를 사용할 수 있습니다.

해당 `Add` 메서드는 `Rectangle` 값을 사용하여 `BoxView` 요소를 배치하고 1개의 `Point` 값만 사용하여 `Label` 요소를 배치하는 [**AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo)에 나와 있습니다.

[**ChessboardFixed**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed) 샘플은 32개의 `BoxView` 요소를 사용하여 체스판 패턴을 만듭니다. 이 프로그램은 `BoxView` 요소에 하드 코드된 35개 단위 정사각형을 제공합니다. `AbsoluteLayout`의 `HorizontalOptions` 및 `VerticalOptions`는 `LayoutOptions.Center`로 설정되어 있으므로 `AbsoluteLayout`은 총 280개의 단위 정사각형을 포함합니다.

## <a name="attached-bindable-properties"></a>연결된 바인딩 가능 속성

정적 메서드 [`AbsoluteLayout.SetLayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutBounds(Xamarin.Forms.BindableObject,Xamarin.Forms.Rectangle))을 사용하여 `Children` 컬렉션에 추가된 후 위치 및 선택적으로 `AbsoluteLayout`의 자식 크기를 설정할 수도 있습니다. 첫 번째 인수는 자식이고 두 번째 인수는 `Rectangle` 개체입니다. 너비 및 높이 값을 [`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) 상수로 설정하여 자식이 가로 및/또는 세로로 자체적으로 크기를 조정하도록 지정할 수 있습니다.

[**ChessboardDynamic**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) 샘플은 모든 자식에 대해 `AbsoluteLayout.SetLayoutBounds`를 호출하여 가능한 한 크게 만들기 위해 `SizeChanged` 처리기를 사용하여 `ContentView`에 `AbsoluteLayout`을 추가합니다.  

`AbsoluteLayout`에서 정의하는 연결된 바인딩 가능 속성은 이름이 [`AbsoluteLayout.LayoutBoundsProperty`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty)이고 `BindableProperty` 형식을 갖는 정적 읽기 전용 필드입니다. 정적 `AbsoluteLayout.SetLayoutBounds` 메서드는 `AbsoluteLayout.LayoutBoundsProperty`를 사용하여 자식에 대해 `SetValue`를 호출하는 방식으로 구현됩니다. 자식은 연결된 바인딩 가능 속성 및 해당 값이 저장되는 사전을 포함합니다. 레이아웃 중에 `AbsoluteLayout`은 `GetValue` 호출로 구현되는 [`AbsoluteLayout.GetLayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutBounds(Xamarin.Forms.BindableObject))를 호출하여 해당 값을 가져올 수 있습니다.

## <a name="proportional-sizing-and-positioning"></a>비례 크기 조정 및 위치 지정

`AbsoluteLayout`은 비례 크기 조정 및 위치 지정 기능을 구현합니다. 이 클래스는 관련된 정적 메서드 [`AbsoluteLayout.SetLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutFlags(Xamarin.Forms.BindableObject,Xamarin.Forms.AbsoluteLayoutFlags)) 및 [`AbsoluteLayout.GetLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutFlags(Xamarin.Forms.BindableObject))를 사용하여 또 다른 연결된 바인딩 가능 속성 [`LayoutFlagsProperty`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty)를 정의합니다.

`AbsoluteLayout.SetLayoutFlags`에 대한 인수와 `AbsoluteLayout.GetLayoutFlags`의 반환 값은 다음 멤버를 포함하는 열거형인 [`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags) 형식의 값입니다.

- [`None`](xref:Xamarin.Forms.AbsoluteLayoutFlags.None)(0과 같음)
- [`XProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.XProportional)(1)
- [`YProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.YProportional)(2)
- [`PositionProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional)(3)
- [`WidthProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional)(4)
- [`HeightProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional)(8)
- [`SizeProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional)(12)
- [`All`](xref:Xamarin.Forms.AbsoluteLayoutFlags.All)(\xFFFFFFFF)

해당 멤버를 C# 비트 OR 연산자와 결합할 수 있습니다.

해당 플래그를 설정하면 자식 위치와 크기를 조정하는 데 사용되는 `Rectangle` 레이아웃 범위 구조의 특정 속성이 비례적으로 해석됩니다.

`WidthProportional` 플래그가 설정되면 `Width` 값 1은 자식이 `AbsoluteLayout`과 동일한 너비로 지정됨을 의미합니다. 높이에도 유사한 방법이 사용됩니다.

비례 위치 지정은 크기를 고려합니다. `XProportional` 플래그가 설정되면 `Rectangle` 레이아웃 범위의 `X` 속성은 비례 관계를 이룹니다. 값이 0이면 자식의 왼쪽 가장자리가 `AbsoluteLayout`의 왼쪽의 가장자리에 배치되지만 위치가 1이면 자식의 오른쪽 가장자리가 예상할 수 있는 것처럼 `AbsoluteLayout`의 오른쪽 가장자리를 벗어나지 않고 `AbsoluteLayout`의 오른쪽 가장자리에 배치됩니다. `X` 속성이 0.5이면 자식은 `AbsoluteLayout`의 가로 중심에 놓입니다.

[**ChessboardProportional**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional) 샘플은 비례적 크기 조정 및 위치 지정 사용 방법을 보여 줍니다.

## <a name="working-with-proportional-coordinates"></a>비례 좌표 작업

경우에 따라 비례 위치 지정을 `AbsoluteLayout`에서 구현하는 것과는 다르게 생각하는 것이 더 쉬울 수 있습니다. `X` 속성이 1인 경우 `AbsoluteLayout`의 오른쪽 가장자리를 기준으로 자식의 왼쪽 가장자리(오른쪽 가장자리 아님)를 배치하는 비례 좌표를 사용하는 것이 적절할 수도 있습니다.

이 대체 위치 지정 체계를 "비율 자식 좌표"라고 합니다. 다음 수식을 사용하여 비율 자식 좌표를 `AbsoluteLayout`에 필요한 레이아웃 범위로 변환할 수 있습니다.

layoutBounds.X = (fractionalChildCoordinate.X / (1 - layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y / (1 - layoutBounds.Height))

[**ProportionalCoordinateCalc**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc) 샘플이 이 작업을 보여 줍니다.

## <a name="absolutelayout-and-xaml"></a>AbsoluteLayout 및 XAML

XAML에서 `AbsoluteLayout`을 사용하고 `AbsoluteLayout.LayoutBounds` 및 `AbsoluteLayout.LayoutFlags` 특성 값을 사용하여 `AbsoluteLayout`의 자식에 대해 연결된 바인딩 가능 속성을 설정할 수 있습니다. 이 방법은 [**AbsoluteXamlDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo) 및 [**ChessboardXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml) 샘플에 나와 있습니다. 후자 프로그램은 32개의 `BoxView` 요소를 포함하지만 `AbsoluteLayout.LayoutFlags` 속성을 포함하는 암시적 `Style`을 사용하여 태그를 최소한으로 유지합니다.

클래스 이름, 도트 및 속성 이름으로 구성된 XAML의 특성은 항상 연결된 바인딩 가능 속성입니다. 

## <a name="overlays"></a>오버레이

`AbsoluteLayout`을 사용하여 다른 컨트롤을 통해 페이지에 적용되는 *오버레이*를 생성하여 사용자가 페이지의 일반 컨트롤과 상호 작용하지 못하게 할 수 있습니다.

[**SimpleOverlay**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay) 샘플은 이 기술을 보여 주며, 프로그램이 작업을 완료한 정도를 표시하는 [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)를 보여 줍니다.

## <a name="some-fun"></a>즐겁게 작업하기

[**DotMatrixClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock) 샘플은 시뮬레이션된 5x7 도트 매트릭스 디스플레이를 사용하여 현재 시간을 표시합니다. 각 도트는 크기가 `BoxView`(228개)이고 `AbsoluteLayout`에 배치됩니다.

[![도트 매트릭스 시계의 3중 스크린샷](images/ch14fg08-small.png "도트 매트릭스 클록")](images/ch14fg08-large.png#lightbox "도트 매트릭스 클록")

[**BouncingText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText) 프로그램은 화면에서 가로 및 세로로 튕기는 두 개의 `Label` 개체에 애니메이션 효과를 적용합니다.

## <a name="related-links"></a>관련 링크

- [14장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [14장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
- [연결된 속성](~/xamarin-forms/xaml/attached-properties.md)
