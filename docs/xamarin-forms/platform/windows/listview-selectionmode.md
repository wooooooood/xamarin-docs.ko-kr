---
title: Windows에서 ListView SelectionMode
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서는 Windows 플랫폼 관련 항목을 ListView에서 탭 제스처에 응답할 수 있는지 여부를 제어 하는 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 57EF3A7F-1407-4B31-AE21-D149293D4228
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: d2a94347448af031f50341729d77c7385225d107
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54209588"
---
# <a name="listview-selectionmode-on-windows"></a>Windows에서 ListView SelectionMode

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

유니버설 Windows 플랫폼에서 Xamarin.Forms 기본적 [ `ListView` ](xref:Xamarin.Forms.ListView) 네이티브를 사용 하 여 `ItemClick` 네이티브 보다는 상호 작용에 응답할 이벤트 `Tapped` 이벤트입니다. Windows 내레이터와 키보드 상호 작용할 수 있도록 내게 필요한 옵션 기능을 제공 합니다 `ListView`합니다. 그러나 렌더링 내에서 모든 탭 제스처를 `ListView` 작동 하지 않습니다.

이 유니버설 Windows 플랫폼, 플랫폼별 제어 여부를 항목을 [ `ListView` ](xref:Xamarin.Forms.ListView) 제스처와 탭에 응답할 수 있습니다 이므로 여부를 네이티브 `ListView` 발생을 `ItemClick` 또는 `Tapped` 이벤트. 설정 하 여 XAML에서 사용 되는 [ `ListView.SelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) 연결 된 속성의 값에는 [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) 열거형:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <ListView ... windows:ListView.SelectionMode="Inaccessible">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

`ListView.On<Windows>` 메서드가 플랫폼별 유니버설 Windows 플랫폼에만 실행 되도록 지정 합니다. [ `ListView.SetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 네임 스페이스에는 컨트롤에 있는지 여부 항목을 [ `ListView` ](xref:Xamarin.Forms.ListView) 제스처를 사용 하 여 탭에 응답할 수 있습니다는 [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) 열거 가능한 두 값을 제공 합니다.

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) – 나타내는 `ListView` 네이티브 시기가 `ItemClick` 상호 작용을 처리 하 고 따라서 내게 필요한 옵션 기능을 제공 하는 이벤트입니다. 따라서 Windows 내레이터와 키보드 상호 작용할 수는 `ListView`합니다. 그러나 항목에 `ListView` 탭 제스처에 응답할 수 없습니다. 이 대 한 기본 동작 `ListView` 유니버설 Windows 플랫폼에서 인스턴스.
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) – 나타내는 합니다 `ListView` 네이티브 시기가 `Tapped` 상호 작용을 처리 하는 이벤트입니다. 따라서 항목을 `ListView` 탭 제스처에 응답할 수 있습니다. 그러나 내게 필요한 옵션 기능은 없습니다 하 고 Windows 내레이터와 키보드 상호 작용할 수 없다는 따라서는 `ListView`합니다.

> [!NOTE]
> 합니다 `Accessible` 하 고 `Inaccessible` 선택 모드 상호 배타적 이며 액세스 가능한 중에서 선택 해야 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 또는 `ListView` 탭 제스처에 응답할 수 있는 합니다.

또한 합니다 [ `GetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.GetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView})) 메서드를 사용 하 여 현재를 반환할 수 있습니다 [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)합니다.

결과 지정 된 [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) 에 적용 되는 [ `ListView` ](xref:Xamarin.Forms.ListView)를 제어 하는 여부를 항목는 `ListView` 탭 제스처에 응답할 수 있습니다 이므로 여부를 네이티브 `ListView` 발생 합니다 `ItemClick` 또는 `Tapped` 이벤트입니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
