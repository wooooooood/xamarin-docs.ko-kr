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
ms.openlocfilehash: 61c0137788303363769fdec80a16542e2d8bea5e
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128611"
---
# <a name="tabbedpage-page-swiping-on-android"></a>Android에서 TabbedPage 페이지 살짝 밀기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 Android 플랫폼별는의 페이지 간에 가로 손가락 제스처를 사용 하 여 살짝 밀기를 사용 하도록 설정 하는 데 사용 됩니다 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . 연결 된 속성을 값으로 설정 하 여 XAML에서 사용 됩니다 [`TabbedPage.IsSwipePagingEnabled`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty) `boolean` .

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

`TabbedPage.On<Android>`메서드는이 플랫폼별가 Android 에서만 실행 되도록 지정 합니다. [ `TabbedPage.SetIsSwipePagingEnabled` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. SetIsSwipePagingEnabled ( Xamarin.Forms . BindableObject, system.string) 메서드는 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스에서의 페이지 사이를 살짝 밀기를 사용 하도록 설정 하는 데 사용 됩니다 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . 또한 `TabbedPage` 네임 스페이스의 클래스에 `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` 는 [ `EnableSwipePaging` ] (f:)도 Xamarin.Forms 있습니다. PlatformConfiguration. EnableSwipePaging ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . TabbedPage}))를 사용 하도록 설정 하는 메서드를 사용 하는 방법을 지정 `DisableSwipePaging` Xamarin.Forms 합니다. PlatformConfiguration. DisableSwipePaging ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . TabbedPage}))를 사용 하지 않도록 설정 하는 메서드를 지정 합니다. [`TabbedPage.OffscreenPageLimit`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty)연결 된 속성 및 [ `SetOffscreenPageLimit` ] (f:) Xamarin.Forms PlatformConfiguration. Setoffscreenandpagelimit ( Xamarin.Forms . BindableObject, System.object) 메서드는 현재 페이지의 한쪽에서 유휴 상태로 유지 해야 하는 페이지 수를 설정 하는 데 사용 됩니다.

결과적으로에 의해 표시 되는 페이지를 통해 살짝 밀기 페이징이 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 활성화 됩니다.

![](tabbedpage-page-swiping-images/tabbedpage-swipe.png)

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
