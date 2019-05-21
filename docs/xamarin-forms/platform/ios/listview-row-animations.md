---
title: IOS에서 ListView 행 애니메이션
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 ListView 항목 컬렉션에 업데이트할 때 행 애니메이션 비활성화 되는지 여부를 제어 하는 iOS 플랫폼 전용을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: E8F5103F-4D8E-4A5A-A16C-7FA14EE786AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/21/2019
ms.openlocfilehash: ac71be25492866b1cf2b12d3343c2f4095fc738d
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65925793"
---
# <a name="listview-row-animations-on-ios"></a>IOS에서 ListView 행 애니메이션

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

이 iOS 플랫폼 특정 컨트롤에 애니메이션 여부 행 경우 비활성화 되는 [ `ListView` ](xref:Xamarin.Forms.ListView) 항목 컬렉션을 업데이트 하는 중입니다. 설정 하 여 XAML에서 사용 되는 `ListView.RowAnimationsEnabled` 바인딩 가능한 속성을 `false`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.RowAnimationsEnabled="false">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetRowAnimationsEnabled(false);
```

`ListView.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. `ListView.SetRowAnimationsEnabled` 메서드의를 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 행 애니메이션 인지 네임 스페이스를 제어 하는 시기를 사용 하지 않도록 설정 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 항목 컬렉션을 업데이트 하는 중입니다. 또한 합니다 `ListView.GetRowAnimationsEnabled` 메서드를 사용 하 여 행 애니메이션에서 비활성화 되는지 여부를 반환할 수 있습니다는 `ListView`합니다.

> [!NOTE]
> [`ListView`](xref:Xamarin.Forms.ListView) 행 애니메이션은 기본적으로 활성화 됩니다. 따라서 애니메이션 경우 새 행이 삽입을 `ListView`입니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
