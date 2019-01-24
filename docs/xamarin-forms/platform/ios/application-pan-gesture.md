---
title: IOS에서 동시 팬 제스처 인식
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 동시 팬 제스처 인식 응용 프로그램에서 사용할 수 있도록 하는 iOS 플랫폼 전용을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 883D89DA-F8FF-4B97-9C3F-2DD05C96A495
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 32c3651734fcc94dd75372f0c47f0ffb22b4a4ee
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54209708"
---
# <a name="simultaneous-pan-gesture-recognition-on-ios"></a>IOS에서 동시 팬 제스처 인식

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

경우는 [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) 스크롤 뷰를 모든 제스처에 의해 캡처되는 pan의 내 보기에 연결할 때를 `PanGestureRecognizer` 스크롤 보기에 전달 되지 않습니다. 따라서 스크롤 뷰가 더 이상 스크롤됩니다.

이 iOS 플랫폼별 수 있도록를 `PanGestureRecognizer` 스크롤 뷰에서 수집 하 고 스크롤 뷰를 사용 하 여 pan 제스처를 공유 합니다. 설정 하 여 XAML에서 사용 되는 [ `Application.PanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.PanGestureRecognizerShouldRecognizeSimultaneouslyProperty) 연결 된 속성을 `true`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.PanGestureRecognizerShouldRecognizeSimultaneously="true">
    ...
</Application>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetPanGestureRecognizerShouldRecognizeSimultaneously(true);
```

`Application.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. [ `Application.SetPanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.SetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application},System.Boolean)) 메서드를 합니다 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 스크롤 보기에서 이동 제스처 인식기는 팬 제스처를 캡처 또는 캡처 및 이동을 공유 하는지 여부를 네임 스페이스를 제어 하는 스크롤 뷰를 사용 하 여 제스처입니다. 또한 합니다 [ `Application.GetPanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.GetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application})) 팬 제스처를 포함 하는 스크롤 뷰를 사용 하 여 공유 되는지 여부를 반환 하려면 메서드를 사용할 수는 [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer)합니다.

따라서이 플랫폼별 경우 활성화를 사용 하 여는 [ `ListView` ](xref:Xamarin.Forms.ListView) 포함을 [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer)두는 `ListView` 및 `PanGestureRecognizer` 팬 제스처를 받게 됩니다 및 이 처리 합니다. 그러나 플랫폼 특정을 사용할 수 없을 때이 사용 하 여는 `ListView` 포함을 `PanGestureRecognizer`의 `PanGestureRecognizer` 팬 제스처를 캡처 및 처리 됩니다 및 `ListView` 팬 제스처를 받지 않습니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
