---
제목: "iOS의 SearchBar 스타일" 설명: "플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 SearchBar의 배경이 있는지 여부를 제어 하는 iOS 플랫폼 관련 기능을 사용 하는 방법을 설명 합니다. "
assetid: 3D512DD6-078E-4BC6-926E-62BA6F4DE640: xamarin-forms author: davidbritch: dabritch:: 03/05/2020-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="searchbar-style-on-ios"></a>IOS의 SearchBar 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼 특정은에 배경이 있는지 여부를 제어 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 합니다. `SearchBar.SearchBarStyle`바인딩 가능한 속성을 열거형의 값으로 설정 하 여 XAML에서 사용 됩니다 `UISearchBarStyle` .

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ios:SearchBar.SearchBarStyle="Minimal"
                   Placeholder="Enter search term" />
        ...
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

SearchBar searchBar = new SearchBar { Placeholder = "Enter search term" };
searchBar.On<iOS>().SetSearchBarStyle(UISearchBarStyle.Minimal);
```

`SearchBar.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. `SearchBar.SetSearchBarStyle`네임 스페이스의 메서드는 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 이 배경을 사용 하는지 여부를 제어 하는 데 사용 됩니다 [`SearchBar`](xref:Xamarin.Forms.SearchBar) . `UISearchBarStyle`열거형은 세 가지 가능한 값을 제공 합니다.

- `Default`에 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 기본 스타일이 있음을 나타냅니다. 이 값은 바인딩 가능한 속성의 기본값입니다 `SearchBar.SearchBarStyle` .
- `Prominent`에 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 반투명 배경이 있고 검색 필드가 불투명 함을 나타냅니다.
- `Minimal`에 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 배경이 없고 검색 필드가 반투명 임을 나타냅니다.

또한 메서드를 사용 하 여에 `SearchBar.GetSearchBarStyle` 적용 되는를 반환할 수 있습니다 `UISearchBarStyle` `SearchBar` .

그러면 지정 된 `UISearchBarStyle` 멤버가에 적용 되어 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 의 배경이 있는지 여부를 제어 합니다 `SearchBar` .

![IOS의 SearchBar 스타일 스크린샷](searchbar-style-images/searchbar-styles.png "IOS의 SearchBar 스타일")

다음 스크린샷에서는 `UISearchBarStyle` [`SearchBar`](xref:Xamarin.Forms.SearchBar) 속성이 설정 된 개체에 적용 되는 멤버를 보여 줍니다 `BackgroundColor` .

![IOS에서 배경색을 사용 하는 SearchBar 스타일의 스크린샷](searchbar-style-images/searchbar-background-styles.png "IOS의 배경색을 사용 하는 SearchBar 스타일")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
