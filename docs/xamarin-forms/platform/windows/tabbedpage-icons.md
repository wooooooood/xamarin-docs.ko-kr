---
title: Windows에서 아이콘 TabbedPage
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 TabbedPage 도구 모음에 페이지 아이콘을 표시 하는 데 사용 되는 Windows 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 7C5031A5-74EE-4469-994E-BEA7BA9D33CB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: a40203e4d01f45ef36ee6988198400a259600aac
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70198089"
---
# <a name="tabbedpage-icons-on-windows"></a>Windows에서 아이콘 TabbedPage

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 유니버설 Windows 플랫폼 플랫폼별를 사용 하 여 페이지 아이콘을 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 도구 모음에 표시 하 고 필요에 따라 아이콘 크기를 지정할 수 있는 기능을 제공 합니다. 설정 하 여 XAML에서 사용 되는 [ `TabbedPage.HeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsEnabledProperty) 연결 된 속성을 `true`, 필요에 따라 설정 하 고는 [ `TabbedPage.HeaderIconsSize` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsSizeProperty) 연결 된 속성을 [ `Size` ](xref:Xamarin.Forms.Size) 값:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:TabbedPage.HeaderIconsEnabled="true">
    <windows:TabbedPage.HeaderIconsSize>
        <Size>
            <x:Arguments>
                <x:Double>24</x:Double>
                <x:Double>24</x:Double>
            </x:Arguments>
        </Size>
    </windows:TabbedPage.HeaderIconsSize>
    <ContentPage Title="Todo" IconImageSource="todo.png">
        ...
    </ContentPage>
    <ContentPage Title="Reminders" IconImageSource="reminders.png">
        ...
    </ContentPage>
    <ContentPage Title="Contacts" IconImageSource="contacts.png">
        ...
    </ContentPage>
</TabbedPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

public class WindowsTabbedPageIconsCS : Xamarin.Forms.TabbedPage
{
  public WindowsTabbedPageIconsCS()
  {
    On<Windows>().SetHeaderIconsEnabled(true);
    On<Windows>().SetHeaderIconsSize(new Size(24, 24));

    Children.Add(new ContentPage { Title = "Todo", IconImageSource = "todo.png" });
    Children.Add(new ContentPage { Title = "Reminders", IconImageSource = "reminders.png" });
    Children.Add(new ContentPage { Title = "Contacts", IconImageSource = "contacts.png" });
  }
}
```

`TabbedPage.On<Windows>` 메서드가 플랫폼별 유니버설 Windows 플랫폼에만 실행 되도록 지정 합니다. 합니다 [ `TabbedPage.SetHeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.SetHeaderIconsEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.TabbedPage},System.Boolean)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 네임 스페이스는 헤더 아이콘 켜기 / 끄기 하는 데 사용 됩니다. [ `TabbedPage.SetHeaderIconsSize` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.SetHeaderIconsSize(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.TabbedPage},Xamarin.Forms.Size)) 사용 하 여 헤더 아이콘 크기를 선택적으로 지정 하는 메서드를 [ `Size` ](xref:Xamarin.Forms.Size) 값입니다.

또한 합니다 `TabbedPage` 클래스를 `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` 네임 스페이스 역시를 [ `EnableHeaderIcons` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.EnableHeaderIcons*) 머리글 아이콘을 사용 하도록 설정 하는 메서드는 [ `DisableHeaderIcons` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.DisableHeaderIcons*) 머리글 아이콘을 사용 하지 않도록 설정 하는 메서드 및 [ `IsHeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.IsHeaderIconsEnabled*) 반환 하는 메서드를 `boolean` 머리글 아이콘 사용 되는지 여부를 나타내는 값입니다.

결과 해당 페이지에 아이콘을 표시할 수 있습니다는 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) 필요에 따라 원하는 크기로 설정 아이콘 크기를 사용 하 여 도구 모음:

![TabbedPage 사용 하도록 설정 하는 아이콘 플랫폼별](tabbedpage-icons-images/tabbedpage-icons.png "TabbedPage 사용 하도록 설정 하는 아이콘 플랫폼별")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
