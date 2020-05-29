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
ms.openlocfilehash: f5a2be4bd61056a42593fc45e1abdd3679795bc0
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139986"
---
# <a name="tabbedpage-toolbar-placement-and-color-on-android"></a>Android의 TabbedPage 도구 모음 배치 및 색

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

> [!IMPORTANT]
> 의 도구 모음 색을 설정 하는 플랫폼별는 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 이제 사용 되지 않으며 및 속성으로 대체 되었습니다 [`SelectedTabColor`](xref:Xamarin.Forms.TabbedPage.SelectedTabColor) [`UnselectedTabColor`](xref:Xamarin.Forms.TabbedPage.UnselectedTabColor) . 자세한 내용은 [Create a TabbedPage](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md#create-a-tabbedpage)을 참조 하세요.

이러한 플랫폼에 대 한 자세한 내용은에서 도구 모음의 배치 및 색을 설정 하는 데 사용 됩니다 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . [`TabbedPage.ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.ToolbarPlacementProperty)연결 된 속성을 열거형의 값으로 설정 하 [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) 고 [`TabbedPage.BarItemColor`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarItemColorProperty) 및 연결 된 [`TabbedPage.BarSelectedItemColor`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarSelectedItemColorProperty) 속성 [`Color`](xref:Xamarin.Forms.Color) 을로 설정 하 여 XAML에서 사용 됩니다.

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.ToolbarPlacement="Bottom"
            android:TabbedPage.BarItemColor="Black"
            android:TabbedPage.BarSelectedItemColor="Red">
    ...
</TabbedPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetToolbarPlacement(ToolbarPlacement.Bottom)
             .SetBarItemColor(Color.Black)
             .SetBarSelectedItemColor(Color.Red);
```

`TabbedPage.On<Android>`메서드는 이러한 플랫폼에 대 한 세부 사항이 Android 에서만 실행 되도록 지정 합니다. [ `TabbedPage.SetToolbarPlacement` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. AndroidSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . TabbedPage}, Xamarin.Forms . 다음 값을 제공 하는 열거형을 사용 하 여 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스에서 도구 모음 배치를 설정 하는 데 사용 됩니다. [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement)

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Default)– 도구 모음이 페이지의 기본 위치에 배치 됨을 나타냅니다. 휴대폰에서 페이지의 위쪽이 고 다른 장치 관용구 페이지의 아래쪽에 있습니다.
- [`Top`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Top)– 도구 모음이 페이지 맨 위에 배치 됨을 나타냅니다.
- [`Bottom`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Bottom)– 도구 모음이 페이지 아래쪽에 배치 됨을 나타냅니다.

또한 [ `TabbedPage.SetBarItemColor` ] (f: Xamarin.Forms 입니다. PlatformConfiguration. SetBarItemColor ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . TabbedPage}, Xamarin.Forms . Color) 및 [ `TabbedPage.SetBarSelectedItemColor` ] (f: Xamarin.Forms . PlatformConfiguration. SetBarSelectedItemColor ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . TabbedPage}, Xamarin.Forms . Color) 메서드를 사용 하 여 도구 모음 항목과 선택한 도구 모음 항목의 색을 각각 설정 합니다.

> [!NOTE]
> [ `GetToolbarPlacement` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. GetToolbarPlacement ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . TabbedPage})), [ `GetBarItemColor` ] (f: Xamarin.Forms . PlatformConfiguration. Get Itemcolor ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . TabbedPage})) 및 [ `GetBarSelectedItemColor` ] (f: Xamarin.Forms . PlatformConfiguration. GetBarSelectedItemColor ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . TabbedPage})) 메서드를 사용 하 여 도구 모음의 배치 및 색을 검색할 수 있습니다 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) .

그러면 도구 모음 배치, 도구 모음 항목의 색 및 선택한 도구 모음 항목의 색을에서 설정할 수 있습니다 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) .

![](tabbedpage-toolbar-placement-color-images/tabbedpage-toolbar-placement.png)

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
