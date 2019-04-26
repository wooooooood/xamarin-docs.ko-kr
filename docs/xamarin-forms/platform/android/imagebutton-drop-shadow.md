---
title: Android에서 ImageButton 그림자
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서는 ImageButton에 그림자를 사용 하도록 설정 하는 Android 플랫폼 특정을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: D3604D87-9F9F-4FE2-8B10-DF3B143C0734
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 56415aff251149aee01c2e2eb7e335e157180962
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61293779"
---
# <a name="imagebutton-drop-shadows-on-android"></a>Android에서 ImageButton 그림자

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

이 Android 플랫폼별에서 그림자를 사용 하도록 설정 되는 `ImageButton`합니다. 설정 하 여 XAML에서 사용 되는 `ImageButton.IsShadowEnabled` 바인딩 가능한 속성을 `true`, 그림자를 제어 하는 추가 선택적 바인딩 가능한 속성의 수와 함께 합니다.

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

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

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
> 일부로 그림자를 그릴 합니다 `ImageButton` 배경색과 배경만 그려지는 경우를 `BackgroundColor` 속성을 설정 합니다. 따라서 그림자를 그릴 수 없는 경우는 `ImageButton.BackgroundColor` 속성이 설정 되지 않습니다.

`ImageButton.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. `ImageButton.SetIsShadowEnabled` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 그림자에 사용 되는지 여부를 제어 하려면 네임 스페이스는는 `ImageButton`합니다. 또한 그림자를 제어 하려면 다음 메서드를 호출할 수 있습니다.

- `SetShadowColor` – 그림자의 색을 설정 합니다. 기본 색은 [ `Color.Default` ](xref:Xamarin.Forms.Color.Default*)합니다.
- `SetShadowOffset` – 그림자의 오프셋을 설정 합니다. 그림자 캐스팅 된 및로 지정 된 방향을 변경 하는 오프셋 된 [ `Size` ](xref:Xamarin.Forms.Size) 값입니다. `Size` 구조 값의 첫 번째 값 (음수) 왼쪽 또는 오른쪽 (양수) 까지의 거리와 두 번째 되 위의 거리 값 (음수 값) 또는 (양수) 아래 장치 독립적 단위 표현 됩니다 . 이 속성의 기본값은 (0.0, 0.0)의 모든 관련 캐스팅은 섀도 있으며 그 결과 `ImageButton`합니다.
- `SetShadowRadius`– 그림자를 렌더링 하는 데 흐리게 반경이 설정 합니다. Radius 기본값은 10.0입니다.

> [!NOTE]
> 호출 하 여 그림자의 상태를 쿼리할 수 있습니다 합니다 `GetIsShadowEnabled`, `GetShadowColor`를 `GetShadowOffset`, 및 `GetShadowRadius` 메서드.

결과에서 그림자를 사용할 수 있습니다는 `ImageButton`:

![](imagebutton-drop-shadow-images/imagebutton-drop-shadow.png "그림자를 사용 하 여 ImageButton")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
