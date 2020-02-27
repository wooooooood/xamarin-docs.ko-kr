---
title: IOS에서 DatePicker 항목 선택
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 DatePicker에서 항목 선택이 발생 하는 경우를 제어 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 847E69D1-6AE0-4E82-B9C8-919E009C2014
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
ms.openlocfilehash: df84cf01909cec564edc9c6c8bb55382a2b9dfe3
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77646691"
---
# <a name="datepicker-item-selection-on-ios"></a>IOS에서 DatePicker 항목 선택

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼 특정 컨트롤은 [`DatePicker`](xref:Xamarin.Forms.DatePicker)에서 항목 선택이 발생할 때 컨트롤에서 항목을 검색할 때 또는 **완료** 단추를 누른 경우에만 항목 선택이 발생 하도록 지정할 수 있습니다. `DatePicker.UpdateMode` 연결 된 속성을 `UpdateMode` 열거형 값으로 설정 하 여 XAML에서 사용 됩니다.

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
       <DatePicker MinimumDate="01/01/2020"
                   MaximumDate="12/31/2020"
                   ios:DatePicker.UpdateMode="WhenFinished" />
       ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

datePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`DatePicker.On<iOS>` 메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스의 `DatePicker.SetUpdateMode` 메서드는 다음 두 가지 가능한 값을 제공 하는 `UpdateMode` 열거를 사용 하 여 항목 선택이 발생 하는 시기를 제어 하는 데 사용 됩니다.

- `Immediately`-사용자가 [`DatePicker`](xref:Xamarin.Forms.DatePicker)항목을 탐색할 때 항목 선택이 발생 합니다. 이것이 Xamarin.Forms의 기본 동작입니다.
- `WhenFinished`-사용자가 [`DatePicker`](xref:Xamarin.Forms.DatePicker)에서 **완료** 단추를 누르면 항목 선택이 발생 합니다.

또한 `SetUpdateMode` 메서드를 사용 하 여 현재 `UpdateMode`를 반환 하는 `UpdateMode` 메서드를 호출 하 여 열거형 값을 토글할 수 있습니다.

```csharp
switch (datePicker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        datePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        datePicker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

그러면 지정 된 `UpdateMode`가 [`DatePicker`](xref:Xamarin.Forms.DatePicker)에 적용 되어 항목 선택이 발생 하는 시기를 제어 합니다.

[![DatePicker 업데이트 모드의 스크린샷](datepicker-selection-images/datepicker-updatemode.png "DatePicker UpdateMode 플랫폼 관련")](datepicker-selection-images/datepicker-updatemode-large.png#lightbox "DatePicker UpdateMode 플랫폼 관련")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
