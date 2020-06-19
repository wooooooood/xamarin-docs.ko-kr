---
title: Android에서 ListView 빠르게 스크롤
description: 플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 ListView에서 데이터를 신속 하 게 스크롤할 수 있도록 Android 플랫폼 관련 기능을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 37D95A2D-74AC-488A-B903-2BDD799EAA5C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 30e6a39b1a7649fbb9e09dfeeb85ee889da68fc1
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128806"
---
# <a name="listview-fast-scrolling-on-android"></a>Android에서 ListView 빠르게 스크롤

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 Android 플랫폼 전용은에서 데이터를 빠르게 스크롤할 수 있도록 하는 데 사용 됩니다 [`ListView`](xref:Xamarin.Forms.ListView) . 연결 된 속성을 값으로 설정 하 여 XAML에서 사용 됩니다 `ListView.IsFastScrollEnabled` `boolean` .

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

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

`ListView.On<Android>`메서드는이 플랫폼별가 Android 에서만 실행 되도록 지정 합니다. `ListView.SetIsFastScrollEnabled`네임 스페이스의 메서드를 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 사용 하 여에서 데이터를 신속 하 게 스크롤할 수 [`ListView`](xref:Xamarin.Forms.ListView) 있습니다. 또한 메서드를 `SetIsFastScrollEnabled` 호출 하 여 `IsFastScrollEnabled` 빠른 스크롤 사용 여부를 반환 하는 메서드를 호출 하 여 빠른 스크롤을 설정/해제 하는 데 메서드를 사용할 수 있습니다.

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

그 결과 스크롤 막대의 크기를 변경 하는의 데이터를 통한 빠른 스크롤을 [`ListView`](xref:Xamarin.Forms.ListView) 사용 하도록 설정할 수 있습니다.

[![](listview-fast-scrolling-images/fastscroll.png "ListView FastScroll Platform-Specific")](listview-fast-scrolling-images/fastscroll-large.png#lightbox "ListView FastScroll Platform-Specific")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
