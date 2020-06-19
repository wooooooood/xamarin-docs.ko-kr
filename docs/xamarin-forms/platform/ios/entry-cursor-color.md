---
title: IOS의 항목 커서 색
description: 플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 항목의 커서 색을 설정 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 867D70BA-53F9-4434-8094-85D71DCECC2D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4d934fd2155a6a088dd543658555bf104b38f302
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138543"
---
# <a name="entry-cursor-color-on-ios"></a>IOS의 항목 커서 색

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼 전용은의 커서 색 [`Entry`](xref:Xamarin.Forms.Entry) 을 지정 된 색으로 설정 합니다. 바인딩 가능한 속성을로 설정 하 여 XAML에서 사용 됩니다 [`Entry.CursorColor`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.CursorColorProperty) [`Color`](xref:Xamarin.Forms.Color) .

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Entry ... ios:Entry.CursorColor="LimeGreen" />
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var entry = new Xamarin.Forms.Entry();
entry.On<iOS>().SetCursorColor(Color.LimeGreen);
```

`Entry.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. [ `Entry.SetCursorColor` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . 입력}, Xamarin.Forms . Color) 메서드는 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스에서 커서 색을 지정 된로 설정 [`Color`](xref:Xamarin.Forms.Color) 합니다. 또한 [ `Entry.GetCursorColor` ] (f: Xamarin.Forms 입니다. PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . Entry})) 메서드를 사용 하 여 현재 커서 색을 검색할 수 있습니다.

따라서의 커서 색을 [`Entry`](xref:Xamarin.Forms.Entry) 특정으로 설정할 수 있습니다 [`Color`](xref:Xamarin.Forms.Color) .

![](entry-cursor-color-images/entry-cursorcolor.png "Entry Cursor Color")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
