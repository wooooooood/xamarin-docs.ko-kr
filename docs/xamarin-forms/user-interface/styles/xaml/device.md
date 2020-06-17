---
제목: "설명:"의 장치 스타일 Xamarin.Forms Xamarin.Forms 은 Device. styles 클래스에서 장치 스타일 이라고 하는 6 가지 동적 스타일을 포함 합니다. 이 문서에서는 응용 프로그램에서 장치 스타일을 사용 하는 방법을 설명 Xamarin.Forms 합니다. "
assetid: 7FF19ED1-0822-4238-9435-AD970317A2F8: xamarin-forms author: davidbritch: dabritch:: 02/17/2016-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="device-styles-in-xamarinforms"></a>장치 스타일Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)

_Xamarin.ios 클래스에서 장치 스타일 이라고 하는 6 가지 동적 스타일을 포함 합니다._

*장치* 스타일은 다음과 같습니다.

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)

6 가지 스타일은 모두 인스턴스에만 적용할 수 있습니다 [`Label`](xref:Xamarin.Forms.Label) . 예를 들어, `Label` 단락의 본문을 표시 하는는 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 속성을로 설정할 수 있습니다 [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle) .

다음 코드 예제에서는 XAML 페이지에서 *장치* 스타일을 사용 하는 방법을 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DeviceStylesPage" Title="Device" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="myBodyStyle" TargetType="Label"
              BaseResourceKey="BodyStyle">
                <Setter Property="TextColor" Value="Accent" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="Title style"
              Style="{DynamicResource TitleStyle}" />
            <Label Text="Subtitle text style"
              Style="{DynamicResource SubtitleStyle}" />
            <Label Text="Body style"
              Style="{DynamicResource BodyStyle}" />
            <Label Text="Caption style"
              Style="{DynamicResource CaptionStyle}" />
            <Label Text="List item detail text style"
              Style="{DynamicResource ListItemDetailTextStyle}" />
            <Label Text="List item text style"
              Style="{DynamicResource ListItemTextStyle}" />
            <Label Text="No style" />
            <Label Text="My body style"
              Style="{StaticResource myBodyStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

장치 스타일은 태그 확장을 사용 하 여 바인딩됩니다 `DynamicResource` . 스타일의 동적 특성은 텍스트 크기에 대 한 **내게 필요한 옵션** 설정을 변경 하 여 iOS에서 볼 수 있습니다. 다음 스크린샷에 표시 된 것 처럼 *장치* 스타일의 모양은 각 플랫폼 마다 다릅니다.

![](device-images/device-styles.png "Device Styles on Each Platform")

*장치 스타일* 은 [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) 속성을 장치 스타일의 키 이름으로 설정 하 여에서 파생 될 수도 있습니다. 위의 코드 예제에서는 `myBodyStyle` 에서 상속 되 [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle) 고 악센트가 있는 텍스트 색을 설정 합니다. 동적 스타일 상속에 대 한 자세한 내용은 [동적 스타일 상속](~/xamarin-forms/user-interface/styles/xaml/dynamic.md#dynamic-style-inheritance)을 참조 하세요.

다음 코드 예제에서는 c #의 해당 페이지를 보여 줍니다.

```csharp
public class DeviceStylesPageCS : ContentPage
{
    public DeviceStylesPageCS ()
    {
        var myBodyStyle = new Style (typeof(Label)) {
            BaseResourceKey = Device.Styles.BodyStyleKey,
            Setters = {
                new Setter {
                    Property = Label.TextColorProperty,
                    Value = Color.Accent
                }
            }
        };

        Title = "Device";
        IconImageSource = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label { Text = "Title style", Style = Device.Styles.TitleStyle },
                new Label { Text = "Subtitle style", Style = Device.Styles.SubtitleStyle },
                new Label { Text = "Body style", Style = Device.Styles.BodyStyle },
                new Label { Text = "Caption style", Style = Device.Styles.CaptionStyle },
                new Label { Text = "List item detail text style",
                  Style = Device.Styles.ListItemDetailTextStyle },
                new Label { Text = "List item text style", Style = Device.Styles.ListItemTextStyle },
                new Label { Text = "No style" },
                new Label { Text = "My body style", Style = myBodyStyle }
            }
        };
    }
}
```

[`Style`](xref:Xamarin.Forms.NavigableElement.Style)각 인스턴스의 속성은 [`Label`](xref:Xamarin.Forms.Label) 클래스의 적절 한 속성으로 설정 됩니다 [`Devices.Styles`](xref:Xamarin.Forms.Device.Styles) .

## <a name="accessibility"></a>접근성

*장치* 스타일은 내게 필요한 옵션 기본 설정을 준수 하므로 각 플랫폼에서 접근성 기본 설정이 변경 될 때 글꼴 크기가 변경 됩니다. 따라서 액세스 가능한 텍스트를 지원 하려면 *장치* 스타일이 응용 프로그램 내의 모든 텍스트 스타일에 대 한 기준으로 사용 되는지 확인 합니다.

다음 스크린샷은 액세스 가능한 최소 글꼴 크기를 사용 하 여 각 플랫폼의 장치 스타일을 보여 줍니다.

[![](device-images/minimum-size.png "Accessible Small Device Styles on Each Platform")](device-images/minimum-size-large.png#lightbox "Accessible Small Device Styles on Each Platform")

다음 스크린샷은 액세스할 수 있는 가장 큰 글꼴 크기를 사용 하 여 각 플랫폼의 장치 스타일을 보여 줍니다.

![](device-images/maximum-size.png "Accessible Large Device Styles on Each Platform")

## <a name="related-links"></a>관련 링크

- [텍스트 스타일](~/xamarin-forms/user-interface/text/styles.md)
- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [동적 스타일 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)
- [스타일 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [장치 스타일](xref:Xamarin.Forms.Device.Styles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
