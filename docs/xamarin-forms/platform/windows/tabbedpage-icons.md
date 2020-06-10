---
제목: "Windows의 TabbedPage 아이콘" 설명: "플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 TabbedPage 도구 모음에 페이지 아이콘을 표시 하는 데 사용 되는 Windows 플랫폼별를 사용 하는 방법을 설명 합니다.
assetid: 7C5031A5-74EE-4469-994E-BEA7BA9D33CB ms. 기술: xamarin-forms author: davidbritch ms. author: dabritch. 날짜: 10/24/2018 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="tabbedpage-icons-on-windows"></a>Windows에서 아이콘 TabbedPage

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 유니버설 Windows 플랫폼 플랫폼별를 사용 하 여 페이지 아이콘을 도구 모음에 표시 하 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 고 필요에 따라 아이콘 크기를 지정할 수 있는 기능을 제공 합니다. 연결 된 속성을로 설정 하 [`TabbedPage.HeaderIconsEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsEnabledProperty) `true` 고 필요 [`TabbedPage.HeaderIconsSize`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsSizeProperty) 에 따라 연결 된 속성을 값으로 설정 하 여 XAML에서 사용 됩니다 [`Size`](xref:Xamarin.Forms.Size) .

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:TabbedPage.HeaderIconsEnabled="true">
    <windows:TabbedPage.HeaderIconsSize>
        <Size>
            <x:Arguments>
                <x:Double>24</x:Double>
                <x:Double>24</x:Double>
            </x:Arguments>
        </Size>
    </windows:TabbedPage.HeaderIconsSize>
    <ContentPage Title="Todo" IconImageSource="todo.png">
        ...
    </ContentPage>
    <ContentPage Title="Reminders" IconImageSource="reminders.png">
        ...
    </ContentPage>
    <ContentPage Title="Contacts" IconImageSource="contacts.png">
        ...
    </ContentPage>
</TabbedPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

public class WindowsTabbedPageIconsCS : Xamarin.Forms.TabbedPage
{
  public WindowsTabbedPageIconsCS()
  {
    On<Windows>().SetHeaderIconsEnabled(true);
    On<Windows>().SetHeaderIconsSize(new Size(24, 24));

    Children.Add(new ContentPage { Title = "Todo", IconImageSource = "todo.png" });
    Children.Add(new ContentPage { Title = "Reminders", IconImageSource = "reminders.png" });
    Children.Add(new ContentPage { Title = "Contacts", IconImageSource = "contacts.png" });
  }
}
```

`TabbedPage.On<Windows>`메서드는이 플랫폼별가 유니버설 Windows 플랫폼 에서만 실행 되도록 지정 합니다. [ `TabbedPage.SetHeaderIconsEnabled` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. SetHeaderIconsEnabled ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . TabbedPage}, system.string) 메서드를 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 사용 하 여 헤더 아이콘을 설정 하거나 해제할 수 있습니다. [ `TabbedPage.SetHeaderIconsSize` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. SetHeaderIconsSize ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . TabbedPage}, Xamarin.Forms . Size) 메서드는 선택적으로 값을 사용 하 여 머리글 아이콘 크기를 지정 합니다 [`Size`](xref:Xamarin.Forms.Size) .

또한 네임 스페이스의 클래스에는 헤더 아이콘을 사용 하는 메서드, 헤더 아이콘을 사용 하지 않도록 설정 하는 메서드 `TabbedPage` `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` [`EnableHeaderIcons`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.EnableHeaderIcons*) [`DisableHeaderIcons`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.DisableHeaderIcons*) 및 [`IsHeaderIconsEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.IsHeaderIconsEnabled*) `boolean` 머리글 아이콘이 설정 되었는지 여부를 나타내는 값을 반환 하는 메서드가 있습니다.

그러면 페이지 아이콘이 도구 모음에 표시 될 수 있으며 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 아이콘 크기는 선택적으로 원하는 크기로 설정 됩니다.

![TabbedPage 아이콘 사용 플랫폼 관련](tabbedpage-icons-images/tabbedpage-icons.png "TabbedPage 아이콘 사용 플랫폼 관련")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
