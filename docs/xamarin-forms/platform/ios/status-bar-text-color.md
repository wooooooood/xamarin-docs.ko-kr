---
title: IOS의 NavigationPage Bar 텍스트 색 모드
description: 플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 NavigationPage의 상태 표시줄 텍스트 색이 탐색 모음의 광도와 일치 하는지 여부를 제어 하는 iOS 플랫폼 관련 기능을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 03698A44-39F1-4030-9AF5-F10A6713828A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: dcbc20139b989ced11f2d1d890ca7dd99a780e96
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137061"
---
# <a name="navigationpage-bar-text-color-mode-on-ios"></a>IOS의 NavigationPage Bar 텍스트 색 모드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 플랫폼 특정은의 상태 표시줄 텍스트 색 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 이 탐색 모음의 광도와 일치 하도록 조정 되는지 여부를 제어 합니다. [`NavigationPage.StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty)연결 된 속성을 열거형의 값으로 설정 하 여 XAML에서 사용 됩니다 [`StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) .

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

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

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

`NavigationPage.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. [ `NavigationPage.SetStatusBarTextColorMode` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . NavigationPage}, Xamarin.Forms . StatusBarTextColorMode) 메서드는 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스에서 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) [`StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) 다음 두 가지 가능한 값을 제공 하는 열거형과 함께의 상태 표시줄 텍스트 색이 탐색 모음의 광도와 일치 하도록 조정 되는지 여부를 제어 합니다.

- [`DoNotAdjust`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust)– 상태 표시줄 텍스트 색을 조정 하지 않아야 함을 나타냅니다.
- [`MatchNavigationBarTextLuminosity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity)– 상태 표시줄 텍스트 색이 탐색 모음의 광도와 일치 해야 함을 나타냅니다.

또한 [ `GetStatusBarTextColorMode` ] (f: Xamarin.Forms 입니다. PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . NavigationPage}) 메서드를 사용 하 여에 적용 된 열거형의 현재 값을 검색할 수 있습니다 [`StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) .

그러면의 상태 표시줄 텍스트 색을 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 탐색 모음의 광도와 일치 하도록 조정할 수 있습니다. 이 예에서는 사용자가 [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) 의 및 페이지 간에 전환할 때 상태 표시줄 텍스트 색이 변경 됩니다 [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) .

![](status-bar-text-color-images/status-bar-text-color-mode.png "Status Bar Text Color Mode Platform-Specific")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
