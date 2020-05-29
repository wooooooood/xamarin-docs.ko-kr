---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 90282262926fef663183be247e37d64dd1be9124
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138569"
---
# <a name="cell-background-color-on-ios"></a>IOS의 셀 배경색

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼별는 인스턴스의 기본 배경색을 설정 합니다 [`Cell`](xref:Xamarin.Forms.Cell) . 바인딩 가능한 속성을로 설정 하 여 XAML에서 사용 됩니다 `Cell.DefaultBackgroundColor` [`Color`](xref:Xamarin.Forms.Color) .

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  IsGroupingEnabled="true">
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <ViewCell ios:Cell.DefaultBackgroundColor="Teal">
                        <Label Margin="10,10"
                               Text="{Binding Key}"
                               FontAttributes="Bold" />
                    </ViewCell>
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var viewCell = new ViewCell { View = ... };
viewCell.On<iOS>().SetDefaultBackgroundColor(Color.Teal);
```

`ListView.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. `Cell.SetDefaultBackgroundColor`네임 스페이스의 메서드는 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 셀 배경색을 지정 된로 설정 합니다 [`Color`](xref:Xamarin.Forms.Color) . 또한 `Cell.DefaultBackgroundColor` 메서드를 사용 하 여 현재 셀 배경색을 검색할 수 있습니다.

그 결과의 배경색을 [`Cell`](xref:Xamarin.Forms.Cell) 특정으로 설정할 수 있습니다 [`Color`](xref:Xamarin.Forms.Color) .

[![IOS의 청록 그룹 머리글 셀 스크린샷](cell-background-color-images/group-header-cell-color.png "청록 그룹 머리글 셀이 있는 ListView")](cell-background-color-images/group-header-cell-color-large.png#lightbox "청록 그룹 머리글 셀이 있는 ListView")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
