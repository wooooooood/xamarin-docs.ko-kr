---
title: 안쪽 여백 및 Android에서 그림자를 단추
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Android 플랫폼 특정 기본 안쪽 여백 및 Android 단추의 섀도 값을 사용 하는 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: BD2B60D1-DE6E-4691-A777-8EC5F560A4E9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: bcdea14ba6a4a9a09a7b35db67794709d6d36c4c
ms.sourcegitcommit: d3f48bfe72bfe03aca247d47bc64bfbfad1d8071
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66741043"
---
# <a name="button-padding-and-shadows-on-android"></a>안쪽 여백 및 Android에서 그림자를 단추

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

이 Android 플랫폼별 기본 안쪽 여백 및 Android 단추의 섀도 값 Xamarin.Forms 단추를 사용 하는지 여부를 제어 합니다. 설정 하 여 XAML에서 사용 되는 [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty) 하 고 [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty) 속성을 연결 `boolean` 값:

```xaml
<ContentPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button ...
                android:Button.UseDefaultPadding="true"
                android:Button.UseDefaultShadow="true" />         
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

`Button.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. [ `Button.SetUseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) 및 [ `Button.SetUseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) 메서드를 합니다 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스 Xamarin.Forms 단추 기본값을 사용 하는지 여부를 제어 하는 데 사용 됩니다 안쪽 여백 및 Android 단추의 섀도 값입니다. 또한 합니다 [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) 및 [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) 단추 기본 각각 값 및 기본값 그림자를 안쪽 여백 사용 하는지 여부를 반환 하려면 메서드를 사용할 수 있습니다.

결과 Xamarin.Forms 단추는 기본 안쪽 여백 및 Android 단추의 섀도 값을 사용할 수 있는:

![](button-padding-shadow-images/button-padding-and-shadow.png "기본 안쪽 여백 및 Android 단추 그림자 값")

위의 각 스크린 샷 [ `Button` ](xref:Xamarin.Forms.Button) 점을 제외 하면 동일한 정의가 오른쪽 `Button` 기본 안쪽 여백 및 Android 단추의 섀도 값을 사용 하 여 합니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
