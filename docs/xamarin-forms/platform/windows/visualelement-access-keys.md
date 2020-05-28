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
ms.openlocfilehash: 1bfd61e79a2b4697e884afb45e4b9080ee939b87
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136528"
---
# <a name="visualelement-access-keys-on-windows"></a>Windows의 VisualElement 액세스 키

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

액세스 키는 사용자가 터치 또는 마우스를 통하지 않고 키보드를 통해 앱의 시각적 UI를 신속 하 게 탐색 하 고 상호 작용할 수 있는 직관적인 방법을 제공 하 여 유니버설 Windows 플랫폼 (UWP)에서 앱의 유용성 및 접근성을 향상 시키는 바로 가기 키입니다. 이는 Alt 키와 하나 이상의 영숫자 키의 조합으로, 일반적으로 순차적으로 눌러져 있습니다. 키보드 바로 가기 키는 단일 영숫자 문자를 사용 하는 선택키가 자동으로 지원 됩니다.

액세스 키 팁은 선택 키를 포함 하는 컨트롤 옆에 표시 되는 부동 배지입니다. 각 액세스 키 팁은 연결 된 컨트롤을 활성화 하는 영숫자 키를 포함 합니다. 사용자가 Alt 키를 누르면 선택 키 팁이 표시 됩니다.

이 UWP 플랫폼 전용은에 대 한 액세스 키를 지정 하는 데 사용 됩니다 [`VisualElement`](xref:Xamarin.Forms.VisualElement) . 연결 된 속성을 영숫자 값으로 설정 하 고 필요에 따라 연결 된 속성을 [`VisualElement.AccessKey`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyProperty) 열거형의 값으로 설정 하 고 연결 된 속성을에 연결 하 [`VisualElement.AccessKeyPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyPlacementProperty) [`AccessKeyPlacement`](xref:Xamarin.Forms.AccessKeyPlacement) [`VisualElement.AccessKeyHorizontalOffset`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyHorizontalOffsetProperty) `double` 고 [`VisualElement.AccessKeyVerticalOffset`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyVerticalOffsetProperty) 연결 `double` 된 속성을로 설정 하 여 XAML에서 사용 됩니다.

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <ContentPage Title="Page 1"
                 windows:VisualElement.AccessKey="1">
        <StackLayout Margin="20">
            ...
            <Switch windows:VisualElement.AccessKey="A" />
            <Entry Placeholder="Enter text here"
                   windows:VisualElement.AccessKey="B" />
            ...
            <Button Text="Access key F, placement top with offsets"
                    Margin="20"
                    Clicked="OnButtonClicked"
                    windows:VisualElement.AccessKey="F"
                    windows:VisualElement.AccessKeyPlacement="Top"
                    windows:VisualElement.AccessKeyHorizontalOffset="20"
                    windows:VisualElement.AccessKeyVerticalOffset="20" />
            ...
        </StackLayout>
    </ContentPage>
    ...
</TabbedPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var page = new ContentPage { Title = "Page 1" };
page.On<Windows>().SetAccessKey("1");

var switchView = new Switch();
switchView.On<Windows>().SetAccessKey("A");
var entry = new Entry { Placeholder = "Enter text here" };
entry.On<Windows>().SetAccessKey("B");
...

var button4 = new Button { Text = "Access key F, placement top with offsets", Margin = new Thickness(20) };
button4.Clicked += OnButtonClicked;
button4.On<Windows>()
    .SetAccessKey("F")
    .SetAccessKeyPlacement(AccessKeyPlacement.Top)
    .SetAccessKeyHorizontalOffset(20)
    .SetAccessKeyVerticalOffset(20);
...
```

`VisualElement.On<Windows>`메서드는이 플랫폼별가 유니버설 Windows 플랫폼 에서만 실행 되도록 지정 합니다. [ `VisualElement.SetAccessKey` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. Windowsl. VisualElement ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . 에 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 대 한 액세스 키 값을 설정 하는 데 네임 스페이스의 VisualElement}, system.string) 메서드를 사용 합니다 `VisualElement` . [ `VisualElement.SetAccessKeyPlacement` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. SetAccessKeyPlacement ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . VisualElement}, Xamarin.Forms . AccessKeyPlacement) 메서드는 선택적으로 [`AccessKeyPlacement`](xref:Xamarin.Forms.AccessKeyPlacement) 다음과 같은 가능한 값을 제공 하는 열거형을 사용 하 여 액세스 키 팁을 표시 하는 데 사용할 위치를 지정 합니다.

