---
title: Xamarin.ios IndicatorView
description: IndicatorView는 CarouselView에서 항목 수 및 현재 위치를 나타내는 표시기를 표시 하는 컨트롤입니다.
ms.prod: xamarin
ms.assetId: BBCC223B-4B02-46B7-80BB-EE0E86A67CE2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/17/2019
ms.openlocfilehash: 6b7845011470d83d8f2187e0227950c23e46d52d
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490519"
---
# <a name="xamarinforms-indicatorview"></a>Xamarin.ios IndicatorView

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)

`IndicatorView`는 항목의 수 및 현재 위치를 나타내는 표시기를 `CarouselView`에 표시 하는 컨트롤입니다.

[![IOS 및 Android에서 CarouselView 및 IndicatorView의 스크린샷](indicatorview-images/circles.png "IndicatorView 원")](indicatorview-images/circles-large.png#lightbox "IndicatorView 원")

`IndicatorView`는 iOS 및 Android 플랫폼에서 Xamarin. 양식 4.4에서 사용할 수 있습니다. 그러나 현재 실험적 이며 `Forms.Init`를 호출 하기 전에 iOS의 `AppDelegate` 클래스 또는 Android의 `MainActivity` 클래스에 다음 코드 줄을 추가 하 여 사용할 수 있습니다.

```csharp
Forms.SetFlags("IndicatorView_Experimental");
```

`IndicatorView` 다음 속성을 정의합니다.

- `int`형식의 `Count`표시기의 수입니다.
- `bool`형식의 `HideSingle`는 하나만 있을 때 표시기를 숨길지 여부를 나타냅니다. 기본값은 `true`여야 합니다.
- `Color`형식의 `IndicatorColor`표시기 색입니다.
- `double`형식의 `IndicatorSize`표시기 크기입니다. 기본값은 6.0입니다.
- `Layout<View>`형식의 `IndicatorLayout``IndicatorView`을 렌더링 하는 데 사용 되는 레이아웃 클래스를 정의 합니다. 이 속성은 Xamarin.ios로 설정 되며 일반적으로 개발자가 설정할 필요가 없습니다.
- `DataTemplate`형식의 `IndicatorTemplate`각 표시기의 모양을 정의 하는 템플릿입니다.
- `IndicatorShape`형식의 `IndicatorsShape`각 표시기의 모양입니다.
- `IEnumerable`형식의 `ItemsSource`이며, 표시기가 표시 될 컬렉션입니다. 이 속성은 `ItemsSourceBy` 속성이 설정 될 때 자동으로 설정 됩니다.
- `VisualElement`형식의 `ItemsSourceBy`표시기를 표시할 `CarouselView` 개체입니다.
- `int`형식의 `MaximumVisible`표시 되는 최대 표시기 수입니다. 기본값은 `int.MaxValue`여야 합니다.
- 현재 선택한 지표 인덱스를 `int`형식의 `Position`입니다. 이 속성은 `TwoWay` 바인딩을 사용 합니다. 이 속성은 `ItemsSourceBy` 속성이 설정 될 때 자동으로 설정 됩니다.
- `Color`형식의 `SelectedIndicatorColor``CarouselView`의 현재 항목을 나타내는 표시기 색입니다.

이러한 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원 됩니다. 즉, 데이터 바인딩의 대상이 될 수 있고 스타일을 지정할 수 있습니다.

## <a name="create-an-indicatorview"></a>IndicatorView 만들기

다음 예제에서는 XAML로 `IndicatorView`를 인스턴스화하는 방법을 보여 줍니다.

```xaml
<StackLayout>
    <CarouselView x:Name="carouselView"
                  ItemsSource="{Binding Monkeys}">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView ItemsSourceBy="carouselView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

이 예에서는 `IndicatorView` `CarouselView`아래에서 렌더링 되며 `CarouselView`의 각 항목에 대 한 표시기가 표시 됩니다. `IndicatorView` `ItemsSourceBy` 속성을 `CarouselView` 개체로 설정 하 여 데이터로 채워집니다. 각 표시기는 연한 회색 원 이지만 `CarouselView`의 현재 항목을 나타내는 표시기는 진한 회색입니다.

> [!IMPORTANT]
> `ItemsSourceBy` 속성을 설정 하면 `Position` 속성이 `CarouselView.Position` 속성에 바인딩하고 `ItemsSource` 속성은 `CarouselView.ItemsSource` 속성에 바인딩합니다.

## <a name="change-indicator-shape"></a>표시기 모양 변경

`IndicatorView` 클래스에는 표시기의 모양을 나타내는 `IndicatorsShape` 속성이 있습니다. 이 속성은 `IndicatorShape` 열거형 멤버 중 하나로 설정할 수 있습니다.

- `Circle` 표시기 셰이프가 원형이 되도록 지정 합니다. 이 값은 `IndicatorView.IndicatorsShape` 속성의 기본값입니다.
- `Square` 표시기 셰이프가 사각형 임을 나타냅니다.

다음 예에서는 사각형 표시기를 사용 하도록 구성 된 `IndicatorView`를 보여 줍니다.

```xaml
<IndicatorView IndicatorsShape="Square"
               ItemsSourceBy="carouselView"
               IndicatorColor="LightGray"
               SelectedIndicatorColor="DarkGray" />
```

## <a name="define-indicator-appearance"></a>표시기 모양 정의

`IndicatorView.IndicatorTemplate` 속성을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)설정 하 여 각 표시기의 모양을 정의할 수 있습니다.

```xaml
<StackLayout>
    <CarouselView x:Name="carouselView"
                  ItemsSource="{Binding Monkeys}">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView ItemsSourceBy="carouselView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="Black"
                   HorizontalOptions="Center">
        <IndicatorView.IndicatorTemplate>
            <DataTemplate>
                <Image Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=12}" />
            </DataTemplate>
        </IndicatorView.IndicatorTemplate>
    </IndicatorView>
</StackLayout>
```

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 에 지정 된 요소는 각 표시기의 모양을 정의 합니다. 이 예제에서 각 표시기는 `FontImage` 태그 확장을 사용 하 여 글꼴 아이콘을 표시 하는 [`Image`](xref:Xamarin.Forms.Image) 입니다.

다음 스크린샷은 글꼴 아이콘을 사용 하 여 렌더링 된 표시기를 보여 줍니다.

[![IOS 및 Android에서 템플릿 IndicatorView의 스크린샷](indicatorview-images/templated.png "템플릿 기반 IndicatorView")](indicatorview-images/templated-large.png#lightbox "템플릿 기반 IndicatorView")

`FontImage` 태그 확장에 대 한 자세한 내용은 [FontImage 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [IndicatorView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)
- [FontImage 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)
