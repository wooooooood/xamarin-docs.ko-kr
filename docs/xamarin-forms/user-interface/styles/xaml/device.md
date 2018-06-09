---
title: Xamarin.Forms에 장치 스타일
description: Xamarin.Forms는 Device.Styles 클래스에서 장치 스타일 라는 6 개의 동적 스타일을 포함 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 장치 스타일을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 7FF19ED1-0822-4238-9435-AD970317A2F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 6b5d4623bb331f4bf52faa096afeacb21d6d7489
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245597"
---
# <a name="device-styles-in-xamarinforms"></a>Xamarin.Forms에 장치 스타일

_Xamarin.Forms는 Device.Styles 클래스에서 장치 스타일 라는 6 개의 동적 스타일을 포함 합니다._

*장치* 스타일은:

- [`BodyStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)
- [`CaptionStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyle/)
- [`ListItemDetailTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyle/)
- [`ListItemTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyle/)
- [`SubtitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyle/)
- [`TitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyle/)

모든 6 개의 스타일에만 적용할 수 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 인스턴스. 예를 들어는 `Label` 단락의 본문에 표시 되는 설정할 수는 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 속성을 [ `BodyStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)합니다.

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

사용 하 여 바인딩된 장치 스타일은 `DynamicResource` 태그 확장 합니다. 스타일의 동적 특성을 변경 하 여 iOS에서 볼 수 있습니다는 **내게 필요한 옵션** 텍스트 크기에 대 한 설정입니다. 모양을 *장치* 스타일 달라 지는 각 플랫폼에 다음 스크린샷에서 같이:

![](device-images/device-styles.png "각 플랫폼에서 장치 스타일")

*장치* 스타일을 설정 하 여도 파생 수 있습니다는 [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) 장치 스타일에 대 한 키 이름 속성입니다. 위의 코드 예에서 `myBodyStyle` 에서 상속 [ `BodyStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/) 강조 된 텍스트 색을 가져오거나 설정 합니다. 동적 스타일 상속에 대 한 자세한 내용은 참조 [동적 스타일 상속](~/xamarin-forms/user-interface/styles/xaml/dynamic.md#dynamic-style-inheritance)합니다.

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

[ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 각 속성 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 인스턴스가에서 적절 한 속성으로 설정 되는 [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) 클래스입니다.

## <a name="accessibility"></a>액세스 가능성

*장치* 스타일 존중 내게 필요한 옵션 기본 설정 글꼴 크기는 각 플랫폼에서 변경 되는 내게 필요한 옵션 기본 설정으로 변경 됩니다. 따라서, 액세스할 수 있는 텍스트를 지원 하기 위해 확인 해야 하는 *장치* 스타일 응용 프로그램 내에서 한 텍스트 스타일에 대 한 기초로 사용 됩니다.

다음 스크린샷에서 가장 작은 액세스할 수 있는 글꼴 크기와 각 플랫폼에서 장치 스타일을 보여 줍니다.

[![](device-images/minimum-size.png "각 플랫폼에 액세스할 수 있는 소형 장치 스타일")](device-images/minimum-size-large.png#lightbox "각 플랫폼에 액세스할 수 있는 소형 장치 스타일")

다음 스크린샷에서 가장 큰 액세스할 수 있는 글꼴 크기와 각 플랫폼에서 장치 스타일을 보여 줍니다.

![](device-images/maximum-size.png "각 플랫폼에 액세스할 수 있는 대형 장치 스타일")

## <a name="summary"></a>요약

Xamarin.Forms 포함 6 *동적* 라고 스타일 *장치* 스타일에 [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) 클래스입니다. 모든 6 개의 스타일에만 적용할 수 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 인스턴스.


## <a name="related-links"></a>관련 링크

- [텍스트 스타일](~/xamarin-forms/user-interface/text/styles.md)
- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [동적 스타일 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [스타일 (샘플) 작업을](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Device.Styles](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [스타일](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
