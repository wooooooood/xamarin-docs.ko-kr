---
제목: "iOS에서 ScrollView 콘텐츠 터치" 설명: "플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 ScrollView 터치 제스처를 처리 하거나 콘텐츠에 전달 하는지 여부를 제어 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다. "
assetid: 99F823DB-B379-40F0-A343-A9783C341120: xamarin-forms author: davidbritch: dabritch:: 10/24/2018-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="scrollview-content-touches-on-ios"></a>IOS에서 ScrollView 콘텐츠 터치

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

암시적 타이머는 [`ScrollView`](xref:Xamarin.Forms.ScrollView) `ScrollView` 제스처를 처리 하거나 해당 콘텐츠에 전달 해야 하는지 여부에 상관 없이 타이머 범위 내의 사용자 작업을 기반으로 하 여 iOS의에서 터치 제스처가 시작 되 고 결정 되는 경우에 트리거됩니다. 기본적으로 iOS는 `ScrollView` 콘텐츠를 지연 하지만,이로 인해 일부 상황에서는 콘텐츠가 제스처를 적용 `ScrollView` 하지 않는 경우 문제가 발생할 수 있습니다. 따라서이 플랫폼 특정은가 터치 제스처를 처리 하는지 아니면 콘텐츠에 전달 하는지 여부를 제어 `ScrollView` 합니다. 연결 된 속성을 값으로 설정 하 여 XAML에서 사용 됩니다 `ScrollView.ShouldDelayContentTouches` `boolean` .

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <MasterDetailPage.Master>
        <ContentPage Title="Menu" BackgroundColor="Blue" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <ContentPage>
            <ScrollView x:Name="scrollView" ios:ScrollView.ShouldDelayContentTouches="false">
                <StackLayout Margin="0,20">
                    <Slider />
                    <Button Text="Toggle ScrollView DelayContentTouches" Clicked="OnButtonClicked" />
                </StackLayout>
            </ScrollView>
        </ContentPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

`ScrollView.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. `ScrollView.SetShouldDelayContentTouches`네임 스페이스의 메서드를 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 사용 하 여이 [`ScrollView`](xref:Xamarin.Forms.ScrollView) 터치 제스처를 처리 하는지 또는 콘텐츠에 전달 하는지 여부를 제어 합니다. 또한 메서드를 `SetShouldDelayContentTouches` 호출 `ShouldDelayContentTouches` 하 여 콘텐츠 터치가 지연 되었는지 여부를 반환 하 여 콘텐츠를 지연 시키는 것을 전환 하는 데 메서드를 사용할 수 있습니다.

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

결과적으로 [`ScrollView`](xref:Xamarin.Forms.ScrollView) ,이 시나리오에서가 [`Slider`](xref:Xamarin.Forms.Slider) 의 페이지 보다는 제스처를 수신 하기 때문에에서 콘텐츠가 수신 되는 지연을 사용 하지 않도록 설정할 수 있습니다 [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) .

[![](scrollview-content-touches-images/scrollview-delay-content-touches.png "ScrollView Delay Content Touches Platform-Specific")](scrollview-content-touches-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Platform-Specific")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
