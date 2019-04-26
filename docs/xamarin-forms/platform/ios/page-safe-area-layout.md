---
title: IOS에서 안전 영역 레이아웃 안내선
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서는 iOS 11 이상를 사용 하는 모든 장치에 대 한 안전한 화면 영역 페이지 콘텐츠가 배치 되는 플랫폼 특정 iOS를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 2B6789C1-39B4-4C16-ADE1-3ED3378EAC63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: bc18f1e1f051e18a970464b134733f2af39681ae
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61250910"
---
# <a name="safe-area-layout-guide-on-ios"></a>IOS에서 안전 영역 레이아웃 안내선

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

이 iOS 플랫폼 특정 화면 iOS 11 이상를 사용 하는 모든 장치에 대 한 안전한 영역에서 페이지 콘텐츠가 배치 되는 확인 하는 데 사용 됩니다. 있는지 확인 하는 데는 특히 장치 둥근된 모서리, 홈 표시기 또는 iPhone X에서 센서 하우징 하 여 해당 콘텐츠가 클리핑 되지 않습니다. 설정 하 여 XAML에서 사용 되는 `Page.UseSafeArea` 연결 된 속성을 `boolean` 값:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Safe Area"
             ios:Page.UseSafeArea="true">
    <StackLayout>
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

`Page.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. 합니다 `Page.SetUseSafeArea` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스, 안전 영역 레이아웃 안내선 사용 되는지 여부를 제어 합니다.

결과 모든 Iphone에 안전한 화면 영역을 페이지 콘텐츠를 배치 될 수 있습니다.

[![](page-safe-area-images/safe-area-layout.png "안전 영역 레이아웃 안내선")](page-safe-area-images/safe-area-layout-large.png#lightbox "안전 영역 레이아웃 안내선")

> [!NOTE]
> Apple에서 정의 되는 안전한 영역 xamarin.forms에서를 사용 설정 합니다 [ `Page.Padding` ](xref:Xamarin.Forms.Page.Padding) 속성인 고 설정 된이 속성의 이전 값을 재정의 합니다.

안전 영역을 검색 하 여 사용자 지정할 수 있습니다 해당 [ `Thickness` ](xref:Xamarin.Forms.Thickness) 값을 `Page.SafeAreaInsets` 메서드에서 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스입니다. 다음으로 수정 필요 하 고 다시 할당할 합니다 `Padding` page 생성자에서 속성 또는 [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) 재정의:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
