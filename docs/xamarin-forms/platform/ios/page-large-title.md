---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0db20620870340386ccd0cedf7f98cb2975527ba
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128048"
---
# <a name="large-page-titles-on-ios"></a>IOS의 큰 페이지 제목

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼 전용은 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) ios 11 이상을 사용 하는 장치에 대 한 탐색 모음에서 페이지 제목을 큰 제목으로 표시 하는 데 사용 됩니다. 큰 제목은 왼쪽에 맞추고, 더 큰 글꼴을 사용 하 고, 화면 부동산을 효율적으로 사용 하기 위해 사용자가 콘텐츠를 스크롤할 때 표준 제목으로 전환 됩니다. 그러나 가로 방향으로 제목이 탐색 모음의 가운데로 돌아가서 콘텐츠 레이아웃을 최적화 합니다. 연결 된 속성을 값으로 설정 하 여 XAML에서 사용 됩니다 `NavigationPage.PrefersLargeTitles` `boolean` .

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

`NavigationPage.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. `NavigationPage.SetPrefersLargeTitle`네임 스페이스의 메서드는 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 큰 제목이 사용 되는지 여부를 제어 합니다.

에서 큰 제목이 사용 하도록 설정 된 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 경우 탐색 스택의 모든 페이지에 큰 제목이 표시 됩니다. 이 동작은 `Page.LargeTitleDisplay` 연결 된 속성을 열거형의 값으로 설정 하 여 페이지에서 재정의할 수 있습니다 `LargeTitleDisplayMode` .

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 페이지 동작을 재정의할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSLargeTitlePageCS : ContentPage
{
    public iOSLargeTitlePageCS(ICommand restore)
    {
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        ...
    }
    ...
}
```

`Page.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. `Page.SetLargeTitleDisplay`네임 스페이스의 메서드는 다음과 같은 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) [`Page`](xref:Xamarin.Forms.Page) `LargeTitleDisplayMode` 세 가지 값을 제공 하는 열거형을 사용 하 여의 긴 제목 동작을 제어 합니다.

- `Always`– 탐색 모음 및 글꼴 크기를 강제로 큰 형식으로 사용 합니다.
- `Automatic`– 탐색 스택에서 이전 항목과 동일한 스타일 (크거나 작음)을 사용 합니다.
- `Never`– 작은 형식의 일반 탐색 모음을 강제로 사용 합니다.

또한 메서드를 `SetLargeTitleDisplay` 호출 하 여 `LargeTitleDisplay` 현재을 반환 하는 메서드를 호출 하 여 열거형 값을 전환 하는 데 메서드를 사용할 수 있습니다 `LargeTitleDisplayMode` .

```csharp
switch (On<iOS>().LargeTitleDisplay())
{
    case LargeTitleDisplayMode.Always:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Automatic);
        break;
    case LargeTitleDisplayMode.Automatic:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        break;
    case LargeTitleDisplayMode.Never:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Always);
        break;
}
```

그 결과, 지정 된 `LargeTitleDisplayMode` 가에 적용 되어 [`Page`](xref:Xamarin.Forms.Page) 커다란 제목 동작을 제어 합니다.

![](page-large-title-images/large-title.png "Blur Effect Platform-Specific")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
