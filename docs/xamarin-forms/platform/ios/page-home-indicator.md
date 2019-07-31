---
title: IOS의 홈 표시기 표시 여부
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 페이지에서 홈 표시기의 표시 여부를 설정 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: F81022E0-3C6C-49C0-A000-FAF6574D3FB7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: c8acf826eb5b3f42f62803ac1caaaa5929c5ea75
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648857"
---
# <a name="home-indicator-visibility-on-ios"></a>IOS의 홈 표시기 표시 여부

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼 관련는의 홈 표시기 [`Page`](xref:Xamarin.Forms.Page)표시 여부를 설정 합니다. [`Page.PrefersHomeIndicatorAutoHidden`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.PrefersHomeIndicatorAutoHiddenProperty) 바인딩 가능한 속성을 `boolean`로 설정 하 여 XAML에서 사용 됩니다.

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersHomeIndicatorAutoHidden="true">
    ...
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersHomeIndicatorAutoHidden(true);
```

`Page.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스의 메서드는 [`Page.SetPrefersHomeIndicatorAutoHidden`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.SetPrefersHomeIndicatorAutoHidden(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Page},System.Boolean)) 홈 표시기의 표시 여부를 제어 합니다. 또한 메서드는 [`Page.PrefersHomeIndicatorAutoHidden`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.PrefersHomeIndicatorAutoHidden(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Page})) 홈 표시기의 표시 여부를 검색 하는 데 사용할 수 있습니다.

그러면에서 [`Page`](xref:Xamarin.Forms.Page) 홈 표시기의 표시 여부를 제어할 수 있습니다.

![IOS 페이지의 홈 표시기 표시 유형 스크린샷](page-home-indicator-images/home-indicator-visibility.png "페이지 홈 표시기 표시 여부")

> [!NOTE]
> 이 [`ContentPage`](xref:Xamarin.Forms.ContentPage)플랫폼별는 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) [,`NavigationPage`](xref:Xamarin.Forms.NavigationPage), [및`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 개체에 적용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
