---
title: IOS의 VisualElement 드롭 그림자
description: 플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 VisualElement에서 그림자를 사용 하도록 설정 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 2147FD66-058E-4BE5-840A-369842B26EC4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7410386e10f605fdeed452fe37755c1e48e6b9b9
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136996"
---
# <a name="visualelement-drop-shadows-on-ios"></a>IOS의 VisualElement 드롭 그림자

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼 전용은에서 그림자를 설정 하는 데 사용 됩니다 [`VisualElement`](xref:Xamarin.Forms.VisualElement) . 이 메서드는 연결 된 속성을로 설정 하 여 XAML에서 사용 되며 [`VisualElement.IsShadowEnabled`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsShadowEnabledProperty) `true` , 드롭 그림자를 제어 하는 여러 추가 선택적 연결 속성과 함께 사용 됩니다.

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <BoxView ...
                 ios:VisualElement.IsShadowEnabled="true"
                 ios:VisualElement.ShadowColor="Purple"
                 ios:VisualElement.ShadowOpacity="0.7"
                 ios:VisualElement.ShadowRadius="12">
            <ios:VisualElement.ShadowOffset>
                <Size>
                    <x:Arguments>
                        <x:Double>10</x:Double>
                        <x:Double>10</x:Double>
                    </x:Arguments>
                </Size>
            </ios:VisualElement.ShadowOffset>
         </BoxView>
        ...
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var boxView = new BoxView { Color = Color.Aqua, WidthRequest = 100, HeightRequest = 100 };
boxView.On<iOS>()
       .SetIsShadowEnabled(true)
       .SetShadowColor(Color.Purple)
       .SetShadowOffset(new Size(10,10))
       .SetShadowOpacity(0.7)
       .SetShadowRadius(12);
```

`VisualElement.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. [ `VisualElement.SetIsShadowEnabled` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . VisualElement}, system.string) 메서드는 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스에서 드롭 그림자가 사용 되는지 여부를 제어 하는 데 사용 됩니다 `VisualElement` . 또한 다음 메서드를 호출 하 여 그림자를 제어할 수 있습니다.

- [ `SetShadowColor` ] (f: Xamarin.Forms . PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . VisualElement}, Xamarin.Forms . Color) – 그림자의 색을 설정 합니다. 기본 색은 [`Color.Default`](xref:Xamarin.Forms.Color.Default*) 입니다.
- [ `SetShadowOffset` ] (f: Xamarin.Forms . PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . VisualElement}, Xamarin.Forms . 크기)) – 그림자의 오프셋을 설정 합니다. 오프셋은 그림자가 캐스팅 되는 방향을 변경 하 고 값으로 지정 됩니다 [`Size`](xref:Xamarin.Forms.Size) . `Size`구조 값은 왼쪽 (음수 값) 또는 오른쪽 (양수 값)에 대 한 첫 번째 값이 고 두 번째 값은 위 거리가 (음수 값) 이하인 (양수 값) 인 장치 독립적 단위로 표현 됩니다. 이 속성의 기본값은 (0.0, 0.0) 이며이로 인해의 모든 측면에서 그림자가 캐스팅 됩니다 `VisualElement` .
- [ `SetShadowOpacity` ] (f: Xamarin.Forms . PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . VisualElement}, system.string)) – 값이 0.0 (투명)에서 1.0 (불투명) 사이의 값을 사용 하 여 그림자의 불투명도를 설정 합니다. 기본 불투명도 값은 0.5입니다.
- [ `SetShadowRadius` ] (f: Xamarin.Forms . PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . VisualElement}, system.string)) – 그림자를 렌더링 하는 데 사용 되는 흐리게 반경을 설정 합니다. 기본 반지름 값은 10.0입니다.

> [!NOTE]
> [ `GetIsShadowEnabled` ] (F:)를 호출 하 여 그림자 상태를 쿼리할 수 있습니다 Xamarin.Forms . PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . VisualElement})), [ `GetShadowColor` ] (f: Xamarin.Forms . PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . VisualElement})), [ `GetShadowOffset` ] (f: Xamarin.Forms . PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . VisualElement})), [ `GetShadowOpacity` ] (f: Xamarin.Forms . PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . VisualElement})) 및 [ `GetShadowRadius` ] (f: Xamarin.Forms . PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . VisualElement})) 메서드.

그 결과,에서 그림자를 사용 하도록 설정할 수 있습니다 [`VisualElement`](xref:Xamarin.Forms.VisualElement) .

![](drop-shadow-images/drop-shadow.png "Drop shadow enabled")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
