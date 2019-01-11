---
title: IOS에서 선택 항목 선택
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에는 선택에서 항목을 선택할 경우를 제어 하는 iOS 플랫폼 전용을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 26B0604A-BD30-49FD-83A6-F0EDFBB0524B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 21c4c289a3fd30db890be6811875412ce4913cf5
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54208718"
---
# <a name="picker-item-selection-on-ios"></a>IOS에서 선택 항목 선택

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

이 iOS 플랫폼 관련 항목 선택에서 발생 하는 경우 제어는 [ `Picker` ](xref:Xamarin.Forms.Picker), 항목 선택 컨트롤에서 항목을 검색할 때 또는 한 번만 발생 하도록 지정할 수 있도록 허용 합니다 **수행** 단추가 눌러져 있습니다. 설정 하 여 XAML에서 사용 되는 `Picker.UpdateMode` 연결 된 속성의 값으로는 `UpdateMode` 열거형:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <Picker ... Title="Select a monkey" ios:Picker.UpdateMode="WhenFinished">
          ...
        </Picker>
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`Picker.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. `Picker.SetUpdateMode` 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스에는 제어 하는 데 항목을 선택할 경우 사용 하 여를 `UpdateMode` 가능한 두 값을 제공 하는 열거형:

- `Immediately` – 사용자가 항목을 탐색할 때 항목 선택이 발생 합니다 [ `Picker` ](xref:Xamarin.Forms.Picker)합니다. 이것이 Xamarin.Forms의 기본 동작입니다.
- `WhenFinished` – 항목 선택 눌렀음을 후에 발생 합니다 **수행** 단추를 [ `Picker` ](xref:Xamarin.Forms.Picker)합니다.

또한 합니다 `SetUpdateMode` 메서드를 호출 하 여 열거형 값을 설정/해제를 사용할 수는 `UpdateMode` 현재 반환 하는 메서드 `UpdateMode`:

```csharp
switch (picker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        picker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

결과 지정한 `UpdateMode` 에 적용 되는 [ `Picker` ](xref:Xamarin.Forms.Picker), 항목을 선택할 경우 제어:

[![](picker-selection-images/picker-updatemode.png "선택기 UpdateMode 플랫폼별")](picker-selection-images/picker-updatemode-large.png#lightbox "선택기 UpdateMode 플랫폼별")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
