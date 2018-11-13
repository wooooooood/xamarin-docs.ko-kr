---
title: 요약 14 장입니다. 절대 레이아웃
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 14 장 요약 합니다. 절대 레이아웃'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 88882A48-3226-42D1-96ED-241250B64A84
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 6393794a20a80fae43e31a96efd4941f4b633c28
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563721"
---
# <a name="summary-of-chapter-14-absolute-layout"></a>요약 14 장입니다. 절대 레이아웃

와 같은 `StackLayout`, [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout) 에서 파생 `Layout<View>` 상속을 `Children` 속성입니다. `AbsoluteLayout` 프로그래머가 해당 자식이 있고 선택적으로 해당 크기의 위치를 지정 하는 레이아웃 시스템을 구현 합니다. 왼쪽 위 모퉁이 기준으로 자식 항목의 왼쪽 위 모퉁이에서 위치 지정을 `AbsoluteLayout` 장치 독립적 단위에서입니다. `AbsoluteLayout` 또한 비례 위치 및 크기 조정 기능을 구현합니다.

`AbsoluteLayout` 프로그래머는 자식에 대해 크기를 적용할 수 있는 경우에 사용할 특수 한 용도의 레이아웃 시스템으로 간주 됩니다 (예를 들어 `BoxView` 요소) 때 요소의 크기에 영향을 주지 다른 하위 항목의 위치 또는 합니다. 합니다 `HorizontalOptions` 하 고 `VerticalOptions` 속성의 자식에 영향을 주지는 `AbsoluteLayout`합니다.

이 장에서 또한의 중요 한 기능을 소개 *연결 된 바인딩 가능한 속성* 하나의 클래스에 정의 된 속성을 허용 하는 (이 예제의 `AbsoluteLayout`) 다른 클래스에 연결 (자식은 `AbsoluteLayout`).

## <a name="absolutelayout-in-code"></a>코드에서 AbsoluteLayout

