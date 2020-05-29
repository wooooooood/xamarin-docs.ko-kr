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
ms.openlocfilehash: 5554341493b52d20c946a4bcfe2d1230e4a02759
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135566"
---
# <a name="button-padding-and-shadows-on-android"></a>Android의 단추 패딩 및 그림자

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 Android 플랫폼 특정 Xamarin.Forms 은 단추가 android 단추의 기본 패딩 및 그림자 값을 사용 하는지 여부를 제어 합니다. [`Button.UseDefaultPadding`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty)및 [`Button.UseDefaultShadow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty) 연결 된 속성을 값으로 설정 하 여 XAML에서 사용 됩니다 `boolean` .

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

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

`Button.On<Android>`메서드는이 플랫폼별가 Android 에서만 실행 되도록 지정 합니다. [ `Button.SetUseDefaultPadding` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. SetUseDefaultPadding ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . }, System.string) 및 [ `Button.SetUseDefaultShadow` ] (f:)를 클릭 Xamarin.Forms 합니다. PlatformConfiguration. SetUseDefaultShadow ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . Button}, system.string) 메서드는 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스에서 Xamarin.Forms 단추가 Android 단추의 기본 패딩 및 그림자 값을 사용 하는지 여부를 제어 하는 데 사용 됩니다. 또한 [ `Button.UseDefaultPadding` ] (f: Xamarin.Forms 입니다. PlatformConfiguration. UseDefaultPadding ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . Button})) 및 [ `Button.UseDefaultShadow` ] (f: Xamarin.Forms . PlatformConfiguration. UseDefaultShadow ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . Button}) 메서드를 사용 하 여 단추에 기본 패딩 값과 기본 그림자 값을 각각 사용할지 여부를 반환할 수 있습니다.

그러면 Xamarin.Forms 단추는 Android 단추의 기본 패딩 및 그림자 값을 사용할 수 있습니다.

![](button-padding-shadow-images/button-padding-and-shadow.png "Default Padding and Shadow Values on Android Buttons")

위의 스크린샷에서는 [`Button`](xref:Xamarin.Forms.Button) 동일한 정의가 있습니다. 단, 오른쪽에는 `Button` Android 단추의 기본 패딩 및 그림자 값이 사용 됩니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
