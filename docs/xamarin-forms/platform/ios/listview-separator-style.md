---
title: IOS에서 ListView 구분 기호 스타일
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서는 ListView에 있는 셀 사이의 구분 기호는 ListView의 전체 너비를 사용 하는지 여부를 제어 하는 iOS 플랫폼 전용을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: A4CB45CE-9FB7-47ED-8C3D-93E39BF282E4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 0fc8cdfac22ad54b73af193ce76e8fd27b8fb0da
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60949037"
---
# <a name="listview-separator-style-on-ios"></a>IOS에서 ListView 구분 기호 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

이 iOS 플랫폼별 사이의 구분 기호 셀에 있는지 여부를 제어를 [ `ListView` ](xref:Xamarin.Forms.ListView) 의 전체 너비를 사용 하는 `ListView`합니다. 설정 하 여 XAML에서 사용 되는 [ `ListView.SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SeparatorStyleProperty) 연결 된 속성의 값에는 [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) 열거형:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.SeparatorStyle="FullWidth">
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

listView.On<iOS>().SetSeparatorStyle(SeparatorStyle.FullWidth);
```

`ListView.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. [ `ListView.SetSeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SetSeparatorStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 사이의 구분 기호 셀에 있는지 여부를 네임 스페이스를 제어 하는 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 전체를 사용 하 여 너비를 `ListView`를 사용 하 여 합니다 [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) 가능한 두 값을 제공 하는 열거형:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.Default) – 기본 iOS 구분 동작을 나타냅니다. 이것이 Xamarin.Forms의 기본 동작입니다.
- [`FullWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.FullWidth) -구분 기호의 한쪽 가장자리에서 그려짐을 나타냅니다는 `ListView` 다른 합니다.

결과 지정한 [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) 값이 적용 되는 [ `ListView` ](xref:Xamarin.Forms.ListView), 셀 사이 있는 구분선의 너비를 제어 하는:

![](listview-separator-style-images/listview-separatorstyle.png "ListView SeparatorStyle 플랫폼별")

> [!NOTE]
> 구분 기호 스타일 설정한 후 `FullWidth`를 다시 변경할 수 없습니다 `Default` 런타임 시.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
