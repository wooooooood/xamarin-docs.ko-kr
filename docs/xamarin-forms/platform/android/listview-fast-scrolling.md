---
title: Android에서 ListView 빠르게 스크롤
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 ListView에서 데이터를 신속 하 게 스크롤할 수 있도록 Android 플랫폼 관련 기능을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 37D95A2D-74AC-488A-B903-2BDD799EAA5C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: ce51483da9599cf049cf005ae18b35d110aa325b
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649983"
---
# <a name="listview-fast-scrolling-on-android"></a>Android에서 ListView 빠르게 스크롤

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 Android 플랫폼 전용은에서 [`ListView`](xref:Xamarin.Forms.ListView)데이터를 빠르게 스크롤할 수 있도록 하는 데 사용 됩니다. 설정 하 여 XAML에서 사용 되는 `ListView.IsFastScrollEnabled` 연결 된 속성을 `boolean` 값:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        ...
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  GroupDisplayBinding="{Binding Key}"
                  IsGroupingEnabled="true"
                  android:ListView.IsFastScrollEnabled="true">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

`ListView.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. `ListView.SetIsFastScrollEnabled` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스에서 데이터를 통해 빠른 스크롤을 사용 하는 [ `ListView` ](xref:Xamarin.Forms.ListView)합니다. 또한 합니다 `SetIsFastScrollEnabled` 메서드를 호출 하 여 빠른 스크롤 설정/해제를 사용할 수는 `IsFastScrollEnabled` 빠른 스크롤 사용 되는지 여부를 반환 하는 방법.

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

결과에서 데이터를 통해 빠른 스크롤 하는 한 [ `ListView` ](xref:Xamarin.Forms.ListView) 스크롤의 크기를 변경 하는 사용할 수 있습니다.

[![](listview-fast-scrolling-images/fastscroll.png "ListView FastScroll 플랫폼별")](listview-fast-scrolling-images/fastscroll-large.png#lightbox "ListView FastScroll 플랫폼별")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
