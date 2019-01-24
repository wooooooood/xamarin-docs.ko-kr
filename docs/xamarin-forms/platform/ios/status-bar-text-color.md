---
title: IOS의 NavigationPage 막대 텍스트 색 모드
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 상태 표시줄 텍스트 색을 NavigationPage에서 탐색 모음의 명도 일치 하는지 여부를 제어 하는 iOS 플랫폼 전용을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 03698A44-39F1-4030-9AF5-F10A6713828A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 39db74f8df8d8d3f62ff53b91e17f93f38c44bf0
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54209098"
---
# <a name="navigationpage-bar-text-color-mode-on-ios"></a>IOS의 NavigationPage 막대 텍스트 색 모드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

이 플랫폼별 제어 상태 표시줄의 텍스트 색에 있는지 여부를 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 탐색 모음의 광도 맞게 조정 됩니다. 설정 하 여 XAML에서 사용 되는 [ `NavigationPage.StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty) 연결 된 속성의 값에는 [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) 열거형:

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    x:Class="PlatformSpecifics.iOSStatusBarTextColorModePage">
    <MasterDetailPage.Master>
        <ContentPage Title="Master Page Title" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage BarBackgroundColor="Blue" BarTextColor="White"
                        ios:NavigationPage.StatusBarTextColorMode="MatchNavigationBarTextLuminosity">
            <x:Arguments>
                <ContentPage>
                    <Label Text="Slide the master page to see the status bar text color mode change." />
                </ContentPage>
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>

```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

IsPresentedChanged += (sender, e) =>
{
    var mdp = sender as MasterDetailPage;
    if (mdp.IsPresented)
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.DoNotAdjust);
    else
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.MatchNavigationBarTextLuminosity);
};
```

`NavigationPage.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. [ `NavigationPage.SetStatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage},Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스를 제어 상태 표시줄의 텍스트 색에 있는지 여부를 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 에 맞게 조정 됩니다는 탐색 모음의 명도와 합니다 [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) 가능한 두 값을 제공 하는 열거형:

- [`DoNotAdjust`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust) -상태 표시줄 텍스트 색을 변경 하지 말아야 나타냅니다.
- [`MatchNavigationBarTextLuminosity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity) -상태 표시줄 텍스트 색의 탐색 모음 광도 일치 해야 함을 나타냅니다.

또한 합니다 [ `GetStatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) 의 현재 값을 검색할 메서드를 사용할 수는 [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) 열거형에 적용 되는 합니다 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage).

결과 상태 표시줄의 텍스트 색을 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 탐색 모음의 광도 맞게 조정할 수 있습니다. 이 예제에서는 사용자 상태 표시줄 텍스트 색 변경 간에 전환 합니다 [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) 및 [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) 페이지를 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

![](status-bar-text-color-images/status-bar-text-color-mode.png "상태 표시줄 텍스트 색 모드 플랫폼별")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
