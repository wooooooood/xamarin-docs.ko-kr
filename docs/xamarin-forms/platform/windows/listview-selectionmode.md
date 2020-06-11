---
제목: "Windows의 ListView SelectionMode" 설명: "플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 ListView의 항목이 탭 제스처에 응답할 수 있는지 여부를 제어 하는 Windows 플랫폼 관련 기능을 사용 하는 방법을 설명 합니다.
assetid: 57EF3A7F-1407-4B31-AE21-D149293D4228: xamarin-forms author: davidbritch: dabritch:: 10/24/2018-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="listview-selectionmode-on-windows"></a>Windows의 ListView SelectionMode

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

유니버설 Windows 플랫폼에서 기본적으로는 네이티브 이벤트를 사용 하 여 Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) `ItemClick` 네이티브 이벤트 대신 상호 작용에 응답 합니다 `Tapped` . 그러면 Windows 내레이터와 키보드에서과 상호 작용할 수 있도록 접근성 기능이 제공 `ListView` 됩니다. 그러나이 도구는 작동 하지 않는 내부에서 탭 제스처를 렌더링 하기도 `ListView` 합니다.

이 유니버설 Windows 플랫폼 플랫폼별는의 항목이 [`ListView`](xref:Xamarin.Forms.ListView) 탭 제스처에 응답할 수 있는지 여부와 네이티브가 또는 이벤트를 발생 시키는 지 여부를 제어 합니다 `ListView` `ItemClick` `Tapped` . [`ListView.SelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty)연결 된 속성을 열거형의 값으로 설정 하 여 XAML에서 사용 됩니다 [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) .

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

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

`ListView.On<Windows>`메서드는이 플랫폼별가 유니버설 Windows 플랫폼 에서만 실행 되도록 지정 합니다. [ `ListView.SetSelectionMode` ] (F: Xamarin.Forms 입니다. PlatformConfiguration ( Xamarin.Forms 입니다. IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . ListView}, Xamarin.Forms . ListViewSelectionMode) 메서드는 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 네임 스페이스에서의 항목이 탭 제스처에 응답할 수 있는지 여부를 제어 하는 데 사용 되며, [`ListView`](xref:Xamarin.Forms.ListView) [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) 열거형은 두 가지 가능한 값을 제공 합니다.

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible)–에서 `ListView` `ItemClick` 상호 작용을 처리 하는 기본 이벤트를 발생 시키고 액세스 가능성 기능을 제공 함을 나타냅니다. 따라서 Windows 내레이터와 키보드는와 상호 작용할 수 있습니다 `ListView` . 그러나의 항목은 `ListView` 탭 제스처에 응답할 수 없습니다. 이는 유니버설 Windows 플랫폼 인스턴스에 대 한 기본 동작입니다 `ListView` .
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible)–가 `ListView` `Tapped` 상호 작용을 처리 하기 위해 네이티브 이벤트를 발생 시키는 것을 나타냅니다. 따라서의 항목은 `ListView` 탭 제스처에 응답할 수 있습니다. 그러나 접근성 기능이 없으므로 Windows 내레이터와 키보드는와 상호 작용할 수 없습니다 `ListView` .

> [!NOTE]
> `Accessible`및 `Inaccessible` 선택 모드는 함께 사용할 수 없으며, [`ListView`](xref:Xamarin.Forms.ListView) `ListView` 탭 제스처에 응답할 수 있는 액세스 가능한 또는를 선택 해야 합니다.

또한 [ `GetSelectionMode` ] (f: Xamarin.Forms 입니다. PlatformConfiguration ( Xamarin.Forms 입니다. IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . ListView}) 메서드를 사용 하 여 현재를 반환할 수 있습니다 [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) .

그 결과, 지정 된가에 [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) 적용 되어의 [`ListView`](xref:Xamarin.Forms.ListView) 항목이 `ListView` 탭 제스처에 응답할 수 있는지 여부와 네이티브가 `ListView` `ItemClick` 또는 이벤트를 발생 `Tapped` 시키는 지 여부를 제어 합니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
