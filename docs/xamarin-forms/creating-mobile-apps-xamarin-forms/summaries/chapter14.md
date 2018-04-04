---
title: 요약 장 14입니다. 절대 레이아웃
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 88882A48-3226-42D1-96ED-241250B64A84
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 87feb17f79dadb0eb8da271f7c072e4a9753381c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-14-absolute-layout"></a>요약 장 14입니다. 절대 레이아웃

마찬가지로 `StackLayout`, [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) 에서 파생 `Layout<View>` 상속는 `Children` 속성입니다. `AbsoluteLayout` 해당 자식이 있고 선택적으로 해당 크기의 위치를 지정 하는 프로그래머를 필요로 하는 레이아웃 시스템을 구현 합니다. 왼쪽 위 모퉁이 기준으로 자식의 왼쪽 위 모서리에 지정 하는 위치가 `AbsoluteLayout` 장치 독립적 단위에서입니다. `AbsoluteLayout` 또한 비례 배치 및 크기 조정 기능을 구현합니다.

`AbsoluteLayout` 프로그래머가 자식에서 크기를 적용할 수 있는 경우에 사용 하는 특수 한 용도의 레이아웃 시스템으로 간주 되어야 합니다 (예를 들어 `BoxView` 요소) 경우의 효과 크기에 영향을 주지 다른 하위 항목의 위치 또는 합니다. `HorizontalOptions` 및 `VerticalOptions` 속성의 자식에 아무 영향도 `AbsoluteLayout`합니다.

이 장에서 또한의 중요 한 기능은 소개 *연결 된 바인딩 가능한 속성* 하나의 클래스에 정의 된 속성을 허용 하는 (이 경우 `AbsoluteLayout`) 다른 클래스에 연결 될 (의 자식은 `AbsoluteLayout`).

## <a name="absolutelayout-in-code"></a>코드에서 AbsoluteLayout

