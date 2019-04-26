---
title: IOS에서 셀 배경색
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에는 iOS에서 셀의 기본 배경색을 설정 하는 iOS 플랫폼 특정을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 2A3FDACF-5AE2-40DE-8488-6FE41733712F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 396e674bb3e5642f7c54455ee9e30ba5bf232f18
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61030090"
---
# <a name="cell-background-color-on-ios"></a>IOS에서 셀 배경색

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

기본 배경색을 설정 하는이 iOS 플랫폼별 [ `Cell` ](xref:Xamarin.Forms.Cell) 인스턴스. 설정 하 여 XAML에서 사용 되는 `Cell.DefaultBackgroundColor` 바인딩 가능한 속성을 [ `Color` ](xref:Xamarin.Forms.Color):

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

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var viewCell = new ViewCell { View = ... };
viewCell.On<iOS>().SetDefaultBackgroundColor(Color.Teal);
```

`ListView.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. 합니다 `Cell.SetDefaultBackgroundColor` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스를 지정 된 셀 배경색을 설정 [ `Color` ](xref:Xamarin.Forms.Color)합니다. 또한는 `Cell.DefaultBackgroundColor` 메서드를 사용 하 여 현재 셀 배경색을 검색할 수 있습니다.

결과 배경색을 [ `Cell` ](xref:Xamarin.Forms.Cell) 특정으로 설정할 수 있습니다 [ `Color` ](xref:Xamarin.Forms.Color):

[![IOS에서 청록색 그룹 머리글 셀을 스크린샷](cell-background-color-images/group-header-cell-color.png "청록 그룹 머리글 셀을 사용 하 여 ListView")](cell-background-color-images/group-header-cell-color-large.png#lightbox "청록 그룹 머리글 셀을 사용 하 여 ListView")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
