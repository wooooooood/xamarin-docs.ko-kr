---
제목: "Windows에서 SearchBar Spell Check" 설명: "플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 SearchBar가 맞춤법 검사 엔진과 상호 작용할 수 있도록 하는 Windows 플랫폼 관련 기능을 사용 하는 방법을 설명 합니다. "
assetid: 7974C91F-7502-4DB3-B0E9-C45E943DDA26: xamarin-forms author: davidbritch: dabritch:: 10/24/2018-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="searchbar-spell-check-on-windows"></a>Windows의 SearchBar Spell Check

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 유니버설 Windows 플랫폼 플랫폼별는를 사용 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 하 여 맞춤법 검사 엔진과 상호 작용할 수 있습니다. 연결 된 속성을 값으로 설정 하 여 XAML에서 사용 됩니다 [`SearchBar.IsSpellCheckEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) `boolean` .

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

`SearchBar.On<Windows>`메서드는이 플랫폼별가 유니버설 Windows 플랫폼 에서만 실행 되도록 지정 합니다. [ `SearchBar.SetIsSpellCheckEnabled` ] (F: Xamarin.Forms 입니다. PlatformConfiguration Xamarin.Forms ()를 지정 합니다. IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . SearchBar}, system.string) 메서드는 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 맞춤법 검사기를 설정 및 해제 합니다. 또한 `SearchBar.SetIsSpellCheckEnabled` 메서드를 사용 하 여 [ `SearchBar.GetIsSpellCheckEnabled` ] (f:)를 호출 하 여 맞춤법 검사기를 전환할 수 있습니다 Xamarin.Forms . PlatformConfiguration Xamarin.Forms ()를 지정 합니다. IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . SearchBar}) 메서드를 사용 하 여 맞춤법 검사기를 사용 하는지 여부를 반환 합니다.

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

결과적으로에 입력 한 텍스트는 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 맞춤법을 검사할 수 있으며 사용자에 게 잘못 된 철자가 표시 됩니다.

![SearchBar 맞춤법 검사 플랫폼별](searchbar-spell-check-images/searchbar-spellcheck.png "SearchBar 맞춤법 검사 플랫폼별")

> [!NOTE]
> `SearchBar`네임 스페이스의 클래스에 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) [`EnableSpellCheck`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) [`DisableSpellCheck`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) 는 각각의 맞춤법 검사기를 사용 하거나 사용 하지 않도록 설정 하는 데 사용할 수 있는 및 메서드가 있습니다 `SearchBar` .

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
