---
title: IOS에서 ListView 그룹 헤더 스타일
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 ListView 헤더 셀 스크롤 하는 동안 부동 소수점 수 있는지 여부를 제어 하는 iOS 플랫폼 전용을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 099B2C7F-727F-4BCF-903B-87E728108C24
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 58b3787b71cff9b78f1c6b577be6c320367f1cee
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557455"
---
# <a name="listview-group-header-style-on-ios"></a>IOS에서 ListView 그룹 헤더 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

이 iOS 플랫폼 특정 컨트롤 여부 [ `ListView` ](xref:Xamarin.Forms.ListView) 스크롤 하는 동안 머리글 셀 float입니다. 설정 하 여 XAML에서 사용 되는 `ListView.GroupHeaderStyle` 바인딩 가능한 속성의 값을 `GroupHeaderStyle` 열거형:

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

`ListView.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. `ListView.SetGroupHeaderStyle` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스에는 컨트롤에 있는지 여부 [ `ListView` ](xref:Xamarin.Forms.ListView) 스크롤 하는 동안 머리글 셀 float입니다. `GroupHeaderStyle` 열거형은 두 개의 가능한 값을 제공 합니다.

- `Plain` – 머리글 셀 float 시기를 나타냅니다 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 는 스크롤 (기본값).
- `Grouped` – 머리글 셀 때 float 되지 않습니다 나타냅니다 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 스크롤됩니다.

또한 합니다 `ListView.GetGroupHeaderStyle` 메서드를 사용 하 여 반환할 수 있습니다 합니다 `GroupHeaderStyle` 에 적용 되는 [ `ListView` ](xref:Xamarin.Forms.ListView)합니다.

결과 지정 된 `GroupHeaderStyle` 값이 적용 되는 [ `ListView` ](xref:Xamarin.Forms.ListView)를 스크롤 하는 동안 머리글 셀 float 여부를 제어 하는:

[![부동 및 비 부동 ListView 머리글 셀을 iOS에서 스크린샷](listview-group-header-style-images/group-header-styles.png "부동 및 비 부동 머리글 셀을 사용 하 여 ListView")](listview-group-header-style-images/group-header-styles-large.png#lightbox "부동 및 비 부동 머리글 셀을 사용 하 여 ListView")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
