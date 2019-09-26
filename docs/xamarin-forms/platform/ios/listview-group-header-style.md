---
title: IOS의 ListView 그룹 머리글 스타일
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 스크롤 하는 동안 ListView 머리글 셀의 부동 여부를 제어 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 099B2C7F-727F-4BCF-903B-87E728108C24
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: f40737799f63c6e0c61fcc6f4f59584222a49d6d
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "68648316"
---
# <a name="listview-group-header-style-on-ios"></a>IOS의 ListView 그룹 머리글 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼별는 스크롤 하는 [`ListView`](xref:Xamarin.Forms.ListView) 동안 머리글 셀이 부동 되는지 여부를 제어 합니다. `ListView.GroupHeaderStyle` 바인딩 가능한 속성을 `GroupHeaderStyle` 열거형의 값으로 설정 하 여 XAML에서 사용 됩니다.

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.GroupHeaderStyle="Grouped">
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

listView.On<iOS>().SetGroupHeaderStyle(GroupHeaderStyle.Grouped);
```

`ListView.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. 네임 스페이스의 메서드는 스크롤 하는 동안 머리글 셀의 [`ListView`](xref:Xamarin.Forms.ListView) 부동 여부를 제어 하는 데 사용 됩니다. `ListView.SetGroupHeaderStyle` [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 열거형 `GroupHeaderStyle` 은 다음 두 가지 가능한 값을 제공 합니다.

- `Plain`– [`ListView`](xref:Xamarin.Forms.ListView) 가 스크롤 될 때 머리글 셀이 float을 나타냅니다 (기본값).
- `Grouped`– [`ListView`](xref:Xamarin.Forms.ListView) 가 스크롤될 때 머리글 셀이 부동 되지 않음을 나타냅니다.

또한 `ListView.GetGroupHeaderStyle` 메서드를 사용 하 여에 적용 `GroupHeaderStyle` [`ListView`](xref:Xamarin.Forms.ListView)되는를 반환할 수 있습니다.

그러면 지정 `GroupHeaderStyle` 된 값이에 적용 되어 스크롤 중에 [`ListView`](xref:Xamarin.Forms.ListView)머리글 셀의 float 여부를 제어 합니다.

[![IOS의 부동 및 비 부동 ListView 머리글 셀의 스크린샷](listview-group-header-style-images/group-header-styles.png "부동 및 비 부동 머리글 셀이 있는 ListView")](listview-group-header-style-images/group-header-styles-large.png#lightbox "부동 및 비 부동 머리글 셀이 있는 ListView")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
