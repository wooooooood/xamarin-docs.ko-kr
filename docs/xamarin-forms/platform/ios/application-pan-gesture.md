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
ms.openlocfilehash: 125685150243ba8e8099cbfbdfec90e5a0b4d6b7
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138582"
---
# <a name="simultaneous-pan-gesture-recognition-on-ios"></a>IOS에서 동시 이동 제스처 인식

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

가 [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) 스크롤 뷰 내의 뷰에 연결 되 면 모든 팬 제스처가에서 캡처되고 `PanGestureRecognizer` 스크롤 뷰로 전달 되지 않습니다. 따라서 스크롤 뷰가 더 이상 스크롤되지 않습니다.

이 iOS 플랫폼에 해당 하는는 스크롤 뷰에서 `PanGestureRecognizer` 이동 제스처를 캡처 및 공유 하는 데 사용할 수 있습니다. 연결 된 속성을로 설정 하 여 XAML에서 사용 됩니다 [`Application.PanGestureRecognizerShouldRecognizeSimultaneously`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.PanGestureRecognizerShouldRecognizeSimultaneouslyProperty) `true` .

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.PanGestureRecognizerShouldRecognizeSimultaneously="true">
    ...
</Application>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetPanGestureRecognizerShouldRecognizeSimultaneously(true);
```

`Application.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. [ `Application.SetPanGestureRecognizerShouldRecognizeSimultaneously` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . 응용 프로그램}, system.string) 메서드를 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 사용 하 여 스크롤 보기의 이동 제스처 인식기가 이동 제스처를 캡처하거나 스크롤 뷰를 사용 하 여 이동 제스처를 캡처 및 공유 하는지 여부를 제어 합니다. 또한 [ `Application.GetPanGestureRecognizerShouldRecognizeSimultaneously` ] (f: Xamarin.Forms 입니다. PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . Application}) 메서드를 사용 하 여 이동 제스처가를 포함 하는 스크롤 뷰와 공유 되는지 여부를 반환할 수 있습니다 [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) .

따라서이 플랫폼별를 사용 하는 경우에이 [`ListView`](xref:Xamarin.Forms.ListView) 포함 되어 있으면 [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) 및가 모두 `ListView` `PanGestureRecognizer` 이동 제스처를 수신 하 고 처리 합니다. 그러나이 플랫폼별를 사용 하지 않도록 설정 된 경우에이 `ListView` 포함 되어 있으면 `PanGestureRecognizer` 에서 `PanGestureRecognizer` 이동 제스처를 캡처하고 처리 하 고,가 `ListView` 이동 제스처를 수신 하지 않습니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
