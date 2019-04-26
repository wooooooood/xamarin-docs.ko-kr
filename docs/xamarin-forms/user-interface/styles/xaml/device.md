---
title: Xamarin.Forms에서 장치 스타일
description: Xamarin.Forms는 Device.Styles 클래스에서 장치 스타일으로 알려진 6 동적 스타일을 포함 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램을 장치 스타일을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 7FF19ED1-0822-4238-9435-AD970317A2F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: fd5181040c1805d3fdabdae4803bbe32c6bb6652
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61345528"
---
# <a name="device-styles-in-xamarinforms"></a>Xamarin.Forms에서 장치 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)

_Xamarin.Forms는 Device.Styles 클래스에서 장치 스타일으로 알려진 6 동적 스타일을 포함 합니다._

합니다 *장치* 스타일은:

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)

모든 6 스타일에만 적용할 수 있습니다 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스. 예를 들어, 한 `Label` 단락의 본문을 표시 하는 설정할 수 해당 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) 속성을 [ `BodyStyle` ](xref:Xamarin.Forms.Device.Styles.BodyStyle)합니다.

다음 코드 예제는 *장치* XAML 페이지의 스타일:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DeviceStylesPage" Title="Device" Icon="xaml.png">
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

장치 스타일을 사용 하 여 바인딩된는 `DynamicResource` 태그 확장 합니다. 스타일의 동적 특성을 변경 하 여 iOS에서 볼 수 있습니다 합니다 **내게 필요한 옵션** 텍스트 크기에 대 한 설정입니다. 모양의 합니다 *장치* 스타일은 다음 스크린샷과에서 같이 각 플랫폼 마다 다르게 합니다.

![](device-images/device-styles.png "각 플랫폼에 대 한 장치 스타일")

*장치* 스타일을 설정 하 여도 파생 수 합니다 [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) 장치 스타일의 키 이름 속성입니다. 위의 코드 예제의 `myBodyStyle` 에서 상속 [ `BodyStyle` ](xref:Xamarin.Forms.Device.Styles.BodyStyle) 악센트 부호가 있는 텍스트 색을 가져오거나 설정 합니다. 동적 스타일 상속에 대 한 자세한 내용은 참조 하세요. [동적 스타일 상속](~/xamarin-forms/user-interface/styles/xaml/dynamic.md#dynamic-style-inheritance)합니다.

다음 코드 예제에서는 C#의 해당 페이지를 보여 줍니다.

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
        Icon = "csharp.png";
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

[ `Style` ](xref:Xamarin.Forms.VisualElement.Style) 의 각 속성 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스가에서 적절 한 속성에 설정 되어 합니다 [ `Devices.Styles` ](xref:Xamarin.Forms.Device.Styles) 클래스입니다.

## <a name="accessibility"></a>액세스 가능성

합니다 *장치* 스타일 준수 내게 필요한 옵션 기본 설정, 글꼴 크기는 각 플랫폼에서 변경 되는 내게 필요한 옵션 기본 설정으로 변경 됩니다. 따라서 액세스할 수 있는 텍스트를 지원 하려면 있는지를 확인 합니다 *장치* 스타일 응용 프로그램 내에서 모든 텍스트 스타일에 대 한 기초로 사용 됩니다.

다음 스크린샷에서 가장 작은 액세스할 수 있는 글꼴 크기를 사용 하 여 각 플랫폼에서 장치 스타일을 보여 줍니다.

[![](device-images/minimum-size.png "각 플랫폼에 액세스할 수 있는 소형 장치 스타일")](device-images/minimum-size-large.png#lightbox "각 플랫폼에 액세스할 수 있는 소형 장치 스타일")

다음 스크린샷에서 가장 큰 액세스할 수 있는 글꼴 크기를 사용 하 여 각 플랫폼에서 장치 스타일을 보여 줍니다.

![](device-images/maximum-size.png "각 플랫폼에 액세스할 수 있는 큰 장치 스타일")

## <a name="related-links"></a>관련 링크

- [텍스트 스타일](~/xamarin-forms/user-interface/text/styles.md)
- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [동적 스타일 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [스타일 (샘플)를 사용 하 여 작업](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Device.Styles](xref:Xamarin.Forms.Device.Styles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [스타일](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
