---
title: IOS의 NavigationPage Bar 반투명도
description: 플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 NavigationPage의 탐색 모음 투명도를 변경 하는 iOS 플랫폼 관련 기능을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 1150941B-56DB-4781-BE36-A4C4F9F2C500
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: be7e95df0ac247c5cd97c1715e4af67b48275180
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128117"
---
# <a name="navigationpage-bar-translucency-on-ios"></a>IOS의 NavigationPage Bar 반투명도

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼별는의 탐색 모음에 대 한 투명도를 변경 하는 데 사용 되며 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 연결 된 속성을 값으로 설정 하 여 XAML에서 사용 됩니다 [`NavigationPage.IsNavigationBarTranslucent`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty) `boolean` .

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

`NavigationPage.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. [ `NavigationPage.EnableTranslucentNavigationBar` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . NavigationPage})) 메서드를 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 사용 하 여 탐색 모음을 반투명 하 게 만들 수 있습니다. 또한 [`NavigationPage`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage) 네임 스페이스의 클래스에 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` 는 [ `DisableTranslucentNavigationBar` ] (f:)도 Xamarin.Forms 있습니다. PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . NavigationPage})) 메서드를 기본 상태로 복원 하 고 [ `SetIsNavigationBarTranslucent` ] (f:로 이동 Xamarin.Forms 합니다. PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . NavigationPage}, system.string) 메서드를 호출 하 여 탐색 모음 투명도를 전환 하는 데 사용할 수 있는 메서드 `IsNavigationBarTranslucent` (f: Xamarin.Forms . PlatformConfiguration. iOSSpecific NavigationPage. Isnavigation바 반투명 ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . NavigationPage})) 메서드:

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

그러면 탐색 모음의 투명도를 변경할 수 있습니다.

![](navigation-bar-translucent-images/translucent-navigation-bar.png "Translucent Navigation Bar Platform-Specific")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
