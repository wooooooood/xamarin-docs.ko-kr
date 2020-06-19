---
title: IOS의 슬라이더 엄지 단추 탭
description: 플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 슬라이더 막대를 눌러 Slider 속성을 설정 하는 데 사용할 수 있는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: D0915D37-9A59-4728-BB6A-FE094A661275
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 93b4dba3e8543bd2cc2a4f2187f617aae5daff77
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137074"
---
# <a name="slider-thumb-tap-on-ios"></a>IOS의 슬라이더 엄지 단추 탭

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼 관련 기능을 사용 하면 [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) [`Slider`](xref:Xamarin.Forms.Slider) 엄지 단추를 끌 필요 없이 막대의 위치를 눌러 속성을 설정할 수 있습니다 `Slider` . 바인딩 가능한 속성을로 설정 하 여 XAML에서 사용 됩니다 [`Slider.UpdateOnTap`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.UpdateOnTapProperty) `true` .

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Slider ... ios:Slider.UpdateOnTap="true" />
        ...
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var slider = new Xamarin.Forms.Slider();
slider.On<iOS>().SetUpdateOnTap(true);
```

`Slider.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. [ `Slider.SetUpdateOnTap` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. iOSSpecific. Slider. SetUpdateOnTap ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . Slider}, system.string) 메서드를 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 사용 하 여 막대를 탭 하면 속성이 설정 되는지 여부를 제어할 수 `Slider` [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) 있습니다. 또한 [ `Slider.GetUpdateOnTap` ] (f: Xamarin.Forms 입니다. PlatformConfiguration. iOSSpecific. GetUpdateOnTap ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . Slider})) 메서드를 사용 하 여 막대를 탭 하면 속성이 설정 되는지 여부를 반환할 수 있습니다 `Slider` `Slider.Value` .

그러면 막대를 탭 하 여 [`Slider`](xref:Xamarin.Forms.Slider) 엄지 단추를 이동 하 `Slider` 고 속성을 설정할 수 있습니다 [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) .

![](slider-thumb-images/slider-updateontap.png "Slider Update on Tap enabled")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
