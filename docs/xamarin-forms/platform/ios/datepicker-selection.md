---
제목: "iOS에서 DatePicker 항목 선택" 설명: "플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 DatePicker에서 항목 선택이 발생 하는 경우를 제어 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
assetid: 847E69D1-6AE0-4E82-B9C8-919E009C2014: xamarin-forms author: davidbritch: dabritch:: 01/15/2020-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="datepicker-item-selection-on-ios"></a>IOS에서 DatePicker 항목 선택

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼 특정 컨트롤은에서 항목 선택을 수행 하는 경우 [`DatePicker`](xref:Xamarin.Forms.DatePicker) 사용자가 컨트롤에서 항목을 검색할 때 또는 **완료** 단추를 누른 경우에만 항목 선택이 발생 하도록 지정할 수 있도록 합니다. `DatePicker.UpdateMode`연결 된 속성을 열거형의 값으로 설정 하 여 XAML에서 사용 됩니다 `UpdateMode` .

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

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

datePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`DatePicker.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. `DatePicker.SetUpdateMode`네임 스페이스의 메서드는 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) `UpdateMode` 다음 두 가지 가능한 값을 제공 하는 열거형을 사용 하 여 항목 선택이 발생 하는 시기를 제어 하는 데 사용 됩니다.

- `Immediately`– 항목 선택은 사용자가의 항목을 탐색할 때 발생 합니다 [`DatePicker`](xref:Xamarin.Forms.DatePicker) . 이는의 기본 동작입니다 Xamarin.Forms .
- `WhenFinished`– 항목 선택은 사용자가에서 [ **완료** ] 단추를 누른 경우에만 발생 합니다 [`DatePicker`](xref:Xamarin.Forms.DatePicker) .

또한 메서드를 `SetUpdateMode` 호출 하 여 `UpdateMode` 현재을 반환 하는 메서드를 호출 하 여 열거형 값을 전환 하는 데 메서드를 사용할 수 있습니다 `UpdateMode` .

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

그러면 지정 된가에 `UpdateMode` 적용 되어 [`DatePicker`](xref:Xamarin.Forms.DatePicker) 항목 선택이 발생 하는 시기를 제어 합니다.

[![DatePicker 업데이트 모드의 스크린샷](datepicker-selection-images/datepicker-updatemode.png "DatePicker UpdateMode 플랫폼 관련")](datepicker-selection-images/datepicker-updatemode-large.png#lightbox "DatePicker UpdateMode 플랫폼 관련")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