자식으로 추가할 수 있습니다는 `Children` 의 컬렉션은 `AbsoluteLayout` 표준을 사용 하 여 [ `Add` ](https://developer.xamarin.com/api/member/System.Collections.Generic.ICollection%3CT%3E.Add/p/T/) 메서드를 하지만 `AbsoluteLayout` 확장 제공 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Rectangle/Xamarin.Forms.AbsoluteLayoutFlags/) 지정할 수 있는 메서드는 [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/)합니다. 다른 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Point/) 방법만 필요는 [ `Point` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Point/),이 경우 제약을 받지 않는 자식과 크기가 자동으로 조정 합니다.

만들 수는 `Rectangle` 값과 [생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/System.Double/System.Double/System.Double/System.Double/) 4 개의 값을 필요로 하 &mdash; 부모에 상대적인 자식의 왼쪽 위 모퉁이의 위치를 나타내는 처음 두 가지로를 나타내는 두 번째 두는 하위 항목의 크기입니다. 또는 사용할 수는 [생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/Xamarin.Forms.Point/Xamarin.Forms.Size/) 필요로 하는 `Point` 및 [ `Size` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/) 값입니다.

이러한 `Add` 메서드에서 보여지는 [ **AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo), 어떤 위치 `BoxView` 요소를 사용 하 여 `Rectangle` 값, 및 `Label` 만을사용하여요소`Point` 값입니다.

[ **ChessboardFixed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed) 샘플에서는 32 `BoxView` 다르지만 패턴을 만들기 위해 요소입니다. 프로그램에 의해 붙습니다는 `BoxView` 35 단위 정사각형의 요소는 하드 코드 된 크기입니다. `AbsoluteLayout` 가 해당 `HorizontalOptions` 및 `VerticalOptions` 로 설정 `LayoutOptions.Center`,으로 구독이 `AbsoluteLayout` 280 단위 정사각형의 총 크기입니다.

## <a name="attached-bindable-properties"></a>연결 된 바인딩 가능한 속성

위치 및 필요에 따라 자식의 크기를 설정 하 고 이기도 한 `AbsoluteLayout` 에 추가 된 후는 `Children` 정적 메서드를 사용 하 여 컬렉션 [ `AbsoluteLayout.SetLayoutBounds` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutBounds/p/Xamarin.Forms.BindableObject/Xamarin.Forms.Rectangle/)합니다. 첫 번째 인수는 자식; 두 번째는는 `Rectangle` 개체입니다. 자식 크기가 자동으로 조정 가로 및/또는 세로로 너비 및 높이 값을 설정 하 여 지정할 수는 [ `AbsoluteLayout.AutoSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.AbsoluteLayout.AutoSize/) 상수입니다.

[ **ChessboardDynamic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) 넣습니다 샘플는 `AbsoluteLayout` 에 `ContentView` 와 `SizeChanged` 호출할 처리기 `AbsoluteLayout.SetLayoutBounds` 을 가능한 한 크게를 모든 자식에서 해당 합니다.  

연결 된 바인딩 가능한 속성은 `AbsoluteLayout` 정의 형식의 정적 읽기 전용 필드는 `BindableProperty` 라는 [ `AbsoluteLayout.LayoutBoundsProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty/)합니다. 정적 `AbsoluteLayout.SetLayoutBounds` 메서드는 호출 하 여 구현 `SetValue` 자식으로 `AbsoluteLayout.LayoutBoundsProperty`합니다. 자식에 연결 된 바인딩 가능한 속성 및 해당 값 저장 되는 사전을 포함 되어 있습니다. 레이아웃 중에서 `AbsoluteLayout` 호출 하 여 해당 값을 가져올 수 [ `AbsoluteLayout.GetLayoutBounds` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutBounds/p/Xamarin.Forms.BindableObject/),으로 구현 되는 `GetValue` 호출 합니다.

## <a name="proportional-sizing-and-positioning"></a>비례 크기 조정 및 위치 지정

`AbsoluteLayout` 비례 크기 조정 및 기능을 위치 지정을 구현 합니다. 두 번째 연결된 바인딩 가능 속성을 정의 하는 클래스 [ `LayoutFlagsProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty/), 관련된 정적 메서드와 함께 [ `AbsoluteLayout.SetLayoutFlags` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutFlags/p/Xamarin.Forms.BindableObject/Xamarin.Forms.AbsoluteLayoutFlags/) 및 [ `AbsoluteLayout.GetLayoutFlags` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutFlags/p/Xamarin.Forms.BindableObject/)합니다.

에 대 한 인수 `AbsoluteLayout.SetLayoutFlags` 의 반환 값과 `AbsoluteLayout.GetLayoutFlags` 형식의 값은 [ `AbsoluteLayoutFlags` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayoutFlags/), 다음 멤버로 구성 된 열거:

- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.None/) (0 같음)
- [`XProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.XProportional/) (1)
- [`YProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.YProportional/) (2)
- [`PositionProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional/) (3)
- [`WidthProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional/) (4)
- [`HeightProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional/) (8)
- [`SizeProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional/) (12)
- [`All`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.All/) (\xFFFFFFFF)

C# 비트 OR 연산자를 가진를 결합할 수 있습니다.

이러한 플래그 집합의 특정 속성으로는 `Rectangle` 자식 크기 및 위치를 지정 하는 데 사용 하는 레이아웃 범위 구조 비례적으로 해석 됩니다.

때는 `WidthProportional` 플래그가 설정 되 면는 `Width` 자식은 동일한 너비로 1 이면 값은 `AbsoluteLayout`합니다. 유사한 방법은 높이에 사용 됩니다.

비례 위치는 크기를 고려 합니다. 때는 `XProportional` 플래그가 설정 되 면는 `X` 의 속성은 `Rectangle` 레이아웃은 비례 합니다. 값이 0 이면 자식의 왼쪽 가장자리의 왼쪽된 가장자리에 배치 되는 `AbsoluteLayout`, 있지만 자식의 오른쪽 가장자리의 오른쪽 가장자리에 배치 됩니다 1 이면 위치가 `AbsoluteLayout`의 오른쪽 가장자리를 벗어나지는 `AbsoluteLayout` expec 수 있습니다 t입니다. `X` 0.5의 속성에 중점을 가로로 자식은 `AbsoluteLayout`합니다.

[ **ChessboardProportional** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional) 샘플의 비율 크기 조정 및 위치 지정 사용을 보여 줍니다.

## <a name="working-with-proportional-coordinates"></a>작업에 비례 좌표

는 쉽게 비례 위치와 다르게에서 구현 하는 방법을 생각 하는 `AbsoluteLayout`합니다. 비례 좌표를 사용할 수도 있습니다 위치는 `X` 의 오른쪽 가장자리에 대해 자식의 왼쪽된 가장자리 (오른쪽 가장자리 아님)를 배치 하는 1의 속성이 고 `AbsoluteLayout`합니다.

이 대체 위치 지정 체계를 호출할 수 있습니다 "소수 자식 좌표입니다." 에 필요한 레이아웃 범위를 소수 자식 좌표에서 변환할 수 있습니다 `AbsoluteLayout` 다음 수식을 사용 하 여:

layoutBounds.X = (fractionalChildCoordinate.X / (1-layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y / (1-layoutBounds.Height))

[ **ProportionalCoordinateCalc** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc) 이 샘플을 보여 줍니다.

## <a name="absolutelayout-and-xaml"></a>AbsoluteLayout 및 XAML

사용할 수는 `AbsoluteLayout` XAML에서의 자식에 연결 된 바인딩 가능한 속성을 설정 하 고는 `AbsoluteLayout` 의 특성 값을 사용 하 여 `AbsoluteLayout.LayoutBounds` 및 `AbsoluteLayout.LayoutFlags`합니다. 이 확인할는 [ **AbsoluteXamlDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo) 및 [ **ChessboardXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml) 샘플입니다. 후자의 프로그램 32에 포함 되어 `BoxView` 요소 하지만 사용 하 여 암시적 `Style` 포함 하는 `AbsoluteLayout.LayoutFlags` 속성을 최소한으로 피드백을 유지 합니다.

클래스 이름, 점, 및 속성 이름으로 구성 된 XAML에서이 특성 값이 *항상* 연결된 된 바인딩 가능한 속성입니다.

## <a name="overlays"></a>오버레이

사용할 수 있습니다 `AbsoluteLayout` 생성 하는 *오버레이*, 사용자 페이지에 일반 컨트롤와 상호 작용을 방지 하기 위해 아마도 다른 제어 기능과 함께 페이지를 포함 하 합니다. 

[ **SimpleOverlay** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay) 샘플이이 기술을 보여 줍니다 방법과 보여 줍니다는 [ `ProgressBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/), 프로그램 완료 된 범위를 표시 하는 작업입니다.

## <a name="some-fun"></a>재미

[ **DotMatrixClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock) 샘플 시뮬레이션된 5 무휴 도트 매트릭스 표시와 함께 현재 시간을 표시 합니다. 각 점이 `BoxView` (그 중 228는) 크기 및 위치에 `AbsoluteLayout`합니다.

[![도트 매트릭스 클록의 삼중 스크린 샷](images/ch14fg08-small.png "도트 매트릭스 클록")](images/ch14fg08-large.png#lightbox "도트 매트릭스 클록")

[ **BouncingText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText) 두 프로그램 애니메이션 `Label` 화면 가로 및 세로로 bounce 개체입니다.



## <a name="related-links"></a>관련 링크

- [14 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [14 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
