---
title: IOS에서 TimePicker 항목 선택
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 TimePicker에서 항목 선택을 수행 하는 시기를 제어 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 554AC877-8698-4B8C-A676-80DD64305A06
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
ms.openlocfilehash: 818f368da8ebb375fbacd97d3d48185ba60470d4
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77646679"
---
# <a name="timepicker-item-selection-on-ios"></a>IOS에서 TimePicker 항목 선택

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼 특정 컨트롤은 [`TimePicker`](xref:Xamarin.Forms.TimePicker)에서 항목 선택이 발생할 때 컨트롤에서 항목을 검색할 때 또는 **완료** 단추를 누른 경우에만 항목 선택이 발생 하도록 지정할 수 있습니다. `TimePicker.UpdateMode` 연결 된 속성을 `UpdateMode` 열거형 값으로 설정 하 여 XAML에서 사용 됩니다.

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

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

timePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`TimePicker.On<iOS>` 메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스의 `TimePicker.SetUpdateMode` 메서드는 다음 두 가지 가능한 값을 제공 하는 `UpdateMode` 열거를 사용 하 여 항목 선택이 발생 하는 시기를 제어 하는 데 사용 됩니다.

- `Immediately`-사용자가 [`TimePicker`](xref:Xamarin.Forms.TimePicker)항목을 탐색할 때 항목 선택이 발생 합니다. 이것이 Xamarin.Forms의 기본 동작입니다.
- `WhenFinished`-사용자가 [`TimePicker`](xref:Xamarin.Forms.TimePicker)에서 **완료** 단추를 누르면 항목 선택이 발생 합니다.

또한 `SetUpdateMode` 메서드를 사용 하 여 현재 `UpdateMode`를 반환 하는 `UpdateMode` 메서드를 호출 하 여 열거형 값을 토글할 수 있습니다.

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

그러면 지정 된 `UpdateMode`가 [`TimePicker`](xref:Xamarin.Forms.TimePicker)에 적용 되어 항목 선택이 발생 하는 시기를 제어 합니다.

[![TimePicker 업데이트 모드의 스크린샷](timepicker-selection-images/timepicker-updatemode.png "TimePicker UpdateMode 플랫폼 관련")](timepicker-selection-images/timepicker-updatemode-large.png#lightbox "TimePicker UpdateMode 플랫폼 관련")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
