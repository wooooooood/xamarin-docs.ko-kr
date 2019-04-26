---
title: Android에서 VisualElement 레거시 색 모드
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Xamarin.Forms 레거시 색 모드를 사용 하지 않도록 설정 하는 Android 플랫폼 특정을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 37D95A2D-74AC-488A-B903-2BDD799EAA5C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: c04cb4a48a984ed9854ae791e554d33b665241ea
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61388984"
---
# <a name="visualelement-legacy-color-mode-on-android"></a>Android에서 VisualElement 레거시 색 모드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Xamarin.Forms 뷰의 일부 레거시 색 모드를 기능입니다. 이 모드에서는 때 합니다 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) 뷰의 속성 `false`, 보기에는 색 사용 안 함된 상태에 대 한 기본 네이티브 색을 사용 하 여 사용자 설정 보다 우선 합니다. 이전 버전과 호환성을 위해이 레거시 색 모드는 지원 되는 보기에 대 한 기본 동작을 유지 합니다.

이 Android 플랫폼 특정 뷰를 사용 하지 않도록 설정 하는 경우에 사용자가 설정한 뷰에 색 유지 되도록이 레거시 색 모드를 해제 합니다. 설정 하 여 XAML에서 사용 되는 [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty) 연결 된 속성을 `false`:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                android:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Android>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. 합니다 [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement},System.Boolean)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 레거시 색 모드가 비활성화 되어 있는지 여부를 제어 하려면 네임 스페이스는 합니다. 또한 합니다 [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement})) 메서드를 사용 하 여 레거시 색 모드를 비활성화 되었는지 여부를 반환할 수 있습니다.

결과 보기를 비활성화 하는 경우 사용자가 설정한 뷰에 색도 계속 되도록 레거시 색 모드를 비활성화할 수 있습니다.

![](legacy-color-mode-images/legacy-color-mode-disabled.png "레거시 색 모드 사용 안 함")

> [!NOTE]
> 설정 하는 경우는 [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) 레거시 색 모드는 완전히 뷰에서 무시 됩니다. 시각적 상태에 대 한 자세한 내용은 참조 하세요. [은 Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)합니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
