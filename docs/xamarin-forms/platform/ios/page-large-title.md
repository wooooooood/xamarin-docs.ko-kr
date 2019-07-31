---
title: IOS의 큰 페이지 제목
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 NavigationPage의 탐색 모음에 페이지 제목을 표시 하는 iOS 플랫폼 관련 기능을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 45FD9145-8319-452C-9AE6-624431A4D43C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: ab9becf2f7363674346abf004c1748cb06eb0d31
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655419"
---
# <a name="large-page-titles-on-ios"></a>IOS의 큰 페이지 제목

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 ios 플랫폼 전용은 ios 11 이상을 사용 하는 장치에 대 한 탐색 모음 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)에서 페이지 제목을 큰 제목으로 표시 하는 데 사용 됩니다. 큰 제목 왼쪽 맞춤은 큰 글꼴을 사용 하 여 및 화면 부동산 효율적으로 사용 되도록 사용자가 콘텐츠를 스크롤 하는 대로 표준 제목으로 전환 합니다. 그러나 가로 방향으로 제목 콘텐츠 레이아웃을 최적화 하기 위해 탐색 모음의 센터로 돌아갑니다. 설정 하 여 XAML에서 사용 되는 `NavigationPage.PrefersLargeTitles` 연결 된 속성을 `boolean` 값:

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

`NavigationPage.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. 합니다 `NavigationPage.SetPrefersLargeTitle` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스에는 큰 제목 사용 되는지 여부를 제어 합니다.

큰 제목에서 사용 되는 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), 탐색 스택의 모든 페이지에는 큰 제목 표시 됩니다. 설정 하 여 페이지에서이 동작을 재정의할 수 있습니다 합니다 `Page.LargeTitleDisplay` 연결 된 속성의 값으로는 `LargeTitleDisplayMode` 열거형:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 페이지 동작을 재정의할 수 있습니다.

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

`Page.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. `Page.SetLargeTitleDisplay` 메서드를 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스에서 큰 제목 동작을 제어를 [ `Page` ](xref:Xamarin.Forms.Page)를 사용 하 여를 `LargeTitleDisplayMode` 세 가지 가능한을 제공 하는 열거형 값:

- `Always` – 탐색 모음 및 글꼴을 강제로 큰 형식을 사용 하는 크기입니다.
- `Automatic` – 탐색 스택의 이전 항목으로 같은 스타일 (중소기업)을 사용 합니다.
- `Never` – 작은 일반 형식으로 탐색 모음을 사용 합니다.

또한 합니다 `SetLargeTitleDisplay` 메서드를 호출 하 여 열거형 값을 설정/해제를 사용할 수는 `LargeTitleDisplay` 현재 반환 하는 메서드 `LargeTitleDisplayMode`:

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

결과 지정한 `LargeTitleDisplayMode` 에 적용 되는 [ `Page` ](xref:Xamarin.Forms.Page), 큰 제목 동작을 제어:

![](page-large-title-images/large-title.png "효과가 플랫폼별 흐리게 표시")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