자식으로 추가할 수 있습니다는 `Children` 의 컬렉션을 `AbsoluteLayout` 표준을 사용 하 여 [ `Add` ](xref:System.Collections.Generic.ICollection`1.Add*) 메서드를 하지만 `AbsoluteLayout` 도 확장을 제공 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Rectangle/Xamarin.Forms.AbsoluteLayoutFlags/) 지정할 수 있는 메서드를 [ `Rectangle` ](xref:Xamarin.Forms.Rectangle)합니다. 다른 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Point/) 하기만 메서드를 [ `Point` ](xref:Xamarin.Forms.Point), 자식 제약을 받지 않는 하 고 크기가 자동으로 조정 하는 경우.

만들 수 있습니다는 `Rectangle` 값을 [생성자](xref:Xamarin.Forms.Rectangle.%23ctor(System.Double,System.Double,System.Double,System.Double)) 네 가지 값을 지정 해야 하는 &mdash; 부모에 상대적인 자식 항목의 왼쪽 위 모퉁이 위치를 나타내는 첫 번째 두 및 나타내는 두는 자식의 크기입니다. 사용할 수 있습니다는 [생성자](xref:Xamarin.Forms.Rectangle.%23ctor(Xamarin.Forms.Point,Xamarin.Forms.Size)) 필요로 하는 `Point` 및 [ `Size` ](xref:Xamarin.Forms.Size) 값입니다.

이러한 `Add` 방법에서 설명 됩니다 [ **AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo)에 위치 `BoxView` 요소를 사용 하 여 `Rectangle` 값 및 `Label` 에만사용하여요소`Point` 값입니다.

합니다 [ **ChessboardFixed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed) 샘플에서는 32 `BoxView` 판 패턴을 만들 요소입니다. 프로그램을 제공 합니다 `BoxView` 35 단위 정사각형의 크기를 하드 코드 된 요소입니다. `AbsoluteLayout` 에 해당 `HorizontalOptions` 및 `VerticalOptions` 로 설정 `LayoutOptions.Center`, 있어는 `AbsoluteLayout` 280 단위 정사각형의 총 크기입니다.

## <a name="attached-bindable-properties"></a>연결 된 바인딩 가능한 속성

것도 가능 위치 및 필요에 따라 자식의 크기를 설정 하는 `AbsoluteLayout` 에 추가 된 후 합니다 `Children` 정적 메서드를 사용 하 여 컬렉션 [ `AbsoluteLayout.SetLayoutBounds` ](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutBounds(Xamarin.Forms.BindableObject,Xamarin.Forms.Rectangle))합니다. 첫 번째 인수는 자식; 두 번째는 `Rectangle` 개체입니다. 자식 크기 자체 가로 및/또는 세로로 너비 및 높이 값을 설정 하 여 지정할 수 있습니다 합니다 [ `AbsoluteLayout.AutoSize` ](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) 상수입니다.

[ **ChessboardDynamic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) put 샘플를 `AbsoluteLayout` 에 `ContentView` 사용 하 여를 `SizeChanged` 호출할 처리기입니다 `AbsoluteLayout.SetLayoutBounds` 최대한 크게 있도록 모든 자식에 대해.  

연결된 된 바인딩 가능한 속성입니다 `AbsoluteLayout` 정의 형식의 정적 읽기 전용 필드 `BindableProperty` 라는 [ `AbsoluteLayout.LayoutBoundsProperty` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty)합니다. 정적 `AbsoluteLayout.SetLayoutBounds` 메서드를 호출 하 여 구현 됩니다 `SetValue` 자식으로 `AbsoluteLayout.LayoutBoundsProperty`합니다. 자식 연결된 된 바인딩 가능한 속성 및 해당 값 저장 되는 사전을 포함 합니다. 레이아웃 중 합니다 `AbsoluteLayout` 를 호출 하 여 해당 값을 가져올 수 [ `AbsoluteLayout.GetLayoutBounds` ](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutBounds(Xamarin.Forms.BindableObject)),으로 구현 되는 `GetValue` 호출 합니다.

## <a name="proportional-sizing-and-positioning"></a>가변 크기 조정 및 위치 지정

`AbsoluteLayout` 비례하여 크기 조정 및 위치 기능을 구현 합니다. 두 번째 연결된 바인딩 가능한 속성을 정의 하는 클래스 [ `LayoutFlagsProperty` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty), 관련 된 정적 메서드를 사용 하 여 [ `AbsoluteLayout.SetLayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutFlags(Xamarin.Forms.BindableObject,Xamarin.Forms.AbsoluteLayoutFlags)) 고 [ `AbsoluteLayout.GetLayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutFlags(Xamarin.Forms.BindableObject))합니다.

인수 `AbsoluteLayout.SetLayoutFlags` 의 반환 값과 `AbsoluteLayout.GetLayoutFlags` 형식의 값인 [ `AbsoluteLayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayoutFlags)를 다음 멤버로 구성 된 열거형:

- [`None`](xref:Xamarin.Forms.AbsoluteLayoutFlags.None) (0 같음)
- [`XProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.XProportional) (1)
- [`YProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.YProportional) (2)
- [`PositionProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional) (3)
- [`WidthProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional) (4)
- [`HeightProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional) (8)
- [`SizeProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional) (12)
- [`All`](xref:Xamarin.Forms.AbsoluteLayoutFlags.All) (\xFFFFFFFF)

이를 결합할 수 있습니다는 C# 비트 OR 연산자입니다.

이러한 플래그 집합의 특정 속성을 사용 하 여는 `Rectangle` 레이아웃 범위 구조를 배치 하 고 자식 크기를 비례적으로 해석 됩니다.

경우는 `WidthProportional` 플래그가 설정 되 면를 `Width` 값이 1 이면 자식 동일한 너비로는 `AbsoluteLayout`합니다. 높이 대 한 유사한 접근 방식을 사용 됩니다.

