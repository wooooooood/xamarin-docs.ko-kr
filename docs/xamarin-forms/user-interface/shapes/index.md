---
title: Xamarin.Forms셰이프도
description: Xamarin.Forms도형은 화면에 모양을 그릴 수 있는 뷰 형식입니다.
ms.prod: xamarin
ms.assetid: 4E749FE8-852C-46DA-BB1E-652936106357
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fdab7d422040aee5a00b16bb1e301f917b0c4883
ms.sourcegitcommit: ef3d4a70e70927c4f231b763842c5355f1571d15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243765"
---
# <a name="xamarinforms-shapes"></a>Xamarin.Forms셰이프도

![](~/media/shared/preview.png "This API is currently pre-release")

는 `Shape` [`View`](xref:Xamarin.Forms.View) 화면에 모양을 그릴 수 있는의 형식입니다. `Shape``Shape`클래스는 클래스에서 파생 되므로 레이아웃 클래스 및 대부분의 컨트롤 내에서 개체를 사용할 수 있습니다 `View` .

Xamarin.Forms셰이프는 `Xamarin.Forms.Shapes` iOS, Android, macOS, 유니버설 Windows 플랫폼 (UWP) 및 Windows Presentation Foundation (WPF)의 네임 스페이스에서 사용할 수 있습니다.

> [!IMPORTANT]
> Xamarin.Forms도형은 현재 실험적 이며 플래그를 설정 하는 방법 으로만 사용할 수 있습니다 `Shapes_Experimental` . 자세한 내용은 [실험적 플래그](~/xamarin-forms/internals/experimental-flags.md)를 참조 하세요.

`Shape`는 다음 속성을 정의합니다.

- `Aspect`형식의은 `Stretch` 셰이프가 할당 된 공간을 채우는 방법을 설명 합니다. 이 속성의 기본값은 `Stretch.None`입니다.
- `Fill`형식의는 [`Color`](xref:Xamarin.Forms.Color) 도형의 내부를 그리는 데 사용 되는 색을 나타냅니다.
- `Stroke`형식의는 [`Color`](xref:Xamarin.Forms.Color) 도형의 윤곽선을 그리는 데 사용 되는 색을 나타냅니다.
- `StrokeDashArray`는 `DoubleCollection` `double` 도형의 윤곽선을 지정 하는 데 사용 되는 대시와 간격의 패턴을 나타내는 값의 컬렉션을 나타내는 형식의입니다.
- `StrokeDashOffset`형식의는 대시 `double` 패턴 내에서 대시가 시작 되는 거리를 지정 합니다. 이 속성의 기본값은 0.0입니다.
- `StrokeLineCap`형식의는 `PenLineCap` 선 또는 세그먼트의 시작과 끝에 있는 셰이프를 설명 합니다. 이 속성의 기본값은 `PenLineCap.Flat`입니다.
- `StrokeLineJoin`형식의는 `PenLineJoin` 도형의 꼭 짓 점에 사용 되는 조인 유형을 지정 합니다. 이 속성의 기본값은 `PenLineJoin.Miter`입니다.
- `StrokeThickness`형식의는 `double` 셰이프 윤곽선의 너비를 나타냅니다. 이 속성의 기본값은 1.0입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

Xamarin.Forms클래스에서 파생 되는 개체의 수를 정의 `Shape` 합니다. 여기에는,,,, `Ellipse` `Line` 및가 `Path` `Polygon` `Polyline` `Rectangle` 있습니다.

## <a name="paint-shapes"></a>그리기 모양

[`Color`](xref:Xamarin.Forms.Color)개체는 모양의 및을 그리는 데 사용 `Stroke` 됩니다 `Fill` .

```xaml
<Ellipse Fill="DarkBlue"
         Stroke="Red"
         StrokeThickness="4"
         WidthRequest="150"
         HeightRequest="50"
         HorizontalOptions="Start" />
```

이 예제에서는의 스트로크 및 채우기 `Ellipse` 가 지정 됩니다.

