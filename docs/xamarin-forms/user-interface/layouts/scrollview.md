---
제목: " Xamarin.Forms ScrollView" description: " Xamarin.Forms ScrollView은 콘텐츠를 스크롤할 수 있는 레이아웃입니다."
assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E: xamarin-forms author: davidbritch: dabritch:: 05/27/2020-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-scrollview"></a>Xamarin.FormsScrollView

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-scrollviewdemos)

[![Xamarin.FormsScrollView](scrollview-images/layouts.png "[! OP. NO-LOC (Xamarin.ios)] ScrollView")](scrollview-images/layouts-large.png#lightbox "[! OP. NO-LOC (Xamarin.ios)] ScrollView")

[`ScrollView`](xref:Xamarin.Forms.ScrollView)는 콘텐츠를 스크롤할 수 있는 레이아웃입니다. 클래스는 `ScrollView` 클래스에서 파생 [`Layout`](xref:Xamarin.Forms.Layout) 되며 기본적으로 콘텐츠를 세로로 스크롤합니다. 는 `ScrollView` 다른 레이아웃 일 수 있지만 하나의 자식만 가질 수 있습니다.

> [!WARNING]
> [`ScrollView`](xref:Xamarin.Forms.ScrollView)개체는 중첩 될 수 없습니다. 또한 개체는 `ScrollView` 스크롤을 제공 하는 다른 컨트롤 (예:, 및)에 중첩 될 수 없습니다 [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ListView`](xref:Xamarin.Forms.ListView) [`WebView`](xref:Xamarin.Forms.WebView) .

[`ScrollView`](xref:Xamarin.Forms.ScrollView)는 다음 속성을 정의 합니다.

- [`Content`](xref:Xamarin.Forms.ScrollView.Content)형식의은 [`View`](xref:Xamarin.Forms.View) 에 표시할 콘텐츠를 나타냅니다 [`ScrollView`](xref:Xamarin.Forms.ScrollView) .
- [`ContentSize`](xref:Xamarin.Forms.ScrollView)형식의는 [`Size`](xref:Xamarin.Forms.Size) 콘텐츠의 크기를 나타냅니다. 이 속성은 읽기 전용입니다.
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView)형식의는 [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibility) 가로 스크롤 막대가 표시 되는 시간을 나타냅니다.
- [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation)형식의는의 [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) 스크롤 방향을 나타냅니다 [`ScrollView`](xref:Xamarin.Forms.ScrollView) . 이 속성의 기본값은 `Vertical`입니다.
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollX)형식의는 `double` 현재 X 스크롤 위치를 나타냅니다. 이 읽기 전용 속성의 기본값은 0입니다.
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollY)형식의는 `double` 현재 Y 스크롤 위치를 나타냅니다. 이 읽기 전용 속성의 기본값은 0입니다.
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView)형식의는 [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibility) 세로 스크롤 막대가 표시 되는 시간을 나타냅니다.

이러한 속성은 개체에 의해 지원 되며 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 속성을 제외 하 [`Content`](xref:Xamarin.Forms.ScrollView.Content) 고 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

[`Content`](xref:Xamarin.Forms.ScrollView.Content)속성은 [`ContentProperty`](xref:Xamarin.Forms.ContentPropertyAttribute) 클래스의입니다 [`ScrollView`](xref:Xamarin.Forms.ScrollView) . 따라서 XAML에서 명시적으로 설정할 필요는 없습니다.

> [!TIP]
> 가능한 최상의 레이아웃 성능을 얻으려면 [레이아웃 성능 최적화](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance)의 지침을 따르세요.

## <a name="scrollview-as-a-root-layout"></a>루트 레이아웃으로 ScrollView

는 [`ScrollView`](xref:Xamarin.Forms.ScrollView) 다른 레이아웃 일 수 있는 단일 자식만 가질 수 있습니다. 따라서가 `ScrollView` 페이지의 루트 레이아웃이 되는 것이 일반적입니다. 자식 콘텐츠를 스크롤하려면에서 [`ScrollView`](xref:Xamarin.Forms.ScrollView) 해당 콘텐츠의 높이와 자체 높이 사이의 차이를 계산 합니다. 이 차이는에서 콘텐츠를 스크롤할 수 있는 양입니다 `ScrollView` .

는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 대체로의 자식 `ScrollView` 입니다. 이 시나리오에서는의 `ScrollView` 높이가 해당 `StackLayout` 자식의 높이 합계와 동일 하 게 설정 됩니다. 그런 다음에서 `ScrollView` 콘텐츠를 스크롤할 수 있는 정도를 결정할 수 있습니다. 에 대 한 자세한 내용은 `StackLayout` [ Xamarin.Forms stacklayout](stacklayout.md)을 참조 하세요.

> [!CAUTION]
> 세로에서 [`ScrollView`](xref:Xamarin.Forms.ScrollView) `VerticalOptions` 속성을, 또는로 설정 하지 `Start` 마십시오 `Center` `End` . 이렇게 하면 `ScrollView` 이 (가) 0이 될 수 있는 만큼의 높이를 나타냅니다. Xamarin.Forms에서이 대비해 야를 보호 하는 동안 수행 하지 않으려는 항목을 제안 하는 코드를 방지 하는 것이 좋습니다.

다음 XAML 예제에는를 [`ScrollView`](xref:Xamarin.Forms.ScrollView) 페이지의 루트 레이아웃으로 포함 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ScrollViewDemos"
             x:Class="ScrollViewDemos.Views.ColorListPage"
             Title="ScrollView demo">
    <ScrollView>
        <StackLayout BindableLayout.ItemsSource="{x:Static local:NamedColor.All}">
            <BindableLayout.ItemTemplate>
                <DataTemplate>
                    <StackLayout Orientation="Horizontal">
                        <BoxView Color="{Binding Color}"
                                 HeightRequest="32"
                                 WidthRequest="32"
                                 VerticalOptions="Center" />
                        <Label Text="{Binding FriendlyName}"
                               FontSize="24"
                               VerticalOptions="Center" />
                    </StackLayout>
                </DataTemplate>
            </BindableLayout.ItemTemplate>
        </StackLayout>
    </ScrollView>
</ContentPage>
```

이 예제에서의 [`ScrollView`](xref:Xamarin.Forms.ScrollView) 콘텐츠는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 로 정의 된 필드를 표시 하기 위해 바인딩 가능한 레이아웃을 사용 하는로 설정 [`Color`](xref:Xamarin.Forms.Color) Xamarin.Forms 됩니다. 기본적으로는 `ScrollView` 세로로 스크롤되며 추가 콘텐츠가 표시 됩니다.

[![루트 ScrollView 레이아웃의 스크린샷](scrollview-images/root-layout.png "Root ScrollView 레이아웃")](scrollview-images/root-layout-large.png#lightbox "Root ScrollView 레이아웃")

해당하는 C# 코드는 다음과 같습니다.

```csharp
public class ColorListPageCode : ContentPage
{
    public ColorListPageCode()
    {
        DataTemplate dataTemplate = new DataTemplate(() =>
        {
            BoxView boxView = new BoxView
            {
                HeightRequest = 32,
                WidthRequest = 32,
                VerticalOptions = LayoutOptions.Center
            };
            boxView.SetBinding(BoxView.ColorProperty, "Color");

            Label label = new Label
            {
                FontSize = 24,
                VerticalOptions = LayoutOptions.Center
            };
            label.SetBinding(Label.TextProperty, "FriendlyName");

            StackLayout horizontalStackLayout = new StackLayout
            {
                Orientation = StackOrientation.Horizontal,
                Children = { boxView, label }
            };
            return horizontalStackLayout;
        });

        StackLayout stackLayout = new StackLayout();
        BindableLayout.SetItemsSource(stackLayout, NamedColor.All);
        BindableLayout.SetItemTemplate(stackLayout, dataTemplate);

        ScrollView scrollView = new ScrollView { Content = stackLayout };

        Title = "ScrollView demo";
        Content = scrollView;
    }
}
```

바인딩 가능한 레이아웃에 대 한 자세한 내용은 [ Xamarin.Forms 의 바인딩 가능한 레이아웃 ](bindable-layouts.md)을 참조 하세요.

## <a name="scrollview-as-a-child-layout"></a>자식 레이아웃으로 ScrollView

는 [`ScrollView`](xref:Xamarin.Forms.ScrollView) 다른 부모 레이아웃에 대 한 자식 레이아웃 일 수 있습니다.

는 [`ScrollView`](xref:Xamarin.Forms.ScrollView) 대체로의 자식 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 입니다. 에는 해당 `ScrollView` 콘텐츠의 높이와 고유한 높이 간의 차이를 계산 하기 위해 특정 높이가 필요 하며,이는가 해당 콘텐츠를 스크롤할 수 있는 양입니다 `ScrollView` . 가 `ScrollView` 의 자식인 경우 `StackLayout` 특정 높이를 받지 않습니다. 는를 `StackLayout` `ScrollView` 최대한 짧게 만들려고 합니다 .이는 콘텐츠의 높이나 `ScrollView` 0입니다. 이 시나리오를 처리 하려면 `VerticalOptions` 의 속성을 `ScrollView` 로 설정 해야 합니다 `FillAndExpand` . 이렇게 하면에서 `StackLayout` `ScrollView` 다른 자식에 필요 하지 않은 모든 추가 공간을 제공 하 고에 `ScrollView` 특정 높이가 지정 됩니다.

다음 XAML 예제에서는를에 [`ScrollView`](xref:Xamarin.Forms.ScrollView) 대 한 자식 레이아웃으로 포함 합니다 [`StackLayout`](xref:Xamarin.Forms.StackLayout) .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ScrollViewDemos.Views.BlackCatPage"
             Title="ScrollView as a child layout demo">
    <StackLayout Margin="20">
        <Label Text="THE BLACK CAT by Edgar Allan Poe"
               FontSize="Medium"
               FontAttributes="Bold"
               HorizontalOptions="Center" />
        <ScrollView VerticalOptions="FillAndExpand">
            <StackLayout>
                <Label Text="FOR the most wild, yet most homely narrative which I am about to pen, I neither expect nor solicit belief. Mad indeed would I be to expect it, in a case where my very senses reject their own evidence. Yet, mad am I not -- and very surely do I not dream. But to-morrow I die, and to-day I would unburthen my soul. My immediate purpose is to place before the world, plainly, succinctly, and without comment, a series of mere household events. In their consequences, these events have terrified -- have tortured -- have destroyed me. Yet I will not attempt to expound them. To me, they have presented little but Horror -- to many they will seem less terrible than barroques. Hereafter, perhaps, some intellect may be found which will reduce my phantasm to the common-place -- some intellect more calm, more logical, and far less excitable than my own, which will perceive, in the circumstances I detail with awe, nothing more than an ordinary succession of very natural causes and effects." />
                <!-- More Label objects go here -->
            </StackLayout>
        </ScrollView>
    </StackLayout>
</ContentPage>
```

이 예제에는 두 개의 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 개체가 있습니다. 첫 번째는 `StackLayout` [`Label`](xref:Xamarin.Forms.Label) 개체 및를 자식으로 포함 하는 루트 레이아웃 개체입니다 [`ScrollView`](xref:Xamarin.Forms.ScrollView) . 에는 `ScrollView` `StackLayout` `StackLayout` 여러 개체가 포함 된의 콘텐츠로가 있습니다 `Label` . 이렇게 정렬 하면 첫 번째는 `Label` 항상 화면에 표시 되 고 다른 개체에 의해 표시 되는 텍스트는 `Label` 스크롤할 수 있습니다.

[![자식 ScrollView 레이아웃의 스크린샷](scrollview-images/child-layout.png "자식 ScrollView 레이아웃")](scrollview-images/child-layout-large.png#lightbox "자식 ScrollView 레이아웃")

해당하는 C# 코드는 다음과 같습니다.

```csharp
public class BlackCatPageCS : ContentPage
{
    public BlackCatPageCS()
    {
        Label titleLabel = new Label
        {
            Text = "THE BLACK CAT by Edgar Allan Poe",
            // More properties set here to define the Label appearance
        };

        ScrollView scrollView = new ScrollView
        {
            VerticalOptions = LayoutOptions.FillAndExpand,
            Content = new StackLayout
            {
                Children =
                {
                    new Label
                    {
                        Text = "FOR the most wild, yet most homely narrative which I am about to pen, I neither expect nor solicit belief. Mad indeed would I be to expect it, in a case where my very senses reject their own evidence. Yet, mad am I not -- and very surely do I not dream. But to-morrow I die, and to-day I would unburthen my soul. My immediate purpose is to place before the world, plainly, succinctly, and without comment, a series of mere household events. In their consequences, these events have terrified -- have tortured -- have destroyed me. Yet I will not attempt to expound them. To me, they have presented little but Horror -- to many they will seem less terrible than barroques. Hereafter, perhaps, some intellect may be found which will reduce my phantasm to the common-place -- some intellect more calm, more logical, and far less excitable than my own, which will perceive, in the circumstances I detail with awe, nothing more than an ordinary succession of very natural causes and effects.",
                    },
                    // More Label objects go here
                }
            }
        };

        Title = "ScrollView as a child layout demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = { titleLabel, scrollView }
        };
    }
}
```

## <a name="orientation"></a>방향

[`ScrollView`](xref:Xamarin.Forms.ScrollView)에 [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) 는의 스크롤 방향을 나타내는 속성이 있습니다 `ScrollView` . 이 속성은 [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) 다음 멤버를 정의 하는 형식입니다.

- `Vertical`가 세로로 스크롤 됨을 나타냅니다 `ScrollView` . 이 멤버는 속성의 기본값입니다 [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) .
- `Horizontal`가 가로로 스크롤 됨을 나타냅니다 `ScrollView` .
- `Both``ScrollView`가 가로 및 세로로 스크롤 됨을 나타냅니다.
- `Neither`가 스크롤되지 않음을 나타냅니다 `ScrollView` .

> [!TIP]
> 속성을로 설정 하 여 스크롤을 사용 하지 않도록 설정할 수 있습니다 [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) `Neither` .

## <a name="detect-scrolling"></a>스크롤 검색

[`ScrollView`](xref:Xamarin.Forms.ScrollView)[`Scrolled`](xref:Xamarin.Forms.ScrollView.Scrolled)스크롤이 발생 했음을 나타내기 위해 발생 하는 이벤트를 정의 합니다. [`ScrolledEventArgs`](xref:Xamarin.Forms.ScrolledEventArgs)이벤트와 함께 제공 되는 개체의 `Scrolled` `ScrollX` 및 `ScrollY` 속성은 모두 형식 `double` 입니다.

> [!IMPORTANT]
> `ScrolledEventArgs.ScrollX`및 `ScrolledEventArgs.ScrollY` 속성은의 시작 부분으로 스크롤할 때 발생 하는 바운스 효과 때문에 음수 값을 가질 수 있습니다 [`ScrollView`](xref:Xamarin.Forms.ScrollView) .

다음 XAML 예제에서는 [`ScrollView`](xref:Xamarin.Forms.ScrollView) 이벤트에 대 한 이벤트 처리기를 설정 하는을 보여 줍니다 [`Scrolled`](xref:Xamarin.Forms.ScrollView.Scrolled) .

```xaml
<ScrollView Scrolled="OnScrollViewScrolled">
        ...
</ScrollView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
ScrollView scrollView = new ScrollView();
scrollView.Scrolled += OnScrollViewScrolled;
```

이 예제에서 이벤트 `OnScrollViewScrolled` 처리기는 이벤트가 발생할 때 실행 됩니다 [`Scrolled`](xref:Xamarin.Forms.ScrollView.Scrolled) .

```csharp
void OnScrollViewScrolled(object sender, ScrolledEventArgs e)
{
    Console.WriteLine($"ScrollX: {e.ScrollX}, ScrollY: {e.ScrollY}");
}
```

이 예제에서 `OnScrollViewScrolled` 이벤트 처리기는 [`ScrolledEventArgs`](xref:Xamarin.Forms.ScrolledEventArgs) 이벤트와 함께 제공 되는 개체의 값을 출력 합니다.

> [!NOTE]
> [`Scrolled`](xref:Xamarin.Forms.ScrollView.Scrolled)이벤트는 사용자가 시작한 스크롤에 대해 발생 하 고 프로그래밍 방식으로 스크롤됩니다.

## <a name="scroll-programmatically"></a>프로그래밍 방식으로 스크롤

[`ScrollView`](xref:Xamarin.Forms.ScrollView)[`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*)는를 비동기적으로 스크롤 하는 두 가지 메서드를 정의 `ScrollView` 합니다. 오버 로드 중 하나가의 지정 된 위치로 스크롤되고 다른 하나 `ScrollView` 는 지정 된 요소를 뷰로 스크롤합니다. 두 오버 로드에는 스크롤에 애니메이션 효과를 적용할지 여부를 나타내는 데 사용할 수 있는 추가 인수가 있습니다.

> [!IMPORTANT]
> [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*)속성이로 설정 된 경우이 메서드는 스크롤을 발생 시 지 않습니다 [`ScrollView.Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) `Neither` .

### <a name="scroll-a-position-into-view"></a>위치를 뷰로 스크롤

내의 위치는 [`ScrollView`](xref:Xamarin.Forms.ScrollView) [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) 및 인수를 허용 하는 메서드를 사용 하 여로 스크롤할 수 있습니다 `double` `x` `y` . `ScrollView`이라는 세로 개체가 지정 된 `scrollView` 경우 다음 예제에서는의 위쪽에서 150 장치 독립적 단위로 스크롤 하는 방법을 보여 줍니다 `ScrollView` .

```csharp
await scrollView.ScrollToAsync(0, 150, true);
```

에 대 한 세 번째 인수는 [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) `animated` 인수입니다 .이 인수는를 프로그래밍 방식으로 스크롤할 때 스크롤 애니메이션이 표시 되는지 여부를 결정 합니다 [`ScrollView`](xref:Xamarin.Forms.ScrollView) .

### <a name="scroll-an-element-into-view"></a>요소를 뷰로 스크롤

내의 요소는 [`ScrollView`](xref:Xamarin.Forms.ScrollView) [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) 및 인수를 허용 하는 메서드를 사용 하 여 뷰로 스크롤할 수 있습니다 [`Element`](xref:Xamarin.Forms.Element) [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) . 이라는 세로 `ScrollView` `scrollView` 와 라는가 지정 된 [`Label`](xref:Xamarin.Forms.Label) 경우 `label` 다음 예제에서는 요소를 뷰로 스크롤 하는 방법을 보여 줍니다.

```csharp
await scrollView.ScrollToAsync(label, ScrollToPosition.End, true);
```

에 대 한 세 번째 인수는 [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) `animated` 인수입니다 .이 인수는를 프로그래밍 방식으로 스크롤할 때 스크롤 애니메이션이 표시 되는지 여부를 결정 합니다 [`ScrollView`](xref:Xamarin.Forms.ScrollView) .

요소를 뷰로 스크롤할 때 스크롤이 완료 된 후 요소의 정확한 위치는 메서드의 두 번째 인수인를 사용 하 여 설정할 수 있습니다 `position` [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) . 이 인수는 열거형 멤버를 허용 합니다 [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) .

- `MakeVisible`에 표시 될 때까지 요소를 스크롤 함을 나타냅니다 `ScrollView` .
- `Start`요소를의 시작 부분으로 스크롤 함을 나타냅니다 `ScrollView` .
- `Center`요소를의 가운데로 스크롤 해야 함을 나타냅니다 `ScrollView` .
- `End`요소를의 끝으로 스크롤 해야 함을 나타냅니다 `ScrollView` .

## <a name="scroll-bar-visibility"></a>스크롤 막대 표시 여부

[`ScrollView`](xref:Xamarin.Forms.ScrollView)[`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView)바인딩 가능한 [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView) 속성에 의해 지원 되는 및 속성을 정의 합니다. 이러한 속성은 [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibility) 가로 또는 세로 스크롤 막대를 표시할지 여부를 나타내는 열거형 값을 가져오거나 설정 합니다. `ScrollBarVisibility` 열거형은 다음 멤버를 정의합니다.

- `Default`플랫폼의 기본 스크롤 막대 동작을 나타내며 및 속성의 기본값입니다 `HorizontalScrollBarVisibility` `VerticalScrollBarVisibility` .
- `Always`뷰가 뷰에 맞는 경우에도 스크롤 막대가 표시 됨을 나타냅니다.
- `Never`콘텐츠가 뷰에 맞지 않는 경우에도 스크롤 막대가 표시 되지 않음을 나타냅니다.

## <a name="related-links"></a>관련 링크

- [ScrollView 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-scrollviewdemos)
- [Xamarin.FormsStackLayout](stacklayout.md)
- [바인딩 가능한 레이아웃Xamarin.Forms](bindable-layouts.md)
