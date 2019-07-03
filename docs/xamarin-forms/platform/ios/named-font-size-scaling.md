---
title: IOS에서 명명 된 글꼴 크기에 대 한 내게 필요한 옵션 확장
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 명명 된 글꼴 크기에 대 한 크기 조정 하는 내게 필요한 옵션을 사용 하지 않도록 설정 하는 iOS 플랫폼 특정을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: B443BAF6-E6F6-4A0E-80B5-CAACE6B550EF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/28/2019
ms.openlocfilehash: 1fc6fefe1f9fe48fe2abb367b376a5a6f3484462
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67517952"
---
# <a name="accessibility-scaling-for-named-font-sizes-on-ios"></a>IOS에서 명명 된 글꼴 크기에 대 한 내게 필요한 옵션 확장

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

이 iOS 플랫폼 특정 명명 된 글꼴 크기에 대 한 크기 조정 하는 내게 필요한 옵션을 해제 합니다. 설정 하 여 XAML에서 사용 되는 `Application.EnableAccessibilityScalingForNamedFontSizes` 바인딩 가능한 속성을 `false`:

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

`Application.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. 합니다 `Application.SetEnableAccessibilityScalingForNamedFontSizes` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스에는 iOS 내게 필요한 옵션 설정에 따라 크기가 조정 되는 명명 된 글꼴 크기를 사용 하지 않도록 설정 하는 데 사용 됩니다. 또한는 `Application.GetEnableAccessibilityScalingForNamedFontSizes` 메서드를 사용 하 여 iOS 내게 필요한 옵션 설정에 따라 명명 된 글꼴 크기는 조정 여부를 반환할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
