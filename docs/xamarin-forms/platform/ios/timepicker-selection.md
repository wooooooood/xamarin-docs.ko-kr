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
ms.openlocfilehash: 009482c8f1e90aaa2f592ea04d8fd4f0f31324e8
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137022"
---
# <a name="timepicker-item-selection-on-ios"></a>IOS에서 TimePicker 항목 선택

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼 특정 컨트롤은에서 항목 선택을 수행 하는 경우 [`TimePicker`](xref:Xamarin.Forms.TimePicker) 사용자가 컨트롤에서 항목을 검색할 때 또는 **완료** 단추를 누른 경우에만 항목 선택이 발생 하도록 지정할 수 있도록 합니다. `TimePicker.UpdateMode`연결 된 속성을 열거형의 값으로 설정 하 여 XAML에서 사용 됩니다 `UpdateMode` .

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
       <TimePicker Time="14:00:00"
                   ios:TimePicker.UpdateMode="WhenFinished" />
       ...
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

timePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`TimePicker.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. `TimePicker.SetUpdateMode`네임 스페이스의 메서드는 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) `UpdateMode` 다음 두 가지 가능한 값을 제공 하는 열거형을 사용 하 여 항목 선택이 발생 하는 시기를 제어 하는 데 사용 됩니다.

- `Immediately`– 항목 선택은 사용자가의 항목을 탐색할 때 발생 합니다 [`TimePicker`](xref:Xamarin.Forms.TimePicker) . 이는의 기본 동작입니다 Xamarin.Forms .
- `WhenFinished`– 항목 선택은 사용자가에서 [ **완료** ] 단추를 누른 경우에만 발생 합니다 [`TimePicker`](xref:Xamarin.Forms.TimePicker) .

또한 메서드를 `SetUpdateMode` 호출 하 여 `UpdateMode` 현재을 반환 하는 메서드를 호출 하 여 열거형 값을 전환 하는 데 메서드를 사용할 수 있습니다 `UpdateMode` .

```csharp
switch (timePicker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        timePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        timePicker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

그러면 지정 된가에 `UpdateMode` 적용 되어 [`TimePicker`](xref:Xamarin.Forms.TimePicker) 항목 선택이 발생 하는 시기를 제어 합니다.

[![TimePicker 업데이트 모드의 스크린샷](timepicker-selection-images/timepicker-updatemode.png "TimePicker UpdateMode 플랫폼 관련")](timepicker-selection-images/timepicker-updatemode-large.png#lightbox "TimePicker UpdateMode 플랫폼 관련")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
