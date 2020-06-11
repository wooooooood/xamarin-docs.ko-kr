---
제목: " Xamarin.Forms IndicatorView" description: "IndicatorView은 CarouselView에 있는 항목의 수 및 현재 위치를 나타내는 표시기를 표시 하는 컨트롤입니다."
assetId: BBCC223B-4B02-46B7-80BB-EE0E86A67CE2: xamarin-forms author: davidbritch: dabritch:: 02/27/2020-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-indicatorview"></a>Xamarin.FormsIndicatorView

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)

는 `IndicatorView` 항목의 수 및 현재 위치를 나타내는 표시기를에 표시 하는 컨트롤입니다 `CarouselView` .

[![IOS 및 Android에서 CarouselView 및 IndicatorView의 스크린샷](indicatorview-images/circles.png "IndicatorView 원")](indicatorview-images/circles-large.png#lightbox "IndicatorView 원")

`IndicatorView`는 Xamarin.Forms iOS 및 Android 플랫폼에서 4.4, 유니버설 Windows 플랫폼에서 4.5에 제공 됩니다. 그러나 현재 실험적 이며 iOS의 클래스에 다음 코드 줄을 추가 `AppDelegate` 하거나를 `MainActivity` 호출 하기 전에 Android의 클래스에 다음 코드 줄을 추가 하는 방법 으로만 사용할 수 있습니다 `Forms.Init` .

```csharp
Forms.SetFlags("IndicatorView_Experimental");
```

`IndicatorView`는 다음 속성을 정의합니다.

- `Count`형식의, `int` 표시기 수를 표시 합니다.
- `HideSingle`형식의는 `bool` 하나만 있을 때 표시기를 숨길지 여부를 나타냅니다. 기본값은 `true`입니다.
- `IndicatorColor`형식의, `Color` 표시기 색입니다.
- `IndicatorSize`형식의, `double` 표시기 크기를 표시 합니다. 기본값은 6.0입니다.
- `IndicatorLayout`형식의는를 `Layout<View>` 렌더링 하는 데 사용 되는 레이아웃 클래스를 정의 합니다 `IndicatorView` . 이 속성은에 의해 설정 되며 Xamarin.Forms 일반적으로 개발자가 설정할 필요가 없습니다.
- `IndicatorTemplate``DataTemplate`각 표시기의 모양을 정의 하는 템플릿인 형식의입니다.
- `IndicatorsShape`형식의, `IndicatorShape` 각 표시기의 모양입니다.
- `ItemsSource`형식의 이며, `IEnumerable` 표시기가 표시 될 컬렉션입니다. 이 속성은 속성이 설정 될 때 자동으로 설정 됩니다 `CarouselView.IndicatorView` .
- `MaximumVisible`형식의, `int` 표시 가능한 최대 지표 수입니다. 기본값은 `int.MaxValue`입니다.
- `Position`형식의, `int` 현재 선택한 지표 인덱스입니다. 이 속성은 `TwoWay` 바인딩을 사용 합니다. 이 속성은 속성이 설정 될 때 자동으로 설정 됩니다 `CarouselView.IndicatorView` .
- `SelectedIndicatorColor`형식의,의 `Color` 현재 항목을 나타내는 표시기의 색입니다 `CarouselView` .

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

## <a name="create-an-indicatorview"></a>IndicatorView 만들기

다음 예제에서는 XAML로를 인스턴스화하는 방법을 보여 줍니다 `IndicatorView` .

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

이 예제에서는의 `IndicatorView` `CarouselView` 각 항목에 대 한 표시기를 사용 하 여 아래에 렌더링 됩니다 `CarouselView` . 는 `IndicatorView` 속성을 개체로 설정 하 여 데이터로 채워집니다 `CarouselView.IndicatorView` `IndicatorView` . 각 표시기는 연한 회색 원 이지만의 현재 항목을 나타내는 표시기는 `CarouselView` 진한 회색입니다.

> [!IMPORTANT]
> 속성을 설정 하면 속성이 속성에 바인딩되어 속성에 속성 `CarouselView.IndicatorView` `IndicatorView.Position` `CarouselView.Position` 바인딩이 생성 `IndicatorView.ItemsSource` `CarouselView.ItemsSource` 됩니다.

## <a name="change-indicator-shape"></a>표시기 모양 변경

클래스에는 `IndicatorView` `IndicatorsShape` 표시기의 모양을 결정 하는 속성이 있습니다. 이 속성은 열거형 멤버 중 하나로 설정할 수 있습니다 `IndicatorShape` .

- `Circle`표시기 셰이프를 원형으로 지정 합니다. 이 값은 `IndicatorView.IndicatorsShape` 속성의 기본값입니다.
- `Square`표시기 셰이프가 정사각형 임을 나타냅니다.

다음 예제에서는 `IndicatorView` 사각형 표시기를 사용 하도록 구성 된를 보여 줍니다.

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorsShape="Square"
               IndicatorColor="LightGray"
               SelectedIndicatorColor="DarkGray" />
```

## <a name="change-indicator-size"></a>표시기 크기 변경

클래스에는 `IndicatorView` `IndicatorSize` `double` 장치 독립적 단위의 표시기 크기를 결정 하는 형식의 속성이 있습니다. 이 속성의 기본값은 6.0입니다.

다음 예제에서는 `IndicatorView` 더 큰 표시기를 표시 하도록 구성 된를 보여 줍니다.

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorSize="18" />
```

## <a name="limit-the-number-of-indicators-displayed"></a>표시 되는 표시기 수 제한

클래스에는 표시 되는 `IndicatorView` `MaximumVisible` `int` 최대 표시기 수를 결정 하는 형식의 속성이 있습니다.

다음 예제에서는 `IndicatorView` 최대 6 개의 표시기를 표시 하도록 구성 된를 보여 줍니다.

```xaml
<IndicatorView x:Name="indicatorView"
               MaximumVisible="6" />
```

## <a name="define-indicator-appearance"></a>표시기 모양 정의

속성을로 설정 하 여 각 표시기의 모양을 정의할 수 있습니다 `IndicatorView.IndicatorTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
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

에서 지정 된 요소는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 각 표시기의 모양을 정의 합니다. 이 예제에서 각 표시기는 [`Image`](xref:Xamarin.Forms.Image) 태그 확장을 사용 하 여 글꼴 아이콘을 표시 하는입니다 `FontImage` .

다음 스크린샷은 글꼴 아이콘을 사용 하 여 렌더링 된 표시기를 보여 줍니다.

[![IOS 및 Android에서 템플릿 IndicatorView의 스크린샷](indicatorview-images/templated.png "템플릿 기반 IndicatorView")](indicatorview-images/templated-large.png#lightbox "템플릿 기반 IndicatorView")

태그 확장에 대 한 자세한 내용은 `FontImage` [FontImage 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [IndicatorView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)
- [FontImage 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)
