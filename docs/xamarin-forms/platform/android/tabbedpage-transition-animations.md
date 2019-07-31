---
title: Android의 TabbedPage 페이지 전환 애니메이션
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 TabbedPage에서 페이지를 탐색할 때 전환 애니메이션을 사용 하지 않도록 설정 하는 Android 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 2DB4EA6D-9CED-4137-BAB2-B20A457B1CA3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 4f8d6ec2b06855364970bc9b672c3d3f7b9bfdfc
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649846"
---
# <a name="tabbedpage-page-transition-animations-on-android"></a>Android의 TabbedPage 페이지 전환 애니메이션

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

에서 프로그래밍 방식으로 또는 탭 모음을 사용 하는 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)경우에서 페이지를 탐색 하는 경우이 Android 플랫폼별를 사용 하 여 전환 애니메이션을 사용 하지 않도록 설정 합니다. 설정 하 여 XAML에서 사용 되는 `TabbedPage.IsSmoothScrollEnabled` 바인딩 가능한 속성을 `false`:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.IsSmoothScrollEnabled="false">
    ...
</TabbedPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetIsSmoothScrollEnabled(false);
```

`TabbedPage.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. `TabbedPage.SetIsSmoothScrollEnabled` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 전환 애니메이션의 페이지 간을 탐색할 때 표시 될 수 있는지 여부를 제어 하려면 네임 스페이스는를 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). 또한 합니다 `TabbedPage` 클래스는 `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` 네임 스페이스는 다음 메서드도 들어:

- `IsSmoothScrollEnabled`에 전환 애니메이션의 페이지 간을 탐색할 때 표시 될 수 있는지 여부를 검색 하는 데 사용 됩니다는 `TabbedPage`합니다.
- `EnableSmoothScroll`에 페이지 사이 탐색할 때 전환 애니메이션을 사용 하도록 설정 하는 데 사용 됩니다는 `TabbedPage`합니다.
- `DisableSmoothScroll`에 페이지 사이 탐색할 때 전환 애니메이션을 사용 하지 않도록 설정 하는 데 사용 됩니다는 `TabbedPage`합니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
