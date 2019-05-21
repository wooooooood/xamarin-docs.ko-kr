---
title: Windows에서 VisualElement 액세스 키
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서는 VisualElement에 대 한 액세스 키를 지정 하는 Windows 플랫폼별을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 771AF785-76B8-4372-89F5-E4D521D21E0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: c480f398c37ce43b634e0ec1c955b965466757f1
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926845"
---
# <a name="visualelement-access-keys-on-windows"></a>Windows에서 VisualElement 액세스 키

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

액세스 키는 터치를 통해 대신 키보드를 통해 앱의 표시 되는 UI를 사용 하 여 사용자가 신속 하 게 이동 하 고 상호 작용 하는 직관적인 방법을 제공 하 여 유용성 및 유니버설 Windows 플랫폼 (UWP)에서 앱의 액세스 가능성을 개선 하는 바로 가기 키 또는 마우스입니다. 이러한는 일반적으로 순차적으로 누르면 하나 이상의 영숫자 키 및 Alt 키의 조합입니다. 바로 가기 키는 단일 영문자를 사용 하는 액세스 키에 대 한 자동으로 지원 됩니다.

액세스 키 팁 액세스 키를 포함 하는 컨트롤 옆에 표시 되는 배지 부동 됩니다. 각 액세스 키 팁에는 연결된 된 컨트롤을 활성화 하는 영숫자 키를 포함 합니다. 사용자가 Alt 키를 누를 때 액세스 키 팁 표시 됩니다.

이 UWP 플랫폼별을 사용 하 여에 대 한 액세스 키를 지정 하는 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 설정 하 여 XAML에서 사용 되는 [ `VisualElement.AccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyProperty) 영숫자 값을 하 고 필요에 따라 설정 하 여 연결 된 속성을 [ `VisualElement.AccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyPlacementProperty) 합니다 로연결된속성[ `AccessKeyPlacement` ](xref:Xamarin.Forms.AccessKeyPlacement) 열거형을 [ `VisualElement.AccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyHorizontalOffsetProperty) 연결 된 속성을 `double`, 및 [ `VisualElement.AccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyVerticalOffsetProperty) 연결 된 속성을 `double`:

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

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

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

`VisualElement.On<Windows>` 메서드가 플랫폼별 유니버설 Windows 플랫폼에만 실행 되도록 지정 합니다. 합니다 [ `VisualElement.SetAccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.String)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 네임 스페이스에 대 한 액세스 키 값을 설정 하는 `VisualElement`합니다. [ `VisualElement.SetAccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},Xamarin.Forms.AccessKeyPlacement)) 메서드를 사용 하 여 액세스 키 팁을 표시 하기 위해 사용할 위치를 필요에 따라 지정 된 [ `AccessKeyPlacement` ](xref:Xamarin.Forms.AccessKeyPlacement) 은 다음 값을 제공 하는 열거형:

- [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto) – 운영 체제에서 액세스 키 팁 배치를 결정 함을 나타냅니다.
- [`Top`](xref:Xamarin.Forms.AccessKeyPlacement.Top) -액세스 키 팁의 위쪽 가장자리 위에 표시 됨을 나타냅니다는 `VisualElement`합니다.
- [`Bottom`](xref:Xamarin.Forms.AccessKeyPlacement.Bottom) -액세스 키 팁의 아래쪽 가장자리 아래에 표시 됩니다는 나타냅니다는 `VisualElement`합니다.
- [`Right`](xref:Xamarin.Forms.AccessKeyPlacement.Right) -액세스 키 팁의 오른쪽 가장자리의 오른쪽에 표시 됨을 나타냅니다는 `VisualElement`합니다.
- [`Left`](xref:Xamarin.Forms.AccessKeyPlacement.Left) -액세스 키 팁의 왼쪽된 가장자리의 왼쪽에 표시 됨을 나타냅니다는 `VisualElement`합니다.
- [`Center`](xref:Xamarin.Forms.AccessKeyPlacement.Center) –의 가운데에 대 한 액세스 키 팁 겹쳐 표시 됩니다 나타냅니다는 `VisualElement`합니다.

> [!NOTE]
> 일반적으로 [ `Auto` ](xref:Xamarin.Forms.AccessKeyPlacement.Auto) 적응형 사용자 인터페이스에 대 한 지원을 포함 하는 키 팁 배치는 충분 합니다.

[ `VisualElement.SetAccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double)) 하 고 [ `VisualElement.SetAccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double)) 메서드 액세스 키 팁 위치 보다 세부적으로 제어에 사용할 수 있습니다. 인수는 `SetAccessKeyHorizontalOffset` 메서드 또는 오른쪽 하단으로 떨어져 있는 액세스 키 팁 왼쪽 이동 하는 방법 및 인수를 나타냅니다는 `SetAccessKeyVerticalOffset` 메서드 액세스 키 팁을 위 또는 아래로 이동 하는 정도 나타냅니다.

>[!NOTE]
> 액세스 키 배치 설정 된 경우 액세스 키 팁 오프셋을 설정할 수 없습니다 `Auto`합니다.

또한 합니다 [ `GetAccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement}))를 [ `GetAccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement}))를 [ `GetAccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), 및 [ `GetAccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) 메서드를 사용할 수 있습니다 액세스 키를 검색할 키 값과의 위치입니다.

결과 옆에 있는 키 팁에 대 한 액세스를 표시할 수 있습니다 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Alt 키를 눌러 액세스 키를 정의 하는 인스턴스:

![VisualElement 액세스 키 플랫폼별](visualelement-access-keys-images/visualelement-accesskeys.png "VisualElement 액세스 플랫폼 전용 키")

사용자 액세스를 뒤 Alt 키를 눌러 액세스 키를 활성화 하는 경우 키에 대 한 기본 작업을 `VisualElement` 실행 됩니다. 예를 들어, 활성화 하면 액세스 키에는 [ `Switch` ](xref:Xamarin.Forms.Switch)는 `Switch` 설정/해제 합니다. 사용자의 액세스 키를 활성화 하는 경우는 [ `Entry` ](xref:Xamarin.Forms.Entry)는 `Entry` 포커스를 얻습니다. 사용자의 액세스 키를 활성화 하는 경우는 [ `Button` ](xref:Xamarin.Forms.Button)에 대 한 이벤트 처리기를 [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) 이벤트가 실행 됩니다.

액세스 키에 대 한 자세한 내용은 참조 하세요. [선택키가](/windows/uwp/design/input/access-keys#key-tip-positioning)합니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