- [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto)– 선택 키 팁 배치가 운영 체제에 의해 결정 됨을 나타냅니다.
- [`Top`](xref:Xamarin.Forms.AccessKeyPlacement.Top)– 선택 키 팁이의 위쪽 가장자리 위에 표시 됨을 나타냅니다 `VisualElement` .
- [`Bottom`](xref:Xamarin.Forms.AccessKeyPlacement.Bottom)– 선택 키 팁이의 아래쪽 가장자리 아래에 표시 됨을 나타냅니다 `VisualElement` .
- [`Right`](xref:Xamarin.Forms.AccessKeyPlacement.Right)– 선택 키 팁이의 오른쪽 가장자리 오른쪽에 표시 됨을 나타냅니다 `VisualElement` .
- [`Left`](xref:Xamarin.Forms.AccessKeyPlacement.Left)– 선택 키 팁이의 왼쪽 가장자리 왼쪽에 표시 됨을 나타냅니다 `VisualElement` .
- [`Center`](xref:Xamarin.Forms.AccessKeyPlacement.Center)– 선택 키 팁이의 중앙에 겹쳐서 표시 됨을 나타냅니다 `VisualElement` .

> [!NOTE]
> 일반적으로 [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto) 주요 팁 배치가 충분 합니다. 여기에는 적응 사용자 인터페이스에 대 한 지원이 포함 됩니다.

[ `VisualElement.SetAccessKeyHorizontalOffset` ] (F: Xamarin.Forms 입니다. PlatformConfiguration ( Xamarin.Forms .. IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . VisualElement}, system.string)) 및 [ `VisualElement.SetAccessKeyVerticalOffset` ] (f: Xamarin.Forms . PlatformConfiguration ( Xamarin.Forms .. IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . VisualElement}, System.object) 메서드를 사용 하 여 액세스 키 팁 위치를 보다 세밀 하 게 제어할 수 있습니다. 메서드에 대 한 인수는 `SetAccessKeyHorizontalOffset` 액세스 키 팁을 왼쪽 또는 오른쪽으로 이동 하는 정도를 나타내고, 메서드에 대 한 인수는 `SetAccessKeyVerticalOffset` 액세스 키 팁을 위나 아래로 이동 하는 정도를 나타냅니다.

>[!NOTE]
> 액세스 키 배치가 설정 된 경우에는 액세스 키 팁 오프셋을 설정할 수 없습니다 `Auto` .

또한 [ `GetAccessKey` ] (f: Xamarin.Forms 입니다. PlatformConfiguration. WindowsSpecific. VisualElement. GetAccessKey ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . VisualElement})), [ `GetAccessKeyPlacement` ] (f: Xamarin.Forms . PlatformConfiguration. WindowsSpecific VisualElement. GetAccessKeyPlacement ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . VisualElement})), [ `GetAccessKeyHorizontalOffset` ] (f: Xamarin.Forms . PlatformConfiguration ( Xamarin.Forms .. IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . VisualElement})) 및 [ `GetAccessKeyVerticalOffset` ] (f: Xamarin.Forms . PlatformConfiguration ( Xamarin.Forms .. IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . VisualElement}) 메서드를 사용 하 여 액세스 키 값 및 해당 위치를 검색할 수 있습니다.

그 결과 [`VisualElement`](xref:Xamarin.Forms.VisualElement) , Alt 키를 눌러 액세스 키를 정의 하는 모든 인스턴스 옆에 액세스 키 팁을 표시할 수 있습니다.

![VisualElement 액세스 키 플랫폼별](visualelement-access-keys-images/visualelement-accesskeys.png "VisualElement 액세스 키 플랫폼별")

사용자가 액세스 키를 활성화 하 고 Alt 키를 누른 다음 선택 키를 누르면의 기본 작업이 `VisualElement` 실행 됩니다. 예를 들어 사용자가에 대 한 액세스 키를 활성화 하면 [`Switch`](xref:Xamarin.Forms.Switch) `Switch` 가 전환 됩니다. 사용자가에서 액세스 키를 활성화 하면가 [`Entry`](xref:Xamarin.Forms.Entry) 포커스를 `Entry` 얻게 됩니다. 사용자가에 대 한 액세스 키를 활성화 하면 [`Button`](xref:Xamarin.Forms.Button) 이벤트에 대 한 이벤트 처리기 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 가 실행 됩니다.

액세스 키에 대 한 자세한 내용은 [액세스 키](/windows/uwp/design/input/access-keys#key-tip-positioning)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
