---
제목: "Android에서의 요소 권한 상승" 설명: "플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 API 21 이상을 대상으로 하는 응용 프로그램에서 VisualElements의 상승을 제어 하는 Android 플랫폼 관련 기능을 사용 하는 방법을 설명 합니다. "
assetid: 5BFD6175-2BBD-41CD-B8F9-521B4750B708: xamarin-forms author: davidbritch: dabritch:: 07/10/2018-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="visualelement-elevation-on-android"></a>Android에서 VisualElement 권한 상승

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 Android 플랫폼별는 API 21 이상을 대상으로 하는 응용 프로그램에서 시각적 요소의 권한 상승 또는 Z 순서를 제어 하는 데 사용 됩니다. 시각적 요소를 상승 하면 Z 값이 더 높은 시각적 요소가 occluding 시각적 요소를 사용 하 여 그리기 순서를 결정 합니다. 연결 된 속성을 값으로 설정 하 여 XAML에서 사용 됩니다 `VisualElement.Elevation` `boolean` .

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             Title="Elevation">
    <StackLayout>
        <Grid>
            <Button Text="Button Beneath BoxView" />
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>        
        <Grid Margin="0,20,0,0">
            <Button Text="Button Above BoxView - Click Me" android:VisualElement.Elevation="10"/>
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

public class AndroidElevationPageCS : ContentPage
{
    public AndroidElevationPageCS()
    {
        ...
        var aboveButton = new Button { Text = "Button Above BoxView - Click Me" };
        aboveButton.On<Android>().SetElevation(10);

        Content = new StackLayout
        {
            Children =
            {
                new Grid
                {
                    Children =
                    {
                        new Button { Text = "Button Beneath BoxView" },
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                },
                new Grid
                {
                    Margin = new Thickness(0,20,0,0),
                    Children =
                    {
                        aboveButton,
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                }
            }
        };
    }
}
```

`Button.On<Android>`메서드는이 플랫폼별가 Android 에서만 실행 되도록 지정 합니다. `VisualElement.SetElevation`네임 스페이스의 메서드를 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 사용 하 여 시각적 요소의 승격을 nullable로 설정 합니다 `float` . 또한 `VisualElement.GetElevation` 메서드를 사용 하 여 시각적 요소의 상승 값을 검색할 수 있습니다.

그 결과, Z 값이 더 높은 시각적 요소가 Z 값이 작은 시각적 요소를 려 시각적 요소의 상승을 제어할 수 있습니다. 따라서이 예제에서 두 번째는 [`Button`](xref:Xamarin.Forms.Button) [`BoxView`](xref:Xamarin.Forms.BoxView) 상승 값이 더 높기 때문에에서 렌더링 됩니다.

![](visualelement-elevation-images/elevation.png)

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
