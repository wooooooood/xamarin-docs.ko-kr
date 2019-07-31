---
title: IOS의 명명 된 글꼴 크기에 대 한 접근성 크기 조정
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 명명 된 글꼴 크기에 대 한 접근성 크기 조정을 사용 하지 않도록 설정 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: B443BAF6-E6F6-4A0E-80B5-CAACE6B550EF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/28/2019
ms.openlocfilehash: 678ea2210389eb33bc3a184d758f2bfa07c60bd9
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656727"
---
# <a name="accessibility-scaling-for-named-font-sizes-on-ios"></a>IOS의 명명 된 글꼴 크기에 대 한 접근성 크기 조정

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼별 플랫폼 전용은 명명 된 글꼴 크기에 대 한 접근성 크기 조정을 비활성화 합니다. 설정 하 여 XAML에서 사용 되는 `Application.EnableAccessibilityScalingForNamedFontSizes` 바인딩 가능한 속성을 `false`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.EnableAccessibilityScalingForNamedFontSizes="false">
    ...
</Application>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetEnableAccessibilityScalingForNamedFontSizes(false);
```

`Application.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. [네임스페이스`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 의 메서드는iOS접근성설정에따라크기가조정되는명명된글꼴크기를사용하지않도록설정하는`Application.SetEnableAccessibilityScalingForNamedFontSizes` 데 사용 됩니다. 또한 `Application.GetEnableAccessibilityScalingForNamedFontSizes` 메서드를 사용 하 여 명명 된 글꼴 크기를 iOS 내게 필요한 옵션 설정으로 조정할지 여부를 반환할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
