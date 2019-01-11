---
title: Android에서 VisualElement 권한 상승
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Android 플랫폼 특정 API 21 이상이 대상으로 하는 응용 프로그램에서 VisualElements 상승 제어를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 5BFD6175-2BBD-41CD-B8F9-521B4750B708
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: b3dd300d28e0cf27cc1b5ebea59a68d57145fd61
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54208928"
---
# <a name="visualelement-elevation-on-android"></a>Android에서 VisualElement 권한 상승

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

이 Android 플랫폼별 API 21를 대상으로 하는 권한 상승 또는 응용 프로그램에 대 한 시각적 요소의 Z 순서를 제어 하는 데가 큰 경우 시각적 요소 상승 Z 값이 높을수록 occluding Z 값이 낮은 시각적 요소를 사용 하 여 시각적 요소를 사용 하 여 해당 그리기 순서를 결정 합니다. 설정 하 여 XAML에서 사용 되는 `VisualElement.Elevation` 연결 된 속성을 `boolean` 값:

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

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

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

`Button.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. `VisualElement.SetElevation` 메서드를 합니다 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스에 null을 허용 하는 상승 시각적 요소를 설정 하는 `float`합니다. 또한는 `VisualElement.GetElevation` 메서드를 사용 하 여 시각적 요소 높이 값을 검색할 수 있습니다.

결과 더 높은 Z 값이 포함 된 시각적 요소 채워집니다 Z 값이 낮은 시각적 요소 수 있도록 시각적 요소의 높이 제어할 수 있습니다. 따라서이 예제의 두 번째 [ `Button` ](xref:Xamarin.Forms.Button) 위에 렌더링 되는 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 권한 상승 값이 높을수록 있기 때문에:

![](visualelement-elevation-images/elevation.png)

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