![그리기 모양](images/ellipse.png "그리기 모양")

> [!IMPORTANT]
> 에 값을 지정 하지 [`Color`](xref:Xamarin.Forms.Color) `Stroke` 않거나 `StrokeThickness` 를 0으로 설정 하면 셰이프 주위의 테두리가 그려지지 않습니다.

유효한 값에 대 한 자세한 내용은 [`Color`](xref:Xamarin.Forms.Color) [의 Xamarin.Forms 색 ](~/xamarin-forms/user-interface/colors.md)을 참조 하세요.

## <a name="stretch-shapes"></a>Stretch 셰이프

`Shape`개체에는 `Aspect` 형식의 속성이 `Stretch` 있습니다. 이 속성은 개체의 `Shape` 콘텐츠를 `Shape` 개체의 레이아웃 공간에 채우도록 스트레치 하는 방법을 결정 합니다. `Shape`개체의 레이아웃 공간은 `Shape` Xamarin.Forms 명시적 `WidthRequest` 및 `HeightRequest` 설정이 나 `HorizontalOptions` 및 설정으로 인해가 레이아웃 시스템에서 할당 하는 공간 크기입니다 `VerticalOptions` .

`Stretch` 열거형은 다음 멤버를 정의합니다.

- `None`-콘텐츠가 원래 크기를 유지 함을 나타냅니다. 이 값은 `Shape.Aspect` 속성의 기본값입니다.
- `Fill`-대상 크기를 채우도록 콘텐츠의 크기가 조정 됨을 나타냅니다. 가로 세로 비율은 유지되지 않습니다.
- `Uniform`는 가로 세로 비율을 유지 하면서 대상 크기에 맞게 콘텐츠의 크기를 조정 함을 나타냅니다.
- `UniformToFill`는 가로 세로 비율을 유지 하면서 대상 크기를 채우도록 콘텐츠의 크기를 조정 함을 나타냅니다. 대상 사각형의 가로 세로 비율이 원본과 다를 경우 원본 콘텐츠가 대상 크기에 맞게 잘립니다.

다음 XAML은 속성을 설정 하는 방법을 보여 줍니다 `Aspect` .

```xaml
<Path Aspect="Uniform"
      Stroke="Yellow"
      StrokeThickness="1"
      Fill="Red"
      BackgroundColor="LightGray"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100">
    <Path.Data>
        <!-- Path data goes here -->
    </Path.Data>  
</Path>      
```

이 예제에서 개체는 `Path` 하트를 그립니다. `Path`개체의 `WidthRequest` 및 속성은 `HeightRequest` 100 장치 독립적 단위로 설정 되 고 해당 `Aspect` 속성은로 설정 됩니다 `Uniform` . 따라서 개체의 내용은 가로 세로 비율을 유지 하면서 대상 크기에 맞게 크기가 조정 됩니다.

![Stretch 셰이프](images/aspect.png "Stretch 셰이프")

## <a name="draw-dashed-shapes"></a>파선 도형 그리기

`Shape`개체에는 `StrokeDashArray` 형식의 속성이 `DoubleCollection` 있습니다. 이 속성은 `double` 셰이프를 윤곽선으로 표시 하는 데 사용 되는 대시와 간격의 패턴을 나타내는 값의 컬렉션을 나타냅니다. 는 `DoubleCollection` `ObservableCollection` 값의입니다 `double` . `double`컬렉션의 각은 대시 또는 간격의 길이를 지정 합니다. 인덱스 0에 있는 컬렉션의 첫 번째 항목은 대시의 길이를 지정 합니다. 인덱스 1에 있는 컬렉션의 두 번째 항목은 간격의 길이를 지정 합니다. 따라서 짝수 인덱스 값을 가진 개체는 대시를 지정 하 고 홀수 인덱스 값을 가진 개체는 간격을 지정 합니다.

`Shape`개체에는 `StrokeDashOffset` `double` 대시 패턴 내에서 대시가 시작 되는 거리를 지정 하는 형식의 속성도 있습니다. 이 속성을 설정 하지 않으면 실선이 발생 합니다 `Shape` .

