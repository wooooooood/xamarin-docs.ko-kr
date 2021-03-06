---
title: IOS의 홈 표시기 표시 여부
description: 플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 페이지에서 홈 표시기의 표시 여부를 설정 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: F81022E0-3C6C-49C0-A000-FAF6574D3FB7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e76684ffb293380c283153c35c907acc50e40aab
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128078"
---
# <a name="home-indicator-visibility-on-ios"></a>IOS의 홈 표시기 표시 여부

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼 관련는의 홈 표시기 표시 여부를 설정 합니다 [`Page`](xref:Xamarin.Forms.Page) . 바인딩 가능한 속성을로 설정 하 여 XAML에서 사용 됩니다 [`Page.PrefersHomeIndicatorAutoHidden`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.PrefersHomeIndicatorAutoHiddenProperty) `boolean` .

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersHomeIndicatorAutoHidden="true">
    ...
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersHomeIndicatorAutoHidden(true);
```

`Page.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. [ `Page.SetPrefersHomeIndicatorAutoHidden` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . Page}, system.string) 메서드는 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스에서 홈 표시기의 표시 여부를 제어 합니다. 또한 [ `Page.PrefersHomeIndicatorAutoHidden` ] (f: Xamarin.Forms 입니다. PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . Page}) 메서드를 사용 하 여 홈 표시기의 표시 여부를 검색할 수 있습니다.

그러면에서 홈 표시기의 표시 여부를 [`Page`](xref:Xamarin.Forms.Page) 제어할 수 있습니다.

![IOS 페이지의 홈 표시기 표시 유형 스크린샷](page-home-indicator-images/home-indicator-visibility.png "페이지 홈 표시기 가시성")

> [!NOTE]
> 이 플랫폼별는 [`ContentPage`](xref:Xamarin.Forms.ContentPage) ,, [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 및 개체에 적용할 수 있습니다 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) .

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
