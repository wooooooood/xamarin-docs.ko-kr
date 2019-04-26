---
title: IOS에서 슬라이더 위치 조정 컨트롤 탭
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Slider.Value 속성 슬라이더 막대를 탭 하 여 설정할 수 있도록 하는 iOS 플랫폼 전용을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: D0915D37-9A59-4728-BB6A-FE094A661275
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 43cc87f9d319295ce65d55488e1be032ae00a697
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61082205"
---
# <a name="slider-thumb-tap-on-ios"></a>IOS에서 슬라이더 위치 조정 컨트롤 탭

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

이 iOS 플랫폼별 사용 하도록 설정 합니다 [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) 속성에는 위치에 탭 하 여 설정 될를 [ `Slider` ](xref:Xamarin.Forms.Slider) 끌어 필요가 하는 대신 가로 막대형,는 `Slider` thumb. 설정 하 여 XAML에서 사용 되는 [ `Slider.UpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.UpdateOnTapProperty) 바인딩 가능한 속성을 `true`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Slider ... ios:Slider.UpdateOnTap="true" />
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var slider = new Xamarin.Forms.Slider();
slider.On<iOS>().SetUpdateOnTap(true);
```

`Slider.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. [ `Slider.SetUpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.SetUpdateOnTap(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Slider},System.Boolean)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 여부 탭 컨트롤에 네임 스페이스는를 `Slider` 표시줄 설정를 [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) 속성입니다. 또한 합니다 [ `Slider.GetUpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.GetUpdateOnTap(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Slider})) 메서드를 사용 하 여 반환할 수 있습니다 탭 여부를 `Slider` 표시줄 설정 됩니다는 `Slider.Value` 속성.

결과 탭 합니다 [ `Slider` ](xref:Xamarin.Forms.Slider) 모음 이동할 수는 `Slider` thumb 및 설정 합니다 [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) 속성:

![](slider-thumb-images/slider-updateontap.png "사용 하도록 설정 하는 탭에 대 한 슬라이더 업데이트")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
