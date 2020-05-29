---
제목: ' Xamarin.Forms ScrollView ' 설명: '이 문서에서는 ScrollView 클래스를 사용 하 여 Xamarin.Forms 한 화면에만 맞지 않는 레이아웃을 제공 하는 방법을 설명 합니다.
ms. prod: assetid: ms. 기술: author: ms author: ms. date: no loc:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---

# <a name="xamarinforms-scrollview"></a>Xamarin.FormsScrollView

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`ScrollView`](xref:Xamarin.Forms.ScrollView)레이아웃을 포함 하며 화면을 스크롤할 수 있습니다. `ScrollView`는 또한 키보드를 표시할 때 보기가 화면에 표시 되는 부분으로 자동으로 이동 하도록 하는 데 사용 됩니다.

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms Layouts")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms Layouts")

## <a name="purpose"></a>목적

[`ScrollView`](xref:Xamarin.Forms.ScrollView)를 사용 하 여 더 큰 보기가 작은 휴대폰에서 잘 표시 되도록 할 수 있습니다. 예를 들어 iPhone 6s에서 작동 하는 레이아웃은 iPhone 4s에서 잘릴 수 있습니다. 를 사용 하면 `ScrollView` 레이아웃의 잘린 부분이 더 작은 화면에 표시 될 수 있습니다.

## <a name="usage"></a>사용량

> [!NOTE]
> [`ScrollView`](xref:Xamarin.Forms.ScrollView)개체는 중첩 될 수 없습니다. 또한 및과 `ScrollView` 같이 스크롤 기능을 제공 하는 다른 컨트롤에는를 중첩할 수 없습니다 `ListView` `WebView` .

[`ScrollView`](xref:Xamarin.Forms.ScrollView)`Content`단일 뷰나 레이아웃으로 설정할 수 있는 속성을 노출 합니다. 매우 큰 boxView를 사용 하 여 다음을 수행 하는 레이아웃의 예를 살펴보겠습니다 `Entry` .

```xaml
<ContentPage.Content>
    <ScrollView>
        <StackLayout>
            <BoxView BackgroundColor="Red" HeightRequest="600" WidthRequest="150" />
            <Entry />
        </StackLayout>
    </ScrollView>
</ContentPage.Content>
```

C#:

```csharp
public class ScrollingDemoCode : ContentPage
{
    public ScrollingDemoCode()
    {
        StackLayout stackLayout = new StackLayout();
        stackLayout.Children.Add(new BoxView { BackgroundColor = Color.Red, HeightRequest = 600, WidthRequest = 150 });
        stackLayout.Children.Add(new Entry());
        ScrollView scrollView = new ScrollView();
        scrollView.Content = stackLayout;
        Content = scrollView;
    }
}
```

사용자가 아래로 스크롤하면만 `BoxView` 표시 됩니다.

![](scroll-view-images/scroll-start.png "BoxView in ScrollView")

사용자가에 텍스트를 입력 하기 시작 하면 `Entry` 뷰가 화면에 계속 표시 되도록 스크롤됩니다.

![](scroll-view-images/scroll-end.png "Entry in ScrollView")

## <a name="properties"></a>속성

[`ScrollView`](xref:Xamarin.Forms.ScrollView)는 다음 속성을 정의 합니다.

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty)[`Size`](xref:Xamarin.Forms.Size)콘텐츠의 크기를 나타내는 값을 가져옵니다.
- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty)[`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation)의 스크롤 방향을 나타내는 열거형 값을 가져오거나 설정 합니다 `ScrollView` .
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty)`double`현재 X 스크롤 위치를 나타내는를 가져옵니다.
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty)`double`현재 Y 스크롤 위치를 나타내는를 가져옵니다.
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty)[`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility)가로 스크롤 막대가 표시 되는 때를 나타내는 값을 가져오거나 설정 합니다.
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty)[`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility)세로 스크롤 막대가 표시 될 때를 나타내는 값을 가져오거나 설정 합니다.

> [!NOTE]
> 속성을로 설정 하 여 스크롤을 사용 하지 않도록 설정할 수 있습니다 [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) `Neither` .

## <a name="methods"></a>방법

[`ScrollView`](xref:Xamarin.Forms.ScrollView)좌표를 `ScrollToAsync` 사용 하거나 표시 해야 하는 특정 뷰를 지정 하 여 뷰를 스크롤 하는 데 사용할 수 있는 메서드를 제공 합니다.

좌표를 사용 하는 경우 `x` `y` 스크롤에 애니메이션을 적용 해야 하는지 여부를 나타내는 부울과 함께 및 좌표를 지정 합니다.

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

> [!IMPORTANT]
> `ScrollToAsync`속성이로 설정 된 경우이 메서드는 스크롤을 발생 시 지 않습니다 [`ScrollView.Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) `Neither` .

특정 요소로 스크롤할 때 `ScrollToPosition` 열거형은 뷰에서 요소가 표시 되는 위치를 지정 합니다.

- **가운데** &ndash; 요소를 뷰의 보이는 부분 가운데로 스크롤합니다.
- **종료** &ndash; 요소를 뷰의 표시 되는 부분 끝으로 스크롤합니다.
- **Makevisible** &ndash; 뷰 내에 표시 되도록 요소를 스크롤합니다.
- **시작** &ndash; 요소를 뷰의 표시 되는 부분에 대 한 시작 부분으로 스크롤합니다.

`IsAnimated`속성은 뷰를 스크롤 하는 방법을 지정 합니다. 로 설정 하면 `true` 콘텐츠를 즉시 뷰로 이동 하는 것이 아니라 부드러운 애니메이션이 사용 됩니다.

## <a name="events"></a>이벤트

[`ScrollView`](xref:Xamarin.Forms.ScrollView)는 하나의 이벤트만 정의 `Scrolled` 합니다. `Scrolled`뷰가 스크롤을 마치면 발생 합니다. 에 대 한 이벤트 처리기는 `Scrolled` `ScrolledEventArgs` 및 속성이 있는를 사용 합니다 `ScrollX` `ScrollY` . 다음은의 현재 스크롤 위치를 사용 하 여 레이블을 업데이트 하는 방법을 보여 줍니다 `ScrollView` .

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

목록의 끝에서 스크롤할 때 바운스 효과 때문에 스크롤 위치는 음수일 수 있습니다.

## <a name="related-links"></a>관련 링크

- [레이아웃 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble 예제 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
