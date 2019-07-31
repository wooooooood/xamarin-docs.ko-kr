---
title: IOS의 ListView 행 애니메이션
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 ListView 항목 컬렉션을 업데이트할 때 행 애니메이션을 사용 하지 않도록 설정할지 여부를 제어 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: E8F5103F-4D8E-4A5A-A16C-7FA14EE786AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/21/2019
ms.openlocfilehash: 9e44c22e670847102cf0bc0f79a860abcac30760
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652830"
---
# <a name="listview-row-animations-on-ios"></a>IOS의 ListView 행 애니메이션

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼 특정 항목 컬렉션을 [`ListView`](xref:Xamarin.Forms.ListView) 업데이트 하는 동안 행 애니메이션을 사용 하지 않도록 설정할지 여부를 제어 합니다. 설정 하 여 XAML에서 사용 되는 `ListView.RowAnimationsEnabled` 바인딩 가능한 속성을 `false`:

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

`ListView.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. 네임 스페이스의 메서드는 [`ListView`](xref:Xamarin.Forms.ListView) items 컬렉션을 업데이트할 때 행 애니메이션을 사용 하지 않도록 설정할지 여부를 제어 하는 데 사용 됩니다. `ListView.SetRowAnimationsEnabled` [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 또한 `ListView.GetRowAnimationsEnabled` 메서드를 사용 하 여 `ListView`에서 행 애니메이션을 사용 하지 않도록 설정할지 여부를 반환할 수 있습니다.

> [!NOTE]
> [`ListView`](xref:Xamarin.Forms.ListView)행 애니메이션은 기본적으로 사용 하도록 설정 됩니다. 따라서에 새 행이 삽입 `ListView`될 때 애니메이션이 발생 합니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
