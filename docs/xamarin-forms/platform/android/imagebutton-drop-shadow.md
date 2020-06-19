---
title: Android의 ImageButton 그림자
description: 플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 ImageButton에서 그림자를 사용 하도록 설정 하는 Android 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: D3604D87-9F9F-4FE2-8B10-DF3B143C0734
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5e2ad97eb5e7db3b832e8fb4340c86904b766b9a
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140002"
---
# <a name="imagebutton-drop-shadows-on-android"></a>Android의 ImageButton 그림자

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 Android 플랫폼 전용은에서 그림자를 설정 하는 데 사용 됩니다 `ImageButton` . 바인딩 가능한 속성을로 설정 하 여 XAML에서 사용 되며 `ImageButton.IsShadowEnabled` `true` , 드롭 그림자를 제어 하는 여러 가지 선택적 바인딩 가능 속성도 함께 사용 됩니다.

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
       <ImageButton ...
                    Source="XamarinLogo.png"
                    BackgroundColor="GhostWhite"
                    android:ImageButton.IsShadowEnabled="true"
                    android:ImageButton.ShadowColor="Gray"
                    android:ImageButton.ShadowRadius="12">
            <android:ImageButton.ShadowOffset>
                <Size>
                    <x:Arguments>
                        <x:Double>10</x:Double>
                        <x:Double>10</x:Double>
                    </x:Arguments>
                </Size>
            </android:ImageButton.ShadowOffset>
        </ImageButton>
        ...
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var imageButton = new Xamarin.Forms.ImageButton { Source = "XamarinLogo.png", BackgroundColor = Color.GhostWhite, ... };
imageButton.On<Android>()
           .SetIsShadowEnabled(true)
           .SetShadowColor(Color.Gray)
           .SetShadowOffset(new Size(10, 10))
           .SetShadowRadius(12);
```

> [!IMPORTANT]
> 그림자는 배경의 일부로 그려지며 `ImageButton` 속성을 설정한 경우에만 배경을 그립니다 `BackgroundColor` . 따라서 속성이 설정 되지 않은 경우에는 그림자가 그려지지 않습니다 `ImageButton.BackgroundColor` .

`ImageButton.On<Android>`메서드는이 플랫폼별가 Android 에서만 실행 되도록 지정 합니다. `ImageButton.SetIsShadowEnabled`네임 스페이스의 메서드는 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 에서 그림자를 사용할 수 있는지 여부를 제어 하는 데 사용 됩니다 `ImageButton` . 또한 다음 메서드를 호출 하 여 그림자를 제어할 수 있습니다.

- `SetShadowColor`– 그림자의 색을 설정 합니다. 기본 색은 [`Color.Default`](xref:Xamarin.Forms.Color.Default*) 입니다.
- `SetShadowOffset`– 그림자의 오프셋을 설정 합니다. 오프셋은 그림자가 캐스팅 되는 방향을 변경 하 고 값으로 지정 됩니다 [`Size`](xref:Xamarin.Forms.Size) . `Size`구조 값은 왼쪽 (음수 값) 또는 오른쪽 (양수 값)에 대 한 첫 번째 값이 고 두 번째 값은 위 거리가 (음수 값) 이하인 (양수 값) 인 장치 독립적 단위로 표현 됩니다. 이 속성의 기본값은 (0.0, 0.0) 이며이로 인해의 모든 측면에서 그림자가 캐스팅 됩니다 `ImageButton` .
- `SetShadowRadius`– 그림자를 렌더링 하는 데 사용 되는 흐리게 반경을 설정 합니다. 기본 반지름 값은 10.0입니다.

> [!NOTE]
> 그림자 상태는 `GetIsShadowEnabled` ,, `GetShadowColor` `GetShadowOffset` 및 메서드를 호출 하 여 쿼리할 수 있습니다 `GetShadowRadius` .

그 결과,에서 그림자를 사용 하도록 설정할 수 있습니다 `ImageButton` .

![](imagebutton-drop-shadow-images/imagebutton-drop-shadow.png "ImageButton with drop shadow")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
