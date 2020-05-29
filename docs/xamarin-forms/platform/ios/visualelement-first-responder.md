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
ms.openlocfilehash: d8bd539c2bb0e8963afae3392b6f8e99d79af9af
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136971"
---
# <a name="visualelement-first-responder-on-ios"></a>IOS에서 VisualElement first 응답자

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼별를 사용 하면 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 개체가 요소를 포함 하는 페이지가 아니라 터치 이벤트의 첫 번째 응답자가 될 수 있습니다. 바인딩 가능한 속성을로 설정 하 여 XAML에서 사용 됩니다 `VisualElement.CanBecomeFirstResponder` `true` .

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Entry Placeholder="Enter text" />
        <Button ios:VisualElement.CanBecomeFirstResponder="True"
                Text="OK" />
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Entry entry = new Entry { Placeholder = "Enter text" };
Button button = new Button { Text = "OK" };
button.On<iOS>().SetCanBecomeFirstResponder(true);
```

`VisualElement.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. `VisualElement.SetCanBecomeFirstResponder`네임 스페이스의 메서드는 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) `VisualElement` 터치 이벤트의 첫 번째 응답자가 되도록를 설정 하는 데 사용 됩니다. 또한 `VisualElement.CanBecomeFirstResponder` 메서드를 사용 하 여 `VisualElement` 이 터치 이벤트에 대 한 첫 번째 응답자 인지 여부를 반환할 수 있습니다.

결과적으로는 요소를 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 포함 하는 페이지가 아니라 터치 이벤트의 첫 번째 응답자가 될 수 있습니다. 그러면 채팅 응용 프로그램 등을 누를 때 키보드를 해제 하지 않는 등의 시나리오를 사용할 수 있습니다 [`Button`](xref:Xamarin.Forms.Button) .

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
