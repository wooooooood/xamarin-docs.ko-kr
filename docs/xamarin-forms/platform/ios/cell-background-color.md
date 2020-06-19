---
title: IOS의 셀 배경색
description: 플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 iOS의 셀에 대 한 기본 배경색을 설정 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 2A3FDACF-5AE2-40DE-8488-6FE41733712F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 90282262926fef663183be247e37d64dd1be9124
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
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
