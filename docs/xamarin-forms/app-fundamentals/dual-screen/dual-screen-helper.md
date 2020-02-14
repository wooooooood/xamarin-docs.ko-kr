---
title: Xamarin.Forms DualScreenHelper
description: 이 가이드에서는 Xamarin.Forms DualScreenHelper를 사용하여 Surface Duo 및 Surface Neo와 같은 이중 화면 디바이스의 앱 환경을 최적화하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 5aa184c2-5611-427d-85c7-1c56486c3e1b
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: b06e105560de635a21b42e8366bb47842372cf6a
ms.sourcegitcommit: 07941cf9704ff88cf4087de5ebdea623ff54edb1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145547"
---
# <a name="xamarinforms-dualscreenhelper"></a>Xamarin.Forms DualScreenHelper
[![샘플 다운로드](~/media/shared/download.png)샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/UserInterface/DualScreenDemos)
DualScreenHelper를 사용하면 디바이스가 화면 속 화면 모드를 지원하는지 검색하고 ContentPage를 화면 속 화면 창으로 열 수 있습니다. Neo 디바이스에서 실행 중인 경우에는 작성 모드에서 페이지가 원더바(WonderBar)로 열립니다. 
## <a name="properties"></a>속성
- `HasCompactModeSupport` - 플랫폼이 ContentPage를 CompactMode에서 여는 것을 지원하는지 확인하는 데 사용됩니다.
- `OpenCompactMode` - 플랫폼에서 지원하는 경우 ContentPage를 CompactMode에서 엽니다.
## <a name="example-usage"></a>사용 예
```c#
async void OpenCompactWindowClicked(object sender, EventArgs e)
{
    if(!DualScreenHelper.HasCompactModeSupport())
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