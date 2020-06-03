---
title: Xamarin.Forms 이중 화면 플랫폼 도우미
description: 이 가이드에서는 Xamarin.Forms DualScreenHelper 클래스를 사용하여 Surface Duo 및 Surface Neo와 같은 이중 화면 디바이스의 앱 환경을 최적화하는 방법을 설명합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d9daf5a24c0dcfd07d529955c411259f4c1359df
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138946"
---
# <a name="xamarinforms-dual-screen-platform-helpers"></a>Xamarin.Forms 이중 화면 플랫폼 도우미

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)

`DualScreenHelper` 클래스는 디바이스가 화면 속 화면 모드를 지원하는지 여부를 확인하고 `ContentPage`를 화면 속 화면 창으로 여는 용도로 사용할 수 있습니다. Neo 디바이스에서 실행 중인 경우에는 작성 모드에서 페이지가 원더바(WonderBar)로 열립니다.

## <a name="properties"></a>속성

- `HasCompactModeSupport`는 플랫폼이 `ContentPage`를 CompactMode에서 여는 것을 지원하는지 여부를 확인하는 데 사용됩니다.
- 플랫폼에서 지원하는 경우, `OpenCompactMode`는 `ContentPage`를 CompactMode에서 엽니다.

## <a name="example"></a>예제

```csharp
async void OpenCompactWindowClicked(object sender, EventArgs e)
{
    if (!DualScreenHelper.HasCompactModeSupport())
    {
        await DisplayAlert("Unsupported", "This platform doesn't support this feature", "Ok");
        return;
    }

    ContentPage page = new ContentPage() { BackgroundColor = Color.Purple };

    var button = new Button()
    {
        Text = "Close this Window"
    };

    var layout = new StackLayout()
    {
        Children =
        {
            new Label(){ Text = "Welcome to your Compact Mode Window" },
            button
        },
        BackgroundColor = Color.Yellow,
        HorizontalOptions = LayoutOptions.Fill,
        VerticalOptions = LayoutOptions.Fill
    };

    page.Content = new ScrollView() { Content = layout };
    var args = await DualScreenHelper.OpenCompactMode(page);

    button.Command = new Command(async () =>
    {
        await args.CloseAsync();
    });
}
```
