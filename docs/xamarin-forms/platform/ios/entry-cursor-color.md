---
title: IOS의 항목 커서 색
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 항목의 커서 색을 설정 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 867D70BA-53F9-4434-8094-85D71DCECC2D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: ff88302755f1c1947591d90ab3373dae7e7db8ec
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656688"
---
# <a name="entry-cursor-color-on-ios"></a>IOS의 항목 커서 색

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼 전용은의 [`Entry`](xref:Xamarin.Forms.Entry) 커서 색을 지정 된 색으로 설정 합니다. 설정 하 여 XAML에서 사용 되는 [ `Entry.CursorColor` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.CursorColorProperty) 바인딩 가능한 속성을 [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Entry ... ios:Entry.CursorColor="LimeGreen" />
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var entry = new Xamarin.Forms.Entry();
entry.On<iOS>().SetCursorColor(Color.LimeGreen);
```

`Entry.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. [ `Entry.SetCursorColor` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetCursorColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry},Xamarin.Forms.Color)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 커서 색 지정을 설정 하는 네임 스페이스 [ `Color` ](xref:Xamarin.Forms.Color)합니다. 또한 합니다 [ `Entry.GetCursorColor` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.GetCursorColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry})) 메서드를 사용 하 여 현재 커서 색을 검색할 수 있습니다.

결과 커서에서 색인을 [ `Entry` ](xref:Xamarin.Forms.Entry) 특정으로 설정할 수 있습니다 [ `Color` ](xref:Xamarin.Forms.Color):

![](entry-cursor-color-images/entry-cursorcolor.png "커서 색 항목")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