및 속성을 설정 하 여 파선 셰이프를 그릴 수 있습니다 `StrokeDashArray` `StrokeDashOffset` . `StrokeDashArray`속성은 하나 이상의 값으로 설정 되어야 하며 `double` 각 쌍은 단일 쉼표 및/또는 하나 이상의 공백으로 구분 됩니다. 예를 들어 "0.5 1.0" 및 "0.5, 1.0"은 모두 유효 합니다.

다음 XAML 예제에서는 파선 사각형을 그리는 방법을 보여 줍니다.

```xaml
<Rectangle Fill="DarkBlue"
           Stroke="Red"
           StrokeThickness="4"
           StrokeDashArray="1,1"
           StrokeDashOffset="6"
           WidthRequest="150"
           HeightRequest="50"
           HorizontalOptions="Start" />
```

이 예제에서는 파선 스트로크를 사용 하 여 채워진 사각형을 그립니다.

![파선 사각형](images/dashed-rectangle.png "파선")

## <a name="control-line-ends"></a>줄 끝 제어

줄에는 시작 단면, 줄 본문 및 끝 캡의 세 부분이 있습니다. 시작 및 끝 캡은 선 또는 세그먼트의 시작과 끝에 있는 셰이프를 설명 합니다.

`Shape`개체에는 `StrokeLineCap` `PenLineCap` 줄의 시작과 끝에 있는 셰이프를 설명 하는 형식의 속성이 있습니다. `PenLineCap` 열거형은 다음 멤버를 정의합니다.

- `Flat`는 줄의 마지막 점을 벗어나 확장 되지 않는 캡을 나타냅니다. 이 값은 선 끝 모양과 유사 하며 속성의 기본값입니다 `StrokeLineCap` .
- `Square`-높이가 선 두께와 같고 길이가 선 두께의 절반에 해당 하는 사각형을 나타내는입니다.
- `Round`-지름이 선 두께와 같은 반원를 나타냅니다.

> [!IMPORTANT]
> `StrokeLineCap`시작점 또는 끝점이 없는 도형에 속성을 설정 하는 경우에는 속성이 적용 되지 않습니다. 예를 들어, 또는에서이 속성을 설정 하면이 속성이 적용 되지 않습니다 `Ellipse` `Rectangle` .

다음 XAML은 속성을 설정 하는 방법을 보여 줍니다 `StrokeLineCap` .

```xaml
<Line X1="0"
      Y1="20"
      X2="300"
      Y2="20"
      StrokeLineCap="Round"
      Stroke="Red"
      StrokeThickness="12" />
```

이 예제에서 빨간색 선은 줄의 시작과 끝에서 반올림 됩니다.

![선 끝이](images/linecap.png "선 끝이")

## <a name="control-line-joins"></a>줄 조인 제어

`Shape`개체에는 `StrokeLineJoin` 형식의 속성이 있으며,이 속성은 `PenLineJoin` 셰이프의 꼭 짓 점에 사용 되는 조인 유형을 지정 합니다. `PenLineJoin` 열거형은 다음 멤버를 정의합니다.

- `Miter`는 정규 각도 버텍스를 나타냅니다. 이 값은 `StrokeLineJoin` 속성의 기본값입니다.
- `Bevel`는 경사진 정점을 나타냅니다.
- `Round`는 반올림 된 꼭 짓 점을 나타냅니다.

다음 XAML은 속성을 설정 하는 방법을 보여 줍니다 `StrokeLineJoin` .

```xaml
<Polyline Points="20 20,250 50,20 120"
          Stroke="DarkBlue"
          StrokeThickness="20"
          StrokeLineJoin="Round" />
```

이 예제에서 진한 파란색 폴리라인는 꼭 짓 점에 둥근 조인이 있습니다.

![줄 조인](images/linejoin.png "줄 조인")

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [색의Xamarin.Forms](~/xamarin-forms/user-interface/colors.md)
