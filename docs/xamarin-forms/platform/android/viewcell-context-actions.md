---
title: Android의 ViewCell 컨텍스트 작업
description: 플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 ViewCell 컨텍스트 작업 레거시 모드를 사용 하도록 설정 하는 Android 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 8BD71B10-5037-443F-9975-B941CE393E5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 053697921f1adacc102e9e9bee9dd17f8d44526b
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128559"
---
# <a name="viewcell-context-actions-on-android"></a>Android의 ViewCell 컨텍스트 작업

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

4.3 기본적으로 Xamarin.Forms [`ViewCell`](xref:Xamarin.Forms.ViewCell) Android 응용 프로그램의가의 각 항목에 대 한 컨텍스트 작업을 정의 하는 경우에서 [`ListView`](xref:Xamarin.Forms.ListView) 선택한 항목이 변경 되 면 컨텍스트 작업 메뉴가 업데이트 됩니다 `ListView` . 그러나 이전 버전의 Xamarin.Forms 컨텍스트 작업 메뉴에서는 업데이트 되지 않았으므로이 동작을 `ViewCell` 레거시 모드 라고 합니다. 이 레거시 모드를 사용 하면에서를 사용 하 여 `ListView` [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) `ItemTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 다른 컨텍스트 작업을 정의 하는 개체에서를 설정 하는 경우 잘못 된 동작이 발생할 수 있습니다.

이 Android 플랫폼별를 사용 하면 [`ViewCell`](xref:Xamarin.Forms.ViewCell) 이전 버전과의 호환성을 위해 컨텍스트 작업 메뉴 레거시 모드를 사용할 수 있으므로에서 선택한 항목이 변경 될 때 컨텍스트 작업 메뉴가 업데이트 되지 않습니다 [`ListView`](xref:Xamarin.Forms.ListView) . 바인딩 가능한 속성을로 설정 하 여 XAML에서 사용 됩니다 `ViewCell.IsContextActionsLegacyModeEnabled` `true` .

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ItemsSource="{Binding Items}">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell android:ViewCell.IsContextActionsLegacyModeEnabled="true">
                        <ViewCell.ContextActions>
                            <MenuItem Text="{Binding Item1Text}" />
                            <MenuItem Text="{Binding Item2Text}" />
                        </ViewCell.ContextActions>
                        <Label Text="{Binding Text}" />
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

viewCell.On<Android>().SetIsContextActionsLegacyModeEnabled(true);
```

`ViewCell.On<Android>`메서드는이 플랫폼별가 Android 에서만 실행 되도록 지정 합니다. `ViewCell.SetIsContextActionsLegacyModeEnabled`네임 스페이스의 메서드는 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 컨텍스트 작업 메뉴 레거시 모드를 사용 하도록 설정 하는 데 사용 [`ViewCell`](xref:Xamarin.Forms.ViewCell) 되므로에서 선택한 항목이 변경 될 때 컨텍스트 작업 메뉴가 업데이트 되지 않습니다 [`ListView`](xref:Xamarin.Forms.ListView) . 또한 `ViewCell.GetIsContextActionsLegacyModeEnabled` 메서드를 사용 하 여 컨텍스트 작업 레거시 모드가 사용 되는지 여부를 반환할 수 있습니다.

다음 스크린샷에는 [`ViewCell`](xref:Xamarin.Forms.ViewCell) 컨텍스트 작업 레거시 모드가 사용 됩니다.

![Android에서 ViewCell 레거시 모드 사용의 스크린샷](viewcell-context-actions-images/legacy-mode-enabled.png "ViewCell 레거시 모드 사용")

이 모드에서 셀 2에 대해 정의 되는 상황에 맞는 메뉴 항목이 서로 다른 경우에도 표시 되는 컨텍스트 작업 메뉴 항목은 셀 1과 셀 2에 대해 동일 합니다.

다음 스크린샷에는 [`ViewCell`](xref:Xamarin.Forms.ViewCell) 기본 동작인 컨텍스트 작업 레거시 모드 사용 안 함이 표시 됩니다 Xamarin.Forms .

![Android에서 ViewCell 레거시 모드 사용 안 함의 스크린샷](viewcell-context-actions-images/legacy-mode-disabled.png "ViewCell 레거시 모드 사용 안 함")

이 모드에서는 셀 1과 셀 2에 대 한 올바른 컨텍스트 작업 메뉴 항목이 표시 됩니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