비례 위치는 크기를 고려 합니다. 경우는 `XProportional` 플래그가 설정 되 면를 `X` 의 속성은 `Rectangle` 레이아웃 범위 비례 합니다. 값이 0 이면 자식의 왼쪽 가장자리의 왼쪽된 가장자리에 배치 됩니다 합니다 `AbsoluteLayout`, 하지만 1 이면 자식의 오른쪽 가장자리의 오른쪽 가장자리에 배치 되는 위치를 `AbsoluteLayout`의 오른쪽 가장자리를 넘어 하지는 `AbsoluteLayout` expec 마찬가지로 t입니다. `X` 0.5의 속성에 가로로 자식 센터는 `AbsoluteLayout`합니다.

합니다 [ **ChessboardProportional** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional) 비례 크기 조정 및 위치 지정의 사용법을 보여 줍니다.

## <a name="working-with-proportional-coordinates"></a>비례 좌표를 사용 하 여 작업

경우에 따라 쉽습니다 비례 위치에서 구현 하는 방법을 보다 다르게 생각 하는 `AbsoluteLayout`합니다. 비례 좌표를 사용 하는 것이 좋습니다 위치는 `X` 의 오른쪽 가장자리에 대 한 자식의 왼쪽된 가장자리 (오른쪽 가장자리 아님)를 배치 하는 1의 속성을 `AbsoluteLayout`.

이 대체 위치 지정 체계를 호출할 수 있습니다 "소수 자식 좌표입니다." 에 필요한 레이아웃 경계에 소수 자릿수 자식 좌표에서 변환할 수 있습니다 `AbsoluteLayout` 다음 수식을 사용 하 여:

layoutBounds.X = (fractionalChildCoordinate.X / (1-layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y / (1-layoutBounds.Height))

합니다 [ **ProportionalCoordinateCalc** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc) 샘플에서는이 방법을 보여 줍니다.

## <a name="absolutelayout-and-xaml"></a>AbsoluteLayout 및 XAML

사용할 수는 `AbsoluteLayout` XAML에서 자식의 연결 된 바인딩 가능한 속성을 설정 하 고는 `AbsoluteLayout` 의 특성 값을 사용 하 여 `AbsoluteLayout.LayoutBounds` 및 `AbsoluteLayout.LayoutFlags`합니다. 에 설명 되어이 [ **AbsoluteXamlDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo) 하며 [ **ChessboardXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml) 샘플입니다. 후자 프로그램 32를 포함 `BoxView` 요소 하지만 사용 하 여 암시적 `Style` 포함 하는 `AbsoluteLayout.LayoutFlags` 속성을 최소한으로 태그를 유지 합니다.

클래스 이름, 점, 및 속성 이름으로 구성 된 XAML에서 특성이 *항상* 연결된 된 바인딩 가능한 속성입니다.

## <a name="overlays"></a>오버레이

사용할 수 있습니다 `AbsoluteLayout` 생성 하는 *오버레이*, 페이지의 일반 컨트롤 상호 작용에서 사용자를 보호 하기 위해 아마도 다른 컨트롤을 사용 하 여 페이지를 포함 합니다.

합니다 [ **SimpleOverlay** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay) 샘플이이 기술을 보여 줍니다 및 방법도 보여 줍니다 합니다 [ `ProgressBar` ](xref:Xamarin.Forms.ProgressBar), 프로그램을 완료 하는 정도 표시 하는 작업입니다.

## <a name="some-fun"></a>재미

합니다 [ **DotMatrixClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock) 샘플 시뮬레이션된 5 x 7 도트 매트릭스 표시를 사용 하 여 현재 시간을 표시 합니다. 각 점이 `BoxView` (그 중 228는) 크기 및 배치를 `AbsoluteLayout`입니다.

[![삼중 스크린샷 도트 매트릭스 클록](images/ch14fg08-small.png "도트 매트릭스 클록")](images/ch14fg08-large.png#lightbox "도트 매트릭스 시계")

합니다 [ **BouncingText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText) 두 프로그램 애니메이션 `Label` 화면 가로 및 세로로 반송 하는 개체입니다.



## <a name="related-links"></a>관련 링크

- [14 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [14 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
- [연결된 속성](~/xamarin-forms/xaml/attached-properties.md)
